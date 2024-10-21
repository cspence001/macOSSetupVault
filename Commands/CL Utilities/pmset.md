
---

## `pmset` Command Overview

The `pmset` command in macOS allows you to configure power management settings. The settings can be adjusted for battery power (`-b`), charger (`-c`), UPS (`-u`), or all power sources (`-a`).

### **Power Management Settings**

1. **Sleep and Hibernation:**
   - `sudo pmset -a hibernatefile null`
     - Sets the location of the hibernation file to be used when going into standby mode. It must be on the boot disk.
   - `sudo pmset -a hibernatemode 25`
     - Sets the hibernation mode. Options:
       - `0`: Plain sleep (desktops)
       - `3`: Store a copy of memory to disk and keep memory powered up (laptops)
       - `25`: Full hibernation, which maximizes battery runtime but takes longer to enter and wake up. The system writes the contents of memory to disk and removes power to RAM while sleeping. It will restore from the disk image if needed. 
   - `sudo pmset -a sleep 25`
     - System sleep timeout in minutes.
   - `sudo pmset -a standby 1`
     - Activates standby mode, a deeper sleep state that saves more power.
   - `sudo pmset -a standbydelay 0`
   - `sudo pmset -a standbydelayhigh 0`
   - `sudo pmset -a standbydelaylow 0`
     - Delays before entering standby mode in various power conditions.
   - `sudo pmset -a autopoweroff 1`
     - Enables low-power sleep or standby mode.
   - `sudo pmset -a autopoweroffdelay 0`
     - Delay in seconds before transitioning to low-power sleep or standby mode.

2. **Sleep and Wake Options:**
   - `sudo pmset -a lidwake 1`
     - Wake the machine when the laptop lid (or clamshell) is opened.
   - `sudo pmset -a womp 0`
     - Disable wake on Ethernet magic packet (Wake for network access).
   - `sudo pmset -a ttyskeepawake 0`
     - Disables preventing idle system sleep when any tty (e.g., remote login session) is active.
   - `sudo pmset -a tcpkeepalive 0`
     - Prevents the system from connecting to the internet while sleeping. **WARNING**: Critical features like "Find My Mac" will not function at sleep when this option is disabled.
   - `sudo pmset -a proximitywake 0`
     - Controls system wake from sleep based on proximity of devices using the same iCloud ID.
   - `sudo pmset -a destroyfvkeyonstandby 1`
     - Ensures FileVault key is destroyed on standby.
   - `sudo pmset -a ring 0`
     - Disables wake on modem ring.

3. **Disk and Display Sleep:**
   - `sudo pmset -a disksleep 20`
     - Disk spindown timeout in minutes.
   - `sudo pmset -a displaysleep 20`
     - Display sleep timeout in minutes.

4. **Power Nap and Motion Sensor:**
   - `sudo pmset -a powernap 0`
     - Disables Power Nap, which allows the Mac to perform actions while asleep. (e.g. email checks, etc.)
   - `sudo pmset -a sms 0`
     - Disables sudden motion sensor (not necessary for SSD machines).

5. **Low Power Mode:**
   - `sudo pmset -a lowpowermode 1`
     - Reduces system power usage by optimizing performance and reducing background activity.

### **Checking and Managing Settings**

- `sudo pmset -g`
  - Displays current power management settings.
- `sudo pmset -g pslog`
  - Shows power source log.
- `pmset -g userclients`
  - Lists user clients that may affect power management.

### **File Management and Miscellaneous**

- **Remove the sleep image file:**
  - `sudo rm /private/var/vm/sleepimage`
  - Removes the sleep image file to save disk space.
- **Create a zero-byte file:**
  - `sudo touch /private/var/vm/sleepimage`
  - Creates a zero-byte file in place of the sleep image file.
- **Prevent file from being rewritten:**
  - `sudo chflags uchg /private/var/vm/sleepimage`
  - Ensures the zero-byte file cannot be overwritten.
[Energy Saving `pmset` config](https://github.com/mathiasbynens/dotfiles/blob/b7c7894e7bb2de5d60bfb9a2f5e46d01a61300ea/.macos#L179)
### **Further Reading**

- [PMSET Reference](https://dssw.co.uk/reference/pmset/)
- [Power Management in Detail using pmset](https://eclecticlight.co/2017/01/20/power-management-in-detail-using-pmset/)
- [gist: pmset settings overview](https://gist.github.com/Miigon/545e711090d345b7cdbb455c1ca5c23f)
- [Enable Hibernation on M1 Mac](https://apple.stackexchange.com/questions/416108/how-to-enable-hibernation-on-m1-mac)

For more details on each option and their impacts, refer to the provided links and the `pmset` man page.

---

Adjust any specific settings according to your requirements and check the impact of each change on your system’s power management and battery life.


---

are any of these settings contradictory?
`pmset -g` displays: 
Warning: This option disables TCP Keep Alive mechanism when sytem is sleeping. This will result in some critical features like 'Find My Mac' not to function properly. Setting destroyfvkeyonstandby to True. When system enters standby with this key set all maintenance wakes and powernap activities are disabled System-wide power settings: DestroyFVKeyOnStandby 1 Currently in use: lidwake 1 autopoweroff 0 lowpowermode 0 standbydelayhigh 0 autopoweroffdelay 0 proximitywake 0 standby 0 standbydelaylow 0 ttyskeepawake 0 hibernatemode 25 powernap 0 gpuswitch 2 hibernatefile null highstandbythreshold 50 displaysleep 20 (display sleep prevented by Brave Browser) womp 0 networkoversleep 0 sleep 25 (sleep prevented by Brave Browser, coreaudiod) tcpkeepalive 0 halfdim 1 acwake 0 disksleep 20


Based on the `pmset` settings displayed and the additional information provided, here’s an analysis of potential contradictions or issues:

### **Settings Overview and Potential Conflicts**

1. **Power Nap and Standby Modes:**
   - `powernap` is set to `0` (disabled).
   - `standby` is set to `0` (disabled).
   - `destroyfvkeyonstandby` is set to `1`.

   **Potential Issue:** When `destroyfvkeyonstandby` is enabled, the system will not perform maintenance wakes or Power Nap activities. With `standby` and `powernap` both disabled, this setting might not have an effect since the system is not entering standby or performing Power Nap tasks. However, it’s worth noting that `destroyfvkeyonstandby` generally requires standby to be active to take effect.

2. **Sleep and Hibernation Modes:**
   - `hibernatemode` is set to `25` (full hibernation).
   - `sleep` is set to `25` (system sleep timeout).
   - `autopoweroff` is set to `0` (disabled).
   - `autopoweroffdelay` is set to `0`.

   **Potential Issue:** With `hibernatemode` set to `25`, the system writes the contents of memory to disk and powers down RAM. If the system is set to `sleep` after 25 minutes, it will enter hibernation, but `autopoweroff` and `autopoweroffdelay` are not active. This setup generally should not be contradictory, but you might not be leveraging the autopoweroff functionality effectively.

3. **Network and Connectivity Settings:**
   - `tcpkeepalive` is set to `0` (disabled).
   - `womp` is set to `0` (disabled).

   **Potential Issue:** Disabling `tcpkeepalive` means the system will not maintain network connections while sleeping, which affects features like `Find My Mac`. Disabling `womp` also prevents the system from waking on a network signal. Together, these settings could impact remote access and network-related functionalities.

4. **Miscellaneous Settings:**
   - `lidwake` is set to `1` (enabled).
   - `ttyskeepawake` is set to `0` (disabled).
   - `displaysleep` is set to `20` minutes.
   - `disksleep` is set to `20` minutes.
   - `gpuswitch` is set to `2` (automatic).

   **Potential Issue:** `ttyskeepawake` being `0` means that active tty sessions won’t prevent the system from sleeping, which could be conflicting if you rely on remote sessions or similar activities that keep the system awake.

### **Summary of Conflicts:**

- **Power Nap and Standby:** `destroyfvkeyonstandby` is enabled, but `standby` is disabled. The impact of `destroyfvkeyonstandby` may be irrelevant if the system doesn’t enter standby mode.
- **Sleep and Hibernation:** Disabling `autopoweroff` and `autopoweroffdelay` might conflict with `hibernatemode 25` if you expect the system to enter a low-power state after the initial sleep.
- **Network Connectivity:** Disabling `tcpkeepalive` and `womp` together might prevent essential network-based functionalities and remote wake-up capabilities.

These settings are not strictly contradictory but might not be fully optimized for your needs depending on how you use your system. Adjustments might be necessary based on your usage patterns and the importance of features like remote access or network connectivity.