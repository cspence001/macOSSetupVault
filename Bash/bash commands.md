
- [[#Terminal]]
- [[#Command Listing]]
- [[#Alias Commands]]

[macOS commands](https://tldr.inbrowser.app/pages/osx)

---

## Terminal

- **Split Horizontally:**  
  Press `Command + D`

- **Escapes Horizontal Split:**  
  Press `Command + Shift + D`

- **Automate Terminal Management:**

  - **Open New Terminal or Bring Existing Terminal to Front:**
    - **Using Automator:**
      1. Open Automator.
      2. Select "Quick Action."
      3. Add "Run Shell Script" action.
      4. Enter the command `open -a Terminal`.
      5. Set "Workflow receives" to "no input."
      6. Save as "New Terminal."
      7. Click Automator in the menu bar, navigate to Services (shows "New Terminal" Quick Action), and run it.
      8. Allow Automator to access Terminal.

    - **Create a Keyboard Shortcut:**
      1. Open System Settings.
      2. Go to Keyboard > Keyboard Shortcuts.
      3. Navigate to Services > General.
      4. Add a new shortcut for "New Terminal" by pressing `CTRL + CMD + T`.

---

## Command Listing

- **`compgen` Command:**
  - **List All Commands:**
    ```bash
    compgen -c
    ```
    Lists all commands available to the current user, including built-in commands and executable commands in the directories listed in the PATH environment variable.

  - **List Only Aliases:**
    ```bash
    compgen -a
    ```

  - **List Only Built-in Commands (for Bash):**
    ```bash
    compgen -b
    ```

  - **List Available Keywords:**
    ```bash
    compgen -k
    ```
    Lists keywords used in command-line scripts for Bash.

* **List all commands (executables) in $PATH:**
```bash
#!/bin/bash
# Loop through each directory in PATH
for dir in $(echo $PATH | tr ':' '\n'); do
  echo "Checking directory: $dir"
  if [ -d "$dir" ]; then
    echo "Commands in $dir:"
    # List executable files in the directory
    ls -l "$dir" | grep '^-' | awk '{print $NF}'
    echo
  else
    echo "Directory $dir does not exist or is not accessible."
  fi
done
```

---

- **Push Plain Text Manpage to Clipboard:**
  ```bash
  alias mancopy='man -P "col -b | pbcopy"'
  ```

---
