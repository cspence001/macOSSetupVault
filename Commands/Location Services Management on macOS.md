
---

## Managing `locationd` Client Preferences

### Backup and Edit `clients.plist`
1. **Close System Settings.**
2. **Backup the `clients.plist` file:**
   ```bash
   sudo cp /private/var/db/locationd/clients.plist ~/clients.plist
   chown $USER:staff ~/clients.plist
   ```

3. **Edit with Xcode or another plist-compatible editor.**
4. **Restore the modified file:**
   ```bash
   sudo chown _locationd:_locationd ~/clients.plist
   sudo cp ~/clients.plist /private/var/db/locationd/clients.plist
   sudo killall locationd
   ```

### List and Delete App IDs Using `locationd`
- **For a list of app IDs using `locationd`:**
  ```bash
  sudo defaults read /private/var/db/locationd/clients.plist
  ```

- **Delete a specific app ID:**
  Replace `291BDDC9-1508-47C4-B2E9-EEB42B92BDB6:ius.zoom.xos` with the target app ID.
  ```bash
  sudo -u "_locationd" defaults delete /private/var/db/locationd/clients.plist 291BDDC9-1508-47C4-B2E9-EEB42B92BDB6:ius.zoom.xos
  ```

**Note:** As shown above, for modifying files owned by system process (e.g. `_analytics` user):
`/usr/bin/sudo -u "_analytics" defaults read <plist>`

### Disable Location Services
- **Temporarily disable Location Services:**
  ```bash
  sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.locationd.plist
  sudo defaults write /var/db/locationd/Library/Preferences/ByHost/com.apple.locationd LocationServicesEnabled -int 0
  sudo chown -R _locationd:_locationd /var/db/locationd
  sudo launchctl load /System/Library/LaunchDaemons/com.apple.locationd.plist
  ```

- **Alternative method:**
  ```bash
  sudo -u "_locationd" defaults -currentHost write /var/db/locationd/Library/Preferences/ByHost/com.apple.locationd LocationServicesEnabled -int 0
  ```

## Additional Resources
- [Reddit Discussion on Removing Apps from Location Services](https://www.reddit.com/r/MacOS/comments/mw102n/how_to_remove_app_from_location_services_list/?rdt=61494)
- [Set Location Services and Time Zone Script](https://github.com/boberito/jamfscripts/blob/master/Set%20Location%20Services%20and%20Time%20Zone%20etc.sh)
- [macOS Preferences Configuration Script](https://github.com/tiiiecherle/osx_install_config/blob/master/11_system_and_app_preferences/11c_macos_preferences_10_14.sh)
- [Location Services Authorization Script](https://community.jamf.com/t5/jamf-pro/location-services/m-p/257421)
