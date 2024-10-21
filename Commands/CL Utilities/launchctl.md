[[#`launchctl` Commands and Usage]]
	[[#Basic Commands]]
	[[#Filtering and Searching]]
	[[#Managing System and User Services]]
	[[#Process Info for Specific Services]]
[[#Launch Processes File Locations]]
[[#Additional Resources]]
[[#Services on macOS]]
[[#Investigating Process Invocation on macOS]]

---

## `launchctl` Commands and Usage

### Basic Commands
- **List all jobs:**
  - `launchctl list`
  - `sudo launchctl list` *(requires root privileges; lists only root daemons)*

- **Process Info:**
  - `sudo launchctl procinfo`
  - `sudo launchctl procinfo [PID]` *(replace [PID] with the process ID)*

- **Blame (shows the reason for a launch failure):**
  - `sudo launchctl blame`

- **Print disabled services:**
  - `sudo launchctl print-disabled system`
  - `sudo launchctl print-disabled user/501`

- **Load and Unload Services:**
  - `sudo launchctl unload -w [label]`
  - `sudo launchctl load -w [label]`
  - `launchctl unload -w [label]`
  - `launchctl load -w [label]`

- **Print specific services:**
  - `launchctl print gui/502`
  - `launchctl print system`
  - `launchctl print gui/501/com.company.launchagent.label`
  - `sudo launchctl print system/`
  - `sudo launchctl print system/com.apple.remoted`
  - `sudo launchctl print user/501`

- **Dump State:**
  - `sudo launchctl dumpstate | grep [pattern]`
  - `launchctl dumpstate | grep [pattern]`

- **Cache Output:**
  - `sudo launchctl print-cache > c.txt`

### Filtering and Searching
- **Search for specific patterns:**
  - `launchctl list | grep [pattern]`
  - `sudo launchctl list | grep [pattern]`
  - `launchctl list | egrep -i '[pattern]'`
  - `sudo launchctl list | egrep -i '[pattern]'`
  - `launchctl list | grep remote`
  - `sudo launchctl list | grep remote`
  - `launchctl list | egrep {DESIRED_LABEL}`
  - `launchctl remove {DESIRED_LABEL}`

- **Hide system entries:**
  - `launchctl list | grep -v com.apple`

### Managing System and User Services

- **Disable and Enable User Processes:**
  - Disable:
    - `launchctl disable user/501/homebrew.mxcl.postgresql@15`
    - `launchctl disable user/501/com.apple.iCloudNotificationAgent`
  - Enable:
    - `launchctl enable user/501/com.apple.ManagedClientAgent.enrollagent`
    - `launchctl enable user/501/com.apple.appleseed.seedusaged.postinstall`
    - `launchctl enable user/501/com.apple.ScriptMenuApp`

### Process Info for Specific Services
- **Transparency Service:**
  - `launchctl list com.apple.transparencyd`
  - `launchctl print gui/501/com.apple.transparencyd`
  - `sudo launchctl procinfo 497`
  - `sudo defaults read /System/Library/LaunchAgents/com.apple.transparencyd.plist`
  - `defaults read ~/Library/Preferences/com.apple.transparencyd.plist`

Here's the additional information for your notes under [[#`launchctl` Commands and Usage]] and [[#Services on macOS]]:

---
### Launch Processes File Locations
- **Per-user agents provided by the user:**
  - `~/Library/LaunchAgents`
- **Per-user agents provided by the administrator:**
  - `/Library/LaunchAgents`
- **System-wide daemons provided by the administrator:**
  - `/Library/LaunchDaemons`
- **OS X Per-user agents:**
  - `/System/Library/LaunchAgents`
- **OS X System-wide daemons:**
  - `/System/Library/LaunchDaemons`

- **Listing Launch Processes:**
  - [Listing Launch Processes Guide](https://gist.github.com/tosunkaya/49b6e57b667067533c17cd5bb3eff0a9)

### Additional Resources
- **Cheatsheet:**
  - [Launchctl Cheatsheet](https://gist.github.com/masklinn/a532dfe55bdeab3d60ab8e46ccc38a68)

- **RunAtLoad and KeepAlive:**
  - [Superuser Guide](https://superuser.com/a/1334488) 
  - [ServiceMan Tool](https://webinstall.dev/serviceman/)

- **Malware Persistence:**
  - [Stack Overflow](https://stackoverflow.com/a/62159293)
  - [SentinelOne Blog](https://www.sentinelone.com/blog/how-malware-persists-on-macos/)

- **Parsing Background Items:**
  - [Background Items Parser](https://github.com/mnrkbys/bgiparser)

---

[Reference to macOS Services Overview](https://github.com/drduh/macOS-Security-and-Privacy-Guide/tree/master?tab=readme-ov-file#services)
### Services on macOS

Services on macOS are managed by **launchd**. For detailed information, visit [launchd.info](https://launchd.info/).

To manage and view more information about software running at login, refer to [System Settings](https://support.apple.com/guide/mac-help/change-login-items-settings-mtusr003). For installed System, Quick Look, Finder, and other extensions, see [System Settings](https://support.apple.com/guide/mac-help/change-extensions-settings-mchl8baf92fe).

- **View running user agents:**
  - `launchctl list`

- **View running system daemons:**
  - `sudo launchctl list`

- **Examine a specific service:**
  - `launchctl list com.apple.Maps.mapspushd`

- **Examine job plists:**
  - Use `defaults read` on plists in `/System/Library/LaunchDaemons` and `/System/Library/LaunchAgents`
  - For example: `defaults read /System/Library/LaunchDaemons/com.apple.apsd.plist`

- **Find additional information about binaries:**
  - Use `man` and `strings` commands to learn more about what an agent/daemon does.

  Example:
  ```shell
  defaults read /System/Library/LaunchDaemons/com.apple.apsd.plist
  ```

  Look at the `Program` or `ProgramArguments` section to identify the binary, then use `man` to find more details about it.

- **Check service statuses:**
  - `find /var/db/com.apple.xpc.launchd/ -type f -print -exec defaults read {} \; 2>/dev/null`

- **Important Note:**
  - System services are protected by System Integrity Protection (SIP). Avoid disabling SIP solely to tinker with system services, as SIP is a critical part of macOS security. Disabling system services may lead to system instability.

For more information about `launchd` and login items, refer to [Apple's guide](https://support.apple.com/guide/terminal/script-management-with-launchd-apdc6c1077b-5d5d-4d35-9c19-60f2397b2369).

See Also [[Launch Services (db)]]

---

See Also [[Investigating Process Invocation on macOS]]
