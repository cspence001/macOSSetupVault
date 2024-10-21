#### Install Latest MSOffice Suite on macOS

`install_latest_msoffice.sh`
```sh
#!/bin/bash
# Download Latest Microsoft Office
# Stephen Short, 10/2021

xmlLocation="/private/var/tmp/office-version-output.xml"

# Download the latest Office version information
curl "https://macadmins.software/latest.xml" -o $xmlLocation

officeVersion=$(xmllint --xpath '/latest/o365' $xmlLocation | awk -F"[><]" '{print $3}')
downloadLink=$(xmllint --xpath '//package/download' $xmlLocation | head -n 1 | awk -F "[><]" '{print $3}')

# Download the Microsoft Office package
curl -L --output /private/var/tmp/Microsoft-Office-$officeVersion.pkg "$downloadLink"
sleep 3

# Install the package as root
sudo installer -pkg /private/var/tmp/Microsoft-Office-$officeVersion.pkg -target /

# Cleanup process
rm -f $xmlLocation
rm -f /private/var/tmp/Microsoft-Office-$officeVersion.pkg

echo "Installation complete and cleanup done."
```


#### Removing + Disabling Applications 

**Remove AutoUpdater Entirely**
```sh
sudo rm -rfv /Applications/Microsoft\ Defender\ Shim.app
sudo rm -rfv ~/Library/Preferences/com.microsoft.autoupdate2.plist 
sudo rm -rfv ~/Library/Preferences/com.microsoft.autoupdate.fba.plist
sudo rm -rfv /Library/Preferences/com.microsoft.autoupdate2.plist
sudo rm -rfv /Library/Application\ Support/Microsoft/MAU2.0/Microsoft\ AutoUpdate.app
sudo rm -rfv ~/Library/Caches/com.microsoft.autoupdate.fba
sudo rm -rfv ~/Library/Group\ Containers/UBF8T346G9.ms
sudo rm -rfv ~/Library/HTTPStorages/com.microsoft.autoupdate.fba
sudo rm -rfv /Library/LaunchAgents/com.microsoft.update.agent.plist
sudo rm -rfv /Library/LaunchDaemons/com.microsoft.autoupdate.helper.plist
sudo rm -rfv /Library/PrivilegedHelperTools/com.microsoft.autoupdate.helper
sudo rm -rfv /Library/Logs/Microsoft/autoupdate.log
```
[Reference](https://superuser.com/q/1306789)

**Alternative: Disable AutoUpdater using Preference file**
https://gist.github.com/refo/6127df8f9da7c31ae5220c1a9b880b35
- Disable Launching at boot & periodic checks:
```sh
sudo defaults write /Library/LaunchAgents/com.microsoft.update.agent.plist Disabled -bool YES
sudo defaults write /Library/LaunchAgents/com.microsoft.update.agent.plist RunAtLoad -bool NO
sudo chflags schg /Library/LaunchAgents/com.microsoft.update.agent.plist
```
* If above command does not work:
```sh
sudo /usr/libexec/PlistBuddy -c "Set Disabled YES" /Library/LaunchAgents/com.microsoft.update.agent.plist
sudo /usr/libexec/PlistBuddy -c "Set RunAtLoad NO" /Library/LaunchAgents/com.microsoft.update.agent.plist
```

- Disable feedback, telemetry and starting of the Microsoft AutoUpdate app when you start another Microsoft app:
```sh
defaults write com.microsoft.autoupdate2 'MAUFeedbackEnabled' -bool FALSE
defaults write com.microsoft.autoupdate2 'SendAllTelemetryEnabled' -bool FALSE
defaults write com.microsoft.autoupdate2 'StartDaemonOnAppLaunch' -bool FALSE
```


**Remove Outlook**
```sh
sudo rm -rfv /Applications/Microsoft\ Outlook.app
sudo rm -rfv ~/Library/Containers/com.microsoft.Outlook.*
```

**Remove Powerpoint**
```sh
sudo rm -rfv /Applications/Microsoft\ PowerPoint.app
sudo rm -rfv ~/Library/Containers/com.microsoft.Outlook.*
```

#### Identifying Locations of microsoft files + directories

**Search for Files/Directories containing 'microsoft'**:
```sh
find /Library -iname '*microsoft*' 2>/dev/null
find ~/Library -iname '*microsoft*' 2>/dev/null
```

**User-level search**:
```sh
$ find ~/Library -iname '*microsoft*' 2>/dev/null

~/Library/Application Support/Microsoft
~/Library/Application Support/FileProvider/com.microsoft.OneDrive.FileProvider

~/Library/Preferences/com.microsoft.OneDriveStandaloneUpdater.plist
~/Library/Preferences/com.microsoft.shared.plist
~/Library/Preferences/com.microsoft.OneDriveUpdater.plist

~/Library/HTTPStorages/com.microsoft.OneDriveStandaloneUpdater
~/Library/HTTPStorages/com.microsoft.OneDriveStandaloneUpdater.binarycookies
~/Library/HTTPStorages/com.microsoft.SyncReporter.binarycookies
~/Library/HTTPStorages/com.microsoft.SyncReporter

~/Library/Containers/com.microsoft.OneDrive.FinderSync
~/Library/Containers/com.microsoft.OneDrive.FinderSync/Data/Library/Application Scripts/com.microsoft.OneDrive.FinderSync
~/Library/Containers/com.microsoft.OneDrive.FileProvider
~/Library/Containers/com.microsoft.OneDrive.FileProvider/Data/Library/Application Scripts/com.microsoft.OneDrive.FileProvider
~/Library/Containers/com.microsoft.Outlook.CalendarWidget
~/Library/Containers/com.microsoft.Outlook.CalendarWidget/Data/Library/Application Scripts/com.microsoft.Outlook.CalendarWidget
~/Library/Containers/com.microsoft.onenote.mac.shareextension
~/Library/Containers/com.microsoft.onenote.mac.shareextension/Data/Library/Application Scripts/com.microsoft.onenote.mac.shareextension

~/Library/Caches/Microsoft
~/Library/Caches/Microsoft/uls/com.microsoft.autoupdate.fba
~/Library/Caches/Microsoft/uls/com.microsoft.autoupdate2
~/Library/Caches/com.microsoft.OneDriveStandaloneUpdater
~/Library/Caches/com.microsoft.SyncReporter
```

**System-level search**:
```sh
$ find /Library -iname '*microsoft*' 2>/dev/null

/Library/PrivilegedHelperTools/com.microsoft.office.licensingV2.helper

/Library/Logs/Microsoft
/Library/Logs/Microsoft/InstallLogs/com.microsoft.package.Frameworks.generic.postinstall.log
/Library/Logs/Microsoft/InstallLogs/com.microsoft.package.Proofing_Tools.generic.postinstall.log
/Library/Logs/Microsoft/InstallLogs/com.microsoft.package.DFonts.generic.postinstall.log

/Library/LaunchAgents/com.microsoft.SyncReporter.plist
/Library/LaunchAgents/com.microsoft.OneDriveStandaloneUpdater.plist

/Library/LaunchDaemons/com.microsoft.OneDriveStandaloneUpdaterDaemon.plist
/Library/LaunchDaemons/com.microsoft.OneDriveUpdaterDaemon.plist
/Library/LaunchDaemons/com.microsoft.office.licensingV2.helper.plist
```

**Onedrive (files not included/shown in 'microsoft' search)**
```sh 
find ~/Library -iname '*onedrive*' 2>/dev/null

~/Library/Logs/OneDrive
~/Library/Logs/OneDrive/Updater/OneDriveUpdater-2024-09-21-011424.log

~/Library/Group Containers/UBF8T346G9.OneDriveStandaloneSuite
~/Library/Group Containers/UBF8T346G9.OneDriveStandaloneSuite/Library/Preferences/UBF8T346G9.OneDriveStandaloneSuite.plist
~/Library/Group Containers/UBF8T346G9.OneDriveStandaloneSuite/Library/Application Scripts/UBF8T346G9.OneDriveStandaloneSuite
```


#### Removing Office Entirely:

From [link Official](https://support.microsoft.com/en-us/office/uninstall-office-for-mac-eefa1199-5b58-43af-8a3d-b73dc1a8cae3):

```sh
~/Library/Group\ Containers/UBF8T346G9.ms
~/Library/Group\ Containers/UBF8T346G9.Office 
~/Library/Group\ Containers/UBF8T346G9.OneDriveStandaloneSuite
```

```sh
~/Library/Containers/com.microsoft.onenote.mac.shareextension ~/Library/Containers/com.microsoft.OneDrive.FileProvider ~/Library/Containers/com.microsoft.OneDrive.FinderSync
```

**Review Directories:**
```
/Applications/Microsoft\ *

~/Library/Application\ Support/Microsoft
~/Library/Application\ Scripts/
~/Library/Containers/
~/Library/Group\
~/Library/Group\ Containers/
~/Library/Preferences/
~/Library/Caches/
~/Library/HTTPStorages/

/Library/Preferences/
/Library/Application\ Support/
/Library/LaunchAgents/
/Library/LaunchDaemons/
/Library/PrivilegedHelperTools/
/Library/Logs/
```


#### Blocking Microsoft Telemetry Domains

[Windows Office Telemetry Domains Source for DNS-level Blocking](https://github.com/hagezi/dns-blocklists/blob/main/hosts/native.winoffice.txt)

`block_microsoft_telemetry.sh`:
```sh
#!/bin/bash
# Microsoft Telemetry Domains Source: https://github.com/hagezi/dns-blocklists/blob/main/hosts/native.winoffice.txt
# bash script for macOS by: github.com/cspence001
# Directly appends new entries (Microsoft Telemetry Domains) to the existing /etc/hosts file without modifying its current content.
# Date: September 20, 2024

# Path to the hosts file
HOSTS_FILE="/etc/hosts"

# Path to backup the current hosts file
BACKUP_PATH="/etc/hosts.bak"

# Backup existing hosts file
if [ -f "$HOSTS_FILE" ]; then
    sudo cp "$HOSTS_FILE" "$BACKUP_PATH"
    echo "Backup created at $BACKUP_PATH"
fi

# Fetch domains from the URL
DOMAIN_SOURCE="https://raw.githubusercontent.com/hagezi/dns-blocklists/refs/heads/main/domains/native.winoffice.txt"
DOMAINS_TO_BLOCK=($(curl -s "$DOMAIN_SOURCE"))

# Function to add domain entries to the hosts file
add_hosts_entry() {
    local domain="$1"
    local entry="127.0.0.1 $domain"

    if ! grep -q "$entry" "$HOSTS_FILE"; then
        echo "$entry" | sudo tee -a "$HOSTS_FILE" > /dev/null
        echo "Added $domain to hosts file."
    else
        echo "$domain already exists in hosts file."
    fi
}

# Block domains
for domain in "${DOMAINS_TO_BLOCK[@]}"; do
    add_hosts_entry "$domain"
done

# Flush DNS cache
sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
echo "DNS cache flushed."

```

Ensure script is executable: `sudo chmod +x block_microsoft_telemetry.sh`
Run with `sudo`:  `sudo block_microsoft_telemetry.sh`