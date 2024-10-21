
---
### Configuration and Security Notes for `sudo` on macOS

#### Table of Contents

- [[#Editing the `sudoers` File]]
	- [[#sudoers File Authentication Configuration]]
	- [[#Managing `sudo` Timeout]]
- [[#Preventing Execution of User's Shell Environment with `sudo`]]
- [[#Troubleshooting `sudoers` File Permissions and Ownership]]
- [[#Sudo with TouchID]]

---
##### Editing the `sudoers` File

To safely edit the `sudoers` file:
1. **Open `sudoers` for Editing:**
   ```bash
   sudo visudo
   ```
2. **Navigate and Edit:**
   - Use arrow keys to navigate.
   - Press `i` to enter insert mode.
   - Make changes (e.g., add `user ALL=(ALL) ALL`).
   - Press `Esc` to exit insert mode.
   - Type `:wq` to save and exit.
3. **Exit Session:**
   - Press `Ctrl + D` if you wish to exit the session.

**Example `sudoers` Configuration:**
```plaintext
root ALL=(ALL) ALL
%admin ALL=(ALL) ALL
Defaults tty_tickets
Defaults timestamp_timeout=1 
```

[sudoers config](https://github.com/drduh/config/blob/master/sudoers)

##### sudoers File Authentication Configuration
Ensure that the `sudoers` file is configured to authenticate users on a per-tty basis.
- **Check Configuration:**
  ```bash
  /usr/bin/sudo /usr/bin/grep tty_tickets /etc/sudoers
  ```

- [STIG Viewer for Apple OS X 10.11](https://www.stigviewer.com/stig/apple_os_x_10.11/2017-04-06/finding/V-67709)
##### Managing `sudo` Timeout
Set `sudo` timeout in `sudoers`: (Number of minutes before sudo pw reqrd)
```plaintext
Defaults timestamp_timeout=1
```
To kill the timeout timestamp for `sudo`:
```bash
sudo -k
```

---
##### Preventing Execution of User's Shell Environment with `sudo`

To avoid running your user's dotfiles as root:

1. **Modify `/etc/sudoers`:**
   - Comment out the following line to stop preserving the `HOME` environment variable:
     ```plaintext
     Defaults env_keep += "HOME MAIL"
     ```

2. **Optional: Set Root's Home Directory:**
   - Add to `/var/root/.bashrc`:
     ```bash
     export HOME=/Users/username
     ```

---
##### Troubleshooting `sudoers` File Permissions and Ownership

**Issue:**
Editing `/etc/sudoers` with Sublime Text may result in the error `{username} is not in the sudoers file.`

**Fix:**

1. **Change Ownership and Permissions:**
   ```bash
   sudo chown root:wheel /etc/sudoers
   sudo chmod 440 /etc/sudoers
   ```

2. **References:**
   - [Fixing Ownership Issues](https://unix.stackexchange.com/a/74145)
   - [Apple Stack Exchange Discussion](https://apple.stackexchange.com/a/394943)
   - [Unix Stack Exchange Discussion](https://unix.stackexchange.com/questions/179954/username-is-not-in-the-sudoers-file-this-incident-will-be-reported)
   - [Apple Discussions Thread](https://discussions.apple.com/thread/255188671?sortBy=best)

---
##### Sudo with TouchID

To enable TouchID authentication for `sudo`:
1. **Edit PAM Configuration:**
   ```bash
   sudo nano /etc/pam.d/sudo
   ```
2. **Add the Following Line:**
   ```
   auth sufficient pam_tid.so
   ```
   - **Note:** This configuration will break `sudo` when SSH'ing into your machine, as TouchID cannot be used remotely.
   
