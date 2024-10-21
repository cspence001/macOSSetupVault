
---

## Table of Contents

1. [[#Firewall Management]]
   - [[#Enable Firewall Daemons and Agents]]
   - [[#Manage Firewall via `socketfilterfw`]]
   - [[#Modify Preferences with `defaults`]]
2. [[#Firewall Check Commands]]
   - [[#Trusted Applications Management]]
3. [[#Global ALF Configuration]]
4. [[#Additional Resources]]


---

## Firewall Management

#### Application Layer Firewall Overview

Enabling the Firewall from the "Security & Privacy" launches

`/usr/libexec/ApplicationFirewall/socketfilterfw `(via `launchd`(8)'s `com.apple.alf.agent.plist`

LaunchDaemon property list. `launchd` redirects the daemon's standard error and output to `/var/log/alf.log`, but by default the logging is performed to `/var/log/appfirewall.log`

Apple documents the Application Firewall basics in a knowledgebase article, naturally not disclosing anything as per its implementation. More detail on the operation of this powerful mechanism can be found in Volume II. In a nutshell, however, the configuration is stored in `/Library/Preferences/com.apple.alf.plist` and may be accessed via the defaults (1) command.

The important keys to note are:

- ﻿﻿`allowsignedenabled`: Allows an automatic exemption to code-signed applications
- `applications`: An array of application identifiers, usually empty
- ﻿﻿`exceptions`: An array of dictionary objects, one for each exception. Applications are identified by their path, and the exception also holds a state integer. 
- `explicitauths`: An array of dictionary objects, each one holding an application (bundle identifier). These are used for interpreters or execution environments, like Python, Ruby, a2p, Java, php, nc, and ksh.
- ﻿﻿`firewall`: A dict of keyed services, which each key containing the process name and the integer state.
- ﻿﻿`firewallunload`: An integer specifying if firewall is unloaded. Must be zero 
- `globalstate`: An integer specifying state. Must be two 
- `loggingenabled`: An integer specifying if alf.log is used - must be one.
- ﻿﻿`loggingoption`: An integer specifying logging flags. May be zero.
- ﻿﻿`stealthenabled`: An integer specifying if host will respond to ICMP (e.g. `ping` (8)).  
    Should be one.

---

**Changes with command-line tool `socketfilterfw` updates preferences in `/Library/Preferences/com.apple.alf`**

Firewall settings cannot be modified from command line on managed Mac computers via `socketfilterfw`. Use `defaults` with `com.apple.alf` plist file for modifying firewall preferences. [[#Manage firewall Preferences with `defaults`]]

### Enable Firewall Daemons and Agents
```bash
sudo launchctl load /System/Library/LaunchDaemons/com.apple.alf.agent.plist
sudo launchctl load /System/Library/LaunchAgents/com.apple.alf.useragent.plist
```

### Manage Firewall via `socketfilterfw`
- **Set Global State On:**
  ```bash
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate on
  ```

- **Set Logging Mode and Options:**
  ```bash
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setloggingmode off
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setloggingopt throttled
  ```

- **Configure Stealth and Blocking:**
  ```bash
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setstealthmode on
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setallowsigned off
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setallowsignedapp off
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setblockall on
  ```

- **Reload Configuration:**
  ```bash
  sudo pkill -HUP socketfilterfw
  ```

### Manage firewall Preferences with `defaults`
```bash
sudo defaults write /Library/Preferences/com.apple.alf globalstate -int 2
sudo defaults write /Library/Preferences/com.apple.alf allowsignedenabled -bool false
sudo defaults write /Library/Preferences/com.apple.alf allowdownloadsignedenabled -bool false
sudo defaults write /Library/Preferences/com.apple.alf loggingenabled -bool false
sudo defaults write /Library/Preferences/com.apple.alf stealthenabled -bool true
```

*  **Check Firewall Settings via defaults:**
```sh
  sudo defaults read /Library/Preferences/com.apple.alf 
```

---

## Firewall Check Commands

- **Check Global State:**
  ```bash
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --getglobalstate
  ```

- **Check Allow Signed Apps:**
  ```bash
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --getallowsigned
  ```

- **Check Stealth Mode:**
  ```bash
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --getstealthmode
  ```

- **Check Logging Options and Mode:**
  ```bash
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --getloggingopt
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --getloggingmode
  ```

- **Check Block All State:**
  ```bash
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --getblockall
  ```
  
### Trusted Applications Management
- **List Applications:**
  ```bash
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw -l
  ```

- **Add/Remove/Block/Unblock Applications:**
  ```bash
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --add path
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --remove path
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --blockapp path
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --unblockapp path
  sudo /usr/libexec/ApplicationFirewall/socketfilterfw --getappblocked path
  ```

---

## Global ALF Configuration

- **Global ALF Configuration File:**
  `/usr/libexec/ApplicationFirewall/com.apple.alf.plist`

  - **Global Preferences:**
    - `globalstate` (0 = disabled, 1 = specific services, 2 = essential services)
    - `stealthenabled`
    - `loggingenabled`

### Preferences Files to Verify
```sh
defaults read /usr/libexec/ApplicationFirewall/com.apple.alf.plist
```

---

## Additional Resources

- **ALF Mobile Configuration Example:**
  [Kolide ALF Mobileconfig](https://www.kolide.com/features/checks/mac-firewall)

- **Application Firewall Repository:**
  [App Firewall on GitHub](https://github.com/doug-leith/appFirewall)

- **System Firewall Data Type:**
  ```bash
  system_profiler SPFirewallDataType
  ```

---