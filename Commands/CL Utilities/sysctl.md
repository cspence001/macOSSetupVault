
---

# `sysctl` Configuration and Commands for macOS

**[sysctl Overview](https://tldr.inbrowser.app/pages/osx/sysctl)**

> Access kernel state information. More details can be found here: [sysctl Man Page](https://keith.github.io/xcode-man-pages/sysctl.8.html).

---

## Index

- [[#`sysctl` Display Configurations]]
- [[#`sysctl` Configuration Notes]]
    - [[#Overview]]
    - [[#List Kernel Configuration]]
    - [[#Core Dumps]]
    - [[#Security Settings]]
    - [[#Network and IP Forwarding Settings]]
- [[#Additional Commands]]
- [[#`sysctl.conf`]]

---
## `sysctl` Display Configurations

- **Show all available variables and their values**:
  ```bash
  sysctl -a
  ```

- **Show Apple model identifier**:
  ```bash
  sysctl -n hw.model
  ```

- **Show CPU model**:
  ```bash
  sysctl -n machdep.cpu.brand_string
  ```

- **Show available CPU features (MMX, SSE, SSE2, SSE3, AES, etc.)**:
  ```bash
  sysctl -n machdep.cpu.features
  ```

- **Set a changeable kernel state variable**:
  ```bash
  sysctl -w section.tunable=value
  ```

---

## `sysctl` Configuration Notes

### Overview
`sysctl` settings are reset after a reboot. To make changes persistent, add configurations to `/etc/sysctl.conf`.  
- [[#`sysctl.conf`]] 

Example:
```bash
/etc/sysctl.conf:
kern.securelevel=1
```

### List Kernel Configuration
To list all current kernel configuration settings:
```bash
sysctl -a
```

### Core Dumps
Disable core dumps:
```bash
sysctl kern.coredump=0
```

To remove existing core files:
```bash
sudo rm -f /cores/*
```
[core-dumps-in-macOS]([https://krypted.com/mac-security/core-dumps-in-mac-os-x/)

### Security Settings
Enhance system security with the following settings:

- **Secure Kernel**:
  ```bash
  sysctl kern.secure_kernel=1
  ```

- **Secure Level**:
  ```bash
  sysctl kern.securelevel=1
  ```
Example Implications of kern.securelevel setting:
- 1 means you need to boot to single-user mode to run `chflags noschg /etc/hosts`,
- 0 means you can simply `sudo chflags noschg /etc/hosts`.

- **Code Signing Enforcement**:
  ```bash
  sudo sysctl vm.cs_force_kill=1                # Kill process if code signing is invalidated
  sudo sysctl vm.cs_force_hard=1                # Fail operation if code signing is invalidated
  sudo sysctl vm.cs_all_vnodes=1                # Apply on all Vnodes
  sudo sysctl vm.cs_system_enforcement=1        # Globally apply code signing enforcement
  sudo sysctl vm.cs_process_enforcement=1       # Check code signing for processes
  sudo sysctl vm.cs_library_validation=1        # Validate code signing for libraries
  ```

  For more details, refer to: [New OS X Book - Appendix A](https://newosxbook.com/files/moxii3/AppendixA.pdf)

### Network and IP Forwarding Settings

- **Disable IP Forwarding**:
  ```bash
  sysctl net.inet.ip.forwarding=0
  sysctl net.inet6.ip6.forwarding=0
  ```

- **Disable Source-Routed IPv4 Packets**:
  ```bash
  sysctl net.inet.ip.accept_sourceroute=0
  ```

- **Disable IPv4/IPv6 ICMP Redirects**:
  ```bash
  sysctl net.inet.ip.redirect=0
  sysctl net.inet6.ip6.redirect=0
  ```

- **Ignore IPv4 ICMP Redirect Messages**:
  ```bash
  sysctl net.inet.icmp.drop_redirect=1
  ```

- **Prevent Source-Routed Packets**:
  ```bash
  sysctl net.inet.ip.sourceroute=0
  ```

- **Ignore ICMP Timestamp Requests**:
  ```bash
  sysctl net.inet.icmp.timestamp=0
  ```

For additional references, see: [Tenable DISA STIG for Apple OS X 10.13](https://www.tenable.com/audits/DISA_STIG_Apple_OS_X_10.13_v2r5)

---

## Additional Commands

- **VM Detection**:
  ```bash
  sysctl vm.swapusage
  ```

- **Hardware Model**:
  ```bash
  sysctl hw.model
  ```

- **Man Pages**:
  ```bash
  man sysctl
  ```

- **Kernel Configuration**:
  ```bash
  sysctl -a
  ```

- **Check Disk**:
  ```bash
  df
  ```

- **Device Filesystem Paths (major/minor)**:
  ```bash
  find /dev -type b -ls
  ```
  For more info: [Get Device Filesystem Path from /dev/t on macOS](https://stackoverflow.com/questions/54790858/get-device-filesystem-path-from-dev-t-on-macos)

- **List Users**:
  ```bash
  w
  ```

- **List Connected Hard Drives**:
  ```bash
  diskutil list
  ```

- **System Info**:
  ```bash
  uname -a
  ```

---

## `sysctl.conf`

- [Make sysctl changes at startup](https://apple.stackexchange.com/questions/321720/make-sysctl-changes-at-startup) 
- [Persistence for sysctl using etc/sysctl.conf](https://apple.stackexchange.com/a/390961)
- **Network sysctl plist**:
  ```bash
  /Library/Preferences/com.apple.networkd.sysctl.plist
  ```
