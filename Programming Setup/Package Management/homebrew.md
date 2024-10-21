
---
## XCode

### Installation
- Install Xcode Command Line Tools:
  ```bash
  xcode-select --install
  ```

### Removing Xcode/Command Line Tools
- To remove Xcode and Command Line Tools:
  ```bash
  sudo rm -rf /Library/Developer/CommandLineTools
  ```

---
## Homebrew
[Homebrew Documentation](https://docs.brew.sh) 
### Installation and Configuration

- **Install Homebrew**:
  ```bash
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
  ```
- **Package Install**
  Visit the [Homebrew releases page](https://github.com/Homebrew/brew/releases/latest).
  
- **Install Homebrew in different location**: (see [[#Permissions and Ownership]])
  For installing Homebrew in a different location (within your home folder), follow the instructions in [this article](https://applehelpwriter.com/2018/03/21/how-homebrew-invites-users-to-get-pwned/). 

- **Alternative Install**:
  To untar Homebrew anywhere:
  ```bash
  mkdir homebrew && curl -L https://github.com/Homebrew/brew/tarball/master | tar xz --strip 1 -C homebrew
  eval "$(homebrew/bin/brew shellenv)"
  brew update --force --quiet
  ```

### Configuration Options

- **Turn Off Analytics**:
  ```bash
  brew analytics off
  ```

- **Environment Variables**:
  ```bash
  export HOMEBREW_NO_INSECURE_REDIRECT=1
  export HOMEBREW_NO_ANALYTICS=1
  export HOMEBREW_CASK_OPTS="--require-sha --no-binaries"
  ```
- **Cask Options**
  - `--require-sha` - Require all casks to have a SHA256 checksum.
  - `--no-binaries` - Prevent installation of binaries; requires manual PATH setup post-installation.
[homebrew cask opts](https://github.com/Homebrew/homebrew-cask/blob/HEAD/USAGE.md#options)

- **No Binaries Option**:
  - Prevents installation of binaries; only source builds are installed. For example:
    ```bash
    brew install --cask vscodium --no-binaries
    ```
  - Post-installation, manually add VSCodium binaries to your PATH if needed.

- **No GitHub API Option**:
  ```bash
  HOMEBREW_NO_GITHUB_API=1
  ```
  - Deprecated: Was used to install formulae from Homebrew’s API instead of GitHub.

- **Homebrew Installation from API**
  - **Default Behavior (removed variable)**
  - **Pre-API Mode** (For development or troubleshooting)
    ```sh
    export HOMEBREW_NO_INSTALL_FROM_API=1
    ```

- **HOMEBREW_INSTALL_FROM_API=1**: Uses Homebrew API for formula installations.
	- Note: HOMEBREW_INSTALL_FROM_API and HOMEBREW_NO_INSTALL_FROM_API is only a temporary measure. Once this is merged, HOMEBREW_NO_INSTALL_FROM_API will be the only available option.

- **GITHUB_API_TOKEN**: For GitHub authentication issues, see [this guide](https://gist.github.com/willgarcia/7347306870779bfa664e).
	- [GITHUB_API_TOKEN gist](https://gist.github.com/willgarcia/7347306870779bfa664e)
	- [stackoverflow discussion](https://stackoverflow.com/a/20130816)
	- [github settings tokens](https://github.com/settings/tokens)
	- [Tell git not to use my GitHub account (Keychain) for public repositories](https://apple.stackexchange.com/q/269785)

- **Developer Mode**:
  ```bash
  brew developer
  brew developer off
  ```
- enabled if running dev cmds: [e.g. brew audit](https://github.com/Homebrew/brew/issues/14606#issuecomment-1427147557)
- dev cmds automatically tap the core and cask repos when you use them even if you're only using the API.
- Setting HOMEBREW_INSTALL_FROM_API is not compatible with Homebrew’s developer mode so will error (as Homebrew development needs a full clone).

---
### Taps

- **Manage Taps**:
  ```bash
  brew tap                     # List tapped repositories
  brew tap <tapname>           # Add a tap
  brew untap <tapname>         # Remove a tap
  brew tap-info --installed    # Show information about tapped repositories
  ```

- **Default Behavior**:
  Tapping `homebrew/core` and `homebrew/cask` is generally not necessary unless developing formulae or casks.

- **If `homebrew/core` is missing**:
  ```bash
  export HOMEBREW_NO_INSTALL_FROM_API=1
  brew tap --force homebrew/core
  ```

- **Untap Homebrew Core and Cask (if needed)**
  ```sh
  brew untap homebrew/core
  brew untap homebrew/cask
```


---

### Debugging and Information

- **Check Homebrew Analytics State**:
  ```bash
  brew analytics state
  #output
  InfluxDB analytics are disabled.
  Google Analytics were destroyed.
  ```

- **Show Homebrew and System Configuration**:
  ```bash
  brew config
  ```

- **Summarize Homebrew’s Build Environment**:
  ```bash
  brew --env
  ```
  - man page [brew --env](https://docs.brew.sh/Manpage#environment) 
  - variables [env_config](https://github.com/Homebrew/brew/blob/master/Library/Homebrew/env_config.rb)

- **Show Homebrew’s Shell Environment**:
  ```bash
  brew shellenv
  ```



---

### **List Installed Packages**

To list installed formulae and casks with different criteria, you can use the following commands:

- **List All Installed Formulae and Casks**:
  ```bash
  brew list [options] [installed_formula|installed_cask …]
  ```
  - `--formula`: List only formulae.
  - `--cask`: List only casks.
  - `--full-name`: Print formulae with fully-qualified names.
  - `--versions`: Show version numbers for installed formulae.
  - `--multiple`: Show only formulae with multiple versions installed.
  - `--pinned`: List only pinned formulae.
  - `--installed-on-request`: List the formulae installed on request.
  - `--installed-as-dependency`: List the formulae installed as dependencies.
  - `--poured-from-bottle`: List formulae installed from a bottle.
  - `--built-from-source`: List formulae compiled from source.

### **List Top-Level Packages (Packages that are Not Dependencies)**

- **List Installed Formulae that are Not Dependencies**:
  ```bash
  brew leaves
  ```
  - `-r`, `--installed-on-request`: Only list leaves that were manually installed.
  - `-p`, `--installed-as-dependency`: Only list leaves that were installed as dependencies.

### **List Installed Formulae**

- **List Formulae Installed on Request**:
  ```bash
  brew list --installed-on-request
  ```
  This will list formulae that were installed on request.

- **List Formulae Installed as Dependencies**:
  ```bash
  brew list --installed-as-dependency
  ```
  This will list formulae that were installed as dependencies.

#### Difference between `brew list` and `brew leaves`

##### `brew list` Commands

- **`brew list --installed-on-request`**:
  - **Purpose**: Lists all formulae that were explicitly requested to be installed by the user.
  - **Details**: This includes formulae that you directly installed using commands like `brew install`. These packages are not necessarily top-level; they could be dependencies for other packages.
  - **Example Output**: If you installed `wget` because you needed it, it will appear in this list if it was installed on request.

- **`brew list --installed-as-dependency`**:
  - **Purpose**: Lists all formulae that were installed as dependencies for other formulae.
  - **Details**: This includes packages that were installed automatically as a requirement for other formulae but were not directly requested by you.
  - **Example Output**: If you installed `node` and it required `openssl` as a dependency, `openssl` would appear in this list.

##### `brew leaves` Commands

- **`brew leaves --installed-on-request`**:
  - **Purpose**: Lists top-level formulae that were explicitly requested by the user and are not dependencies for any other formulae.
  - **Details**: This command is useful for identifying formulae you manually installed and that aren't required by any other installed formulae.
  - **Example Output**: If you manually installed `wget` and it is not required by any other formulae, it will appear in this list.

- **`brew leaves --installed-as-dependency`**:
  - **Purpose**: Lists top-level formulae that were installed as dependencies, but it filters for those that were specifically installed as dependencies rather than manually requested.
  - **Details**: This is a more specialized view that helps you see dependencies that you didn't manually install but are still top-level (i.e., not required by any other installed formulae).
  - **Example Output**: If you have a package that was installed as a dependency but is not needed by any other installed formulae, it will show up here if it was installed as a dependency.

##### Summary of Differences

- **`brew list --installed-on-request`** vs. **`brew list --installed-as-dependency`**:
  - `brew list` with `--installed-on-request` or `--installed-as-dependency` shows all packages fitting the criteria regardless of whether they are top-level or not.

- **`brew leaves --installed-on-request`** vs. **`brew leaves --installed-as-dependency`**:
  - `brew leaves` filters top-level packages. `--installed-on-request` shows those top-level packages you manually installed, while `--installed-as-dependency` shows top-level packages that were installed as dependencies.


---

### Dependency Trees and Package Linkage

- **Show Dependencies in Tree Format**:
  ```bash
  brew deps --tree --installed
  ```

- **Show Dependencies Hierarchically**:
  ```bash
  brew deps --include-build --tree $(brew leaves)
  ```

- **Show Dependencies Hierarchically (alias)**:
  ```bash
  alias brewdeps="brew leaves | xargs brew deps --include-build --tree"
  ```

- **Show Dependencies Hierarchically (with annotations)**:
[Marks any build, test, implicit, optional, or recommended dependencies]:
  ```bash
  brew deps --include-build --tree --annotate $(brew leaves)
  ```
 
 * **List all formulas that aren't dependents of any other formulas:**
```bash
%brew deps --formula --for-each $(brew leaves) | sed "s/^.*:/$(tput setaf 4)&$(tput sgr0)/"
```

- **List Installation Dates**:
	- Lists installed packages, sorted by last modified date of the package installation directory, newest to oldest.
  ```bash
  brew ls -lt
  ```
	- With this incantation, sort order can be changed by adding `-U` (creation date) or `-u` (last access date) to the `ls -lt`
```bash
  find "$(brew --cellar)" -type d -maxdepth 0 | xargs ls -lt
  # Creation/Install date
  find "$(brew --cellar)" -type d -maxdepth 0 | xargs ls -ltU
  # Last access date  
  find "$(brew --cellar)" -type d -maxdepth 0 | xargs ls -ltu  
  ```

- **Show Brew Linkage**:
	- For every library that a keg references, print its dylib path followed by the binaries that link to it.
  ```bash
  brew linkage --reverse
  ```

[homebrew dependencies blogpost](https://blog.jpalardy.com/posts/untangling-your-homebrew-dependencies/)


---
### PATH

Homebrew installs itself into /usr/local by default, and asks to be placed first on PATH to prioritize brew installs over system binaries.

Check your PATH to review order and that `brew -prefix` PATH is listed first:
```sh
echo $PATH
echo $(brew --prefix)'/bin:'$(brew --prefix)'/sbin'
```
To place brew formula versions before system binary versions:
```sh
export PATH=$(brew --prefix)/bin:$(brew --prefix)/sbin:$PATH
```

Alternative for .zsh/ .bash shells 
```sh
echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.profile
```
  - **Homebrew Installing Binaries in PATH**: [brew shellenv](https://www.reddit.com/r/MacOS/comments/1da7aqi/is_there_a_way_to_automatically_put_brewinstalled/)

#### brew binaries v system binaries

Homebrew installs binaries in /usr/local/bin, which comes before system directories such as /usr/bin or /usr/sbin in your search path ($PATH). This means it could theoretically install spoofed versions of critical system tools and they would be preferred over the real ones. It also changes ownership of /usr/local/bin to the user, so any process run by that user could install or manipulate binaries. [discussion](https://www.reddit.com/r/PrivacyGuides/comments/vashs2/how_risky_is_adding_homebrew_in_macos/)

Note: having anything in your $PATH ahead of the system bins is a no-no unless it’s also root:wheel protected. See [[#Permissions and Ownership]]  
- **Manage Path**:
  - Rearrange PATH so system directories come before `/usr/local/bin`:
    ```bash
    export PATH="/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:$PATH"
    ```

#### Custom PATH setting 

**System/User Default PATH**:
[https://stackoverflow.com/a/70510488]

  - You can query launchd's current custom PATH setting:
```bash
    launchctl getenv PATH
```

* You can query the default PATH by executing:
```sh
	sysctl -n user.cs_path
```

The `sudo launchctl config user path <...>` command updates `/private/var/db/com.apple.xpc.launchd/config/user.plist`:

```sh
$ cat /private/var/db/com.apple.xpc.launchd/config/user.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>PathEnvironmentVariable</key>
    <string>/opt/homebrew/bin:/usr/bin:/bin:/usr/sbin:/sbin</string>
</dict>
</plist>
```

##### Note about cryptexes in PATH (macOS)
* macos `etc/paths` defaults Ventura (Intel)*** - Cryptexes
* https://apple.stackexchange.com/a/459354 
* https://unix.stackexchange.com/a/725216 
* https://www.reddit.com/r/MacOS/comments/1da7aqi/is_there_a_way_to_automatically_put_brewinstalled 


---
### Permissions and Ownership

See [[homebrew Permissions + Ownership]]


---

###  Uninstall Package

- **Uninstall and Clean Up Package**:
  ```bash
  brew uninstall --zap <package>
  ```
  [brew uninstall pkg & dlt files/folders](https://www.reddit.com/r/MacOS/comments/18zbgav/comment/kglkn8u)

---
### Uninstall Homebrew

[uninstall homebrew official](https://github.com/homebrew/install#uninstall-homebrew)
Run the downloaded script with `/bin/bash uninstall.sh --help` to view more uninstall options.

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"
```

If you want to run the Homebrew uninstaller non-interactively, you can use:
```shell
NONINTERACTIVE=1 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"
```

If you want to to uninstall Homebrew from a specific prefix (e.g. when migrating from Intel to Apple Silicon processors), download the uninstall script and run it with `--path`:
```
curl -fsSLO https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh
/bin/bash uninstall.sh --path /usr/local
```

---

- **Backup and Restore**:
  Backup and restore Homebrew configurations using these scripts and methods:
  [Backup and restore scripts](https://github.com/Jaycedam/mac-setup/blob/main/main.sh)
  [Brew Bundle Restore Backup](https://tomlankhorst.nl/brew-bundle-restore-backup/)
  [brew uninstall/ Brewfile](https://github.com/orgs/Homebrew/discussions/393)
  
---

### Additional Information

- **Homebrew Concepts**:
  - **Package**: Also known as formula; usually CLI software.
  - **Bottle**: Binary program pre-built for specific OS configurations.
  - **Cask**: GUI programs or fonts; installs macOS native applications.
  - **Taps**: Additional repositories containing extra formulae or packages not included in the default Homebrew repository.
[homebrew terminology](https://stackoverflow.com/a/64992043)


- **System/User PATH Settings**
  - Query launchd custom PATH: `launchctl getenv PATH`
  - Default PATH: `sysctl -n user.cs_path`
  - Edit `/private/var/db/com.apple.xpc.launchd/config/user.plist`
  - Config cmd: `sudo launchctl config user path <...>`
  - Remove Customizations: `sudo defaults delete /private/var/db/com.apple.xpc.launchd/config/user.plist PathEnvironmentVariable`





---