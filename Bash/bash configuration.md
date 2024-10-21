Here’s the revised and organized format for the `bash_configuration.md` notes, with modifications and clarifications where necessary:

---

# Bash Configuration Notes

### Index

- [[#Primary Prompts]]
- [[#Colors and Formatting]]
- [[#Prompt Customization]]
- [[#File Configuration]]
- [[#Logout Actions]]
- [[#Shopt Builtin]]
- [[#Set Builtin]]
- [[#Resources]]

---

## Primary Prompts

The `PS1` variable sets the primary prompt displayed before each command. Here are various examples:

- **Basic Prompt:**
  ```bash
  PS1='\h:\W \u\$ '
  ```
  - `\h`: Hostname up to the first dot (e.g., MacBook from MacBook-Pro.local).
  - `\W`: Current working directory (just the last part, e.g., Documents).
  - `\u`: Username of the current user.
  - `\$`: Displays `$` for normal users and `#` for the root user.
  - **Example Output:** `MacBook:Documents username$`

- **Colored Prompt with Hostname and Directory:**
  ```bash
  PS1='\[\e[0;33m\]\h:\W \u\$\[\e[m\] '
  ```
  - `\[\e[0;33m\]`: Sets the color to yellow.
  - **Example Output:** `MyMac:Documents alice$`

- **Colored Prompt with Full Path:**
  ```bash
  PS1=" \[\e[34m\]\u@\h \[\e[33m\]\w\[\e[00m\] $ "
  ```
  - `\[\e[34m\]`: Sets the color to blue for username and hostname.
  - `\[\e[33m\]`: Sets the color to yellow for the current directory.
  - `\[\e[00m\]`: Resets color to default.
  - **Example Output:** `username@MacBook ~/Documents $`

- **Prompt with Timestamp:**
  ```bash
  PS1="\[\e[32m\]\u@\h \[\e[33m\]\$(date +%F\ %T)\[\e[0m\]:\w\$ "
  ```
  - Adds the current date and time (formatted as YYYY-MM-DD HH:MM:SS).
  - **Example Output:** `username@hostname 2024-08-09 10:15:32 ~/Documents $`

- **Prompt with Colored Timestamp:**
  ```bash
  PS1="\[\e[34m\]\u@\h \[\e[32m\]\$(date +%F\ %T) \[\e[33m\]\w\[\e[00m\] \$ "
  ```
  - `\[\e[34m\]`: Sets the text color to blue.
  - `\[\e[32m\]`: Sets the text color to green for the timestamp.
  - `\[\e[33m\]`: Sets the text color to yellow for the directory.
  - **Example Output:** `username@hostname 2024-08-09 10:15:32 ~/Documents $`

---

## Colors and Formatting

- **Basic Color Codes:**
  ```bash
  COLOR_RED="\033[0;31m"
  COLOR_YELLOW="\033[0;33m"
  COLOR_GREEN="\033[0;32m"
  COLOR_OCHRE="\033[38;5;95m"
  COLOR_BLUE="\033[0;34m"
  COLOR_WHITE="\033[0;37m"
  COLOR_RESET="\033[0m"
  COLOR_INVERT="\033[7m"
  ```

- **Examples:**
  ```bash
  # Solid YELLOW for user and directory
  PS1='\[\e[0;33m\]\h:\W \u\$\[\e[m\] '
  
  # Solid BLUE for user, full path YELLOW for directory
  PS1=" \[\033[34m\]\u@\h \[\033[33m\]\w\[\033[31m\]\[\033[00m\] $ "
  ```

---

## Prompt Customization

For more information on prompt customization and advanced formatting, see:

- [Bash Prompt Customization](https://wiki.archlinux.org/title/Bash/Prompt_customization)
- [Bash Colors and Formatting](https://misc.flogisoft.com/bash/tip_colors_and_formatting)

---

## File Configuration

### `/etc/bashrc`

The `/etc/bashrc` file configures system-wide settings for interactive Bash shells.

```bash
#/etc/bashrc

# Check if PS1 is set (interactive shell)
if [ -z "$PS1" ]; then
   return
fi

# Source bash completion scripts
if [ -d "$HOME/.bash_completion.d" ]; then
    for file in "$HOME/.bash_completion.d"/*.bash; do
        [ -r "$file" ] && source "$file"
    done
fi

COLOR_RESET="\e[0m"
COLOR_INVERT="\e[7m"

# Determine shell type
THIS_SHELL_INTERACTIVE_TYPE='non-interactive'
THIS_SHELL_LOGIN_TYPE='non-login'
if tty -s; then THIS_SHELL_INTERACTIVE_TYPE='interactive'; fi
if echo $0 | grep -q -e '^-' 2>/dev/null; then THIS_SHELL_LOGIN_TYPE='login'; fi
printf "${COLOR_INVERT}$THIS_SHELL_INTERACTIVE_TYPE/$THIS_SHELL_LOGIN_TYPE${COLOR_RESET}"

PS1='\h:\W \u\$ '
#PS1='\[\e[0;33m\]\h:\W \u\$\[\e[m\] '
PS1=" \[\e[34m\]\u@\h \[\e[33m\]\w\[\e[31m\]\[\e[00m\] $ "
shopt -s checkwinsize

[ -r "/etc/bashrc_$TERM_PROGRAM" ] && . "/etc/bashrc_$TERM_PROGRAM"

export HISTFILE=/dev/null # Discard history on exit

# Additional environment variables
export PATH="/Library/Frameworks/Python.framework/Versions/3.11/bin:${PATH}"
export PYTHON_BIN_PATH="$(python3 -m site --user-base)/bin"
export PATH="$PYTHON_BIN_PATH:$PATH"
#export PATH="/usr/local/opt/postgresql@15/bin:$PATH"
#export PATH="/usr/local/bin:$PATH"


export PIPENV_VENV_IN_PROJECT=1
export HOMEBREW_NO_INSECURE_REDIRECT=1
export HOMEBREW_NO_ANALYTICS=1
export HOMEBREW_CASK_OPTS="--require-sha"

# Source additional profile
. ~/.bash_profile
```

[interactive/login check source]: https://unix.stackexchange.com/a/522708

### `.bash_completion.d`
Bash completion enhances your command-line experience by providing suggestions for commands, file names, and options as you type. For [`ripgrep`](https://github.com/BurntSushi/ripgrep/blob/bf63fe8f258afc09bae6caa48f0ae35eaf115005/FAQ.md#does-ripgrep-have-support-for-shell-auto-completion), this means:
- **Auto-completion of `rg` options and arguments**: Helps you quickly use available options and file paths without having to remember them all.
```sh
dir="$HOME/.bash_completion.d"
mkdir -p "$dir"
# Example download completions for ripgrep
curl -o "$dir/rg.bash" https://example.com/path/to/rg.bash
# Or generate completions from executable
rg --generate complete-bash > "$dir/rg.bash"
```

more completion scripts
https://github.com/ildar-shaimordanov/colordiff/blob/master/colordiff.bash

### HISTFILE

To discard history on exit:
```bash
export HISTFILE=/dev/null
```
This prevents history from being saved persistently.

---

## Logout Actions

`~/.bash_logout` is only run if it you explicitly exit the shell with `% exit` or `% logout`

To run commands upon exiting a shell, use `trap`. For example, to print a goodbye message:
```bash
print_goodbye() { echo Goodbye; }
trap print_goodbye EXIT
```
This will execute the `print_goodbye` function when the shell exits.
[stackoverflow](https://unix.stackexchange.com/a/424297)

---

## Shopt Builtin

The `shopt` builtin allows for managing shell options.

- **Enable Option:**
  ```bash
  shopt -s huponexit
  ```
  If set, Bash sends `SIGHUP` to all jobs when an interactive login shell exits.

- **Verify Login Shell:**
  ```bash
  shopt login_shell
  ```

- [Shopt Builtin Documentation](https://www.gnu.org/software/bash/manual/bash.html#The-Shopt-Builtin)

---

## Set Builtin

The `set` builtin manages shell options. It can also be used to enable options within a script.

- **Prevent Overwriting Files:**
  ```bash
  set -C noclobber
  ```
  Prevents overwriting of files by redirection.

- **Run Script with "suid":**
  ```bash
  set -p privileged
  ```
  Script runs with "suid" (use with caution).

- **Disable Filename Expansion:**
  ```bash
  set -f noglob
  ```
  Disables filename expansion (globbing).

- [Set Builtin Documentation](https://www.gnu.org/software/bash/manual/bash.html#The-Set-Builtin)
- [set -o Examples](https://gist.github.com/shichao-an/9521462)
- [set Options](https://trvswgnr.github.io/advanced-bash-scripting-guide/options.html)

---

## Resources

- [Example bashrc](https://github.com/mitsuhiko/dotfiles/blob/master/bash/bashrc)
- [Example bashrc with aliases, shopt, set](https://gist.github.com/marcinpohl/9b144ae99c297cfc12e5)
- [Bash Manual](https://www.gnu.org/software/bash/manual/bash.html)
- [Bash Variables](https://www.gnu.org/software/bash/manual/bash.html#Bash-Variables)
- [`.zcompdump / compinit`](https://stackoverflow.com/a/62951409)
- [Example zshrc & aliases](https://github.com/drduh/config/blob/master/zshrc)
- [bash cheatsheet](https://www.zoocoup.org/exhibits/cheatsheets/bash.html )
- [Example zshrc & aliases](https://github.com/drduh/config/blob/master/zshrc)

---

