Nix package manager provides several benefits for managing software packages and environments. Here’s an overview of what installing Nix does and the key features it brings:

### What Nix Installation Does

1. **Installs the Nix Package Manager:**
   - The Nix package manager is installed on your system. This package manager uses a unique approach to handle software packages, ensuring that they are installed in isolation from one another.

2. **Sets Up the Nix Store:**
   - Nix creates a store directory, typically located at `/nix/store`, where all installed packages and their dependencies are kept. This directory is immutable, meaning that once a package is installed, it is not modified.

3. **Provides Nix Commands:**
   - It adds commands such as `nix-env`, `nix-build`, `nix-shell`, and `nix-collect-garbage` to your system. These commands allow you to manage packages, build software from source, and manage development environments.

4. **Configures Environment Variables:**
   - The installation configures environment variables and sets up the necessary paths to integrate Nix with your shell, so that you can use the Nix package manager from the command line.

5. **Creates a User Profile:**
   - Nix sets up a user-specific profile, usually located in `~/.nix-profile`, which is where user-installed packages are symlinked. This allows you to install packages in user space without needing superuser privileges.

See [[#Installation Output]]

---
### Key Features of Nix

1. **Declarative Package Management:**
   - Packages and their dependencies are managed declaratively. You define your desired package environment in a configuration file (e.g., `default.nix` or `shell.nix`), and Nix ensures that your environment matches this specification.

2. **Isolation and Reproducibility:**
   - Nix ensures that each package and its dependencies are isolated from others. This means you can have multiple versions of a package installed simultaneously without conflicts. It also makes environments reproducible, as the exact same configuration will always result in the same environment.

3. **Rollback Capabilities:**
   - Nix allows you to roll back to previous versions of packages and configurations. If an update causes issues, you can easily revert to a known working state.

4. **Per-User Installations:**
   - Packages are installed per-user, meaning you don’t need administrative privileges to install or manage software. Each user can have their own set of packages without affecting the system-wide setup.

5. **Development Environments:**
   - Nix makes it easy to create reproducible development environments. You can specify dependencies and environment settings in a `shell.nix` file, and then use `nix-shell` to enter an environment where these dependencies are available.

6. **Multi-Platform Support:**
   - Nix can be used on various operating systems including Linux, macOS, and Windows (via WSL). This makes it a versatile tool for cross-platform development.

#### [More Features](https://github.com/dustinlyons/nixos-config?tab=readme-ov-file#features)
* **Managed Homebrew**: Zero maintenance homebrew environment with `nix-darwin` and `nix-homebrew`
- **Secrets Management**: Declarative secrets with `agenix` for SSH, PGP, syncthing, and other tools
- **Built In Home Manager**: `home-manager` module for seamless configuration (no extra clunky CLI steps)

---

### How to Use Nix

- **Install a Package:**

  ```bash
  nix-env -iA nixpkgs.<package-name>
  ```

  Example:

  ```bash
  nix-env -iA nixpkgs.curl
  ```

- **Search for Packages:**

  ```bash
  nix-env -qaP <search-term>
  ```

  Example:

  ```bash
  nix-env -qaP python
  ```

- **Create and Enter a Development Shell:**

  Create a `shell.nix` file with your dependencies, then run:

  ```bash
  nix-shell
  ```

- **Build a Package from Source:**

  ```bash
  nix-build <path-to-nix-file>
  ```

  Example:

  ```bash
  nix-build default.nix
  ```

- **Update Package List:**

  ```bash
  nix-env -u
  ```


---
### nix on macOS
* [Using nix on osx - discussion](https://www.reddit.com/r/Nix/comments/1cv1vq8/why_would_someone_install_nix_on_a_mac_os/)
* [Installing packages with nix on osx - discussion](https://www.reddit.com/r/NixOS/comments/122xd56/simple_way_to_install_package_using_nix_on_macos/)
* [Home Manager](https://nix-community.github.io/home-manager/)
* [nix config template + step-by-step guide for macOS](https://github.com/dustinlyons/nixos-config)

### Uninstalling
https://nix.dev/manual/nix/2.24/installation/uninstall.html

---

### Installation Output
~/folder/macsetup/_outputs/nixinstall_cmdoutput.rtf

```sh
$ sh <(curl -L https://nixos.org/nix/install)
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100  4267  100  4267    0     0   2405      0  0:00:01  0:00:01 --:--:--  2405
downloading Nix 2.24.4 binary tarball for x86_64-darwin from 'https://releases.nixos.org/nix/nix-2.24.4/nix-2.24.4-x86_64-darwin.tar.xz' to '/var/folders/vy/qdp8xcfj4s3dd5c6d1xp_y3w0000gn/T/nix-binary-tarball-unpack.XXXXXXXXXX.V3vJ5UGI'...

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 17.6M  100 17.6M    0     0  2655k      0  0:00:06  0:00:06 --:--:-- 4061k
**Switching to the Multi-user Installer**
Welcome to the Multi-User Nix Installation

This installation tool will set up your computer with the Nix package
manager. This will happen in a few stages:

1. Make sure your computer doesn't already have Nix. If it does, I
   will show you instructions on how to clean up your old install.

2. Show you what I am going to install and where. Then I will ask
   if you are ready to continue.

3. Create the system users (uids [301..332]) and groups (gid 30000)
   that the Nix daemon uses to run builds. To create system users
   in a different range, exit and run this tool again with
   NIX_FIRST_BUILD_UID set.

4. Perform the basic installation of the Nix files daemon.

5. Configure your shell to import special Nix Profile files, so you
   can use Nix.

6. Start the Nix daemon.

Would you like to see a more detailed list of what I will do?
[y/n] y


I will:

 - make sure your computer doesn't already have Nix files
   (if it does, I will tell you how to clean them up.)
 - create local users (see the list above for the users I'll make)
 - create a local group (nixbld)
 - install Nix in /nix
 - create a configuration file in /etc/nix
 - set up the "default profile" by creating some Nix-related files in
   /var/root
 - back up /etc/bashrc to /etc/bashrc.backup-before-nix
 - update /etc/bashrc to include some Nix configuration
 - back up /etc/zshrc to /etc/zshrc.backup-before-nix
 - update /etc/zshrc to include some Nix configuration
 - create a Nix volume and a LaunchDaemon to mount it
 - create a LaunchDaemon (at /Library/LaunchDaemons/org.nixos.nix-daemon.plist) for nix-daemon

Ready to continue?

[y/n] y


```

