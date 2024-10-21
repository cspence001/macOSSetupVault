
---

## Safari Preferences Configuration

**Reference Script:** [safari.sh](https://github.com/joeyhoer/starter/blob/master/apps/safari.sh)

### General Defaults
- **System Defaults Database:**
  ```bash
  defaults read com.apple.Safari
  ```
  - Reads from system-wide cache or default values set programmatically or by other tools.
  
- **User-Specific Preferences File:**
  ```bash
  defaults read ~/Library/Preferences/com.apple.Safari.plist
  ```
  - Reads directly from the user's preference file for Safari.

### Preferences Configuration

- **Spotlight Suggestions:**
  ```bash
  defaults write com.apple.Safari UniversalSearchEnabled -bool false
  ```

- **Search Engine Suggestions:**
  ```bash
  defaults write com.apple.Safari SuppressSearchSuggestions -bool true
  ```

- **Quick Website Search:**
  ```bash
  defaults write com.apple.Safari WebsiteSpecificSearchEnabled -bool true
  ```

- **Favorites in Smart Search Field:**
  ```bash
  defaults write com.apple.Safari ShowFavoritesUnderSmartSearchField -bool false
  defaults write com.apple.Safari ShowFullURLInSmartSearchField -bool false
  ```

- **New Window and Tab Behavior:
0: Homepage
1: Empty Page
2: Same Page
3: Bookmarks
4: Top Sites

  - **New Tab Behavior:**
    ```bash
    defaults write com.apple.Safari NewTabBehavior -int 1
    ```

  - **New Window Behavior:**
    ```bash
    defaults write com.apple.Safari NewWindowBehavior -int 1
    ```

- **Set home page to `about:blank` for faster loading:**
  ```bash
  defaults write com.apple.Safari HomePage -string "about:blank"
  ```

- **Open Pages in Tabs Instead of Windows:**
  ```bash
  defaults write com.apple.Safari TabCreationPolicy -int 2
  ```
0: Never
1: Automatically
2: Always

- **Auto-Open Safe Downloads:**
  ```bash
  defaults write com.apple.Safari AutoOpenSafeDownloads -bool false
  ```

- **Thumbnail Cache for History and Top Sites:**
  ```bash
  defaults write com.apple.Safari DebugSnapshotsUpdatePolicy -int 2
  ```

- **History Items Removal:**
  ```bash
  defaults write com.apple.Safari HistoryAgeInDaysLimit -int 1
  ```

- **Session Restoration:**
  ```bash
  defaults write com.apple.Safari AlwaysRestoreSessionAtLaunch -bool false
  ```

- **Downloads List Items Removal:**
  ```bash
  defaults write com.apple.Safari DownloadsClearingPolicy -int 2
  ```
0: Manually
1: When Safari Quits
2: Upon Successful Download

- **Search Banner Behavior:**
  ```bash
  defaults write com.apple.Safari FindOnPageMatchesWordStartsOnly -bool false
  ```

- **Location Services for Websites:**
  ```bash
  defaults write com.apple.Safari SafariGeolocationPermissionPolicy -int 0
  ```
0: Deny without prompting
1: Prompt for each website once each day
2: Prompt for each website one time only

- **Do Not Track Header:**
  ```bash
  defaults write com.apple.Safari SendDoNotTrackHTTPHeader -bool true
  ```

- **Cookies and Website Data Policies:**
  ```bash
  defaults write com.apple.Safari BlockStoragePolicy -int 0
  defaults write com.apple.Safari WebKitStorageBlockingPolicy -int 2
  defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2StorageBlockingPolicy -int 1
  ```
0,2: Always block
3,1: Allow from current website only
2,1: Allow from websites I visit
1,0: Always allow
### WebKit Preferences

- **Enable Extensions:**
  ```bash
  defaults write com.apple.Safari ExtensionsEnabled -bool true
  ```

- **Disable Plug-ins:**
  ```bash
  defaults write com.apple.Safari WebKitPluginsEnabled -bool false
  defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2PluginsEnabled -bool false
  ```

- **Stop Internet Plug-ins to Save Power:**
  ```bash
  defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2PlugInSnapshottingEnabled -bool false
  ```

- **Tab Highlighting:**
  ```bash
  defaults write com.apple.Safari WebKitTabToLinksPreferenceKey -bool false
  ```

- **Disable Java:**
  ```bash
  defaults write com.apple.Safari WebKitJavaEnabled -bool false
  ```

- **Block Pop-up Windows:**
  ```bash
  defaults write com.apple.Safari WebKitJavaScriptCanOpenWindowsAutomatically -bool false
  ```

- **Enable Develop Menu and Web Inspector:**
  ```bash
  defaults write com.apple.Safari IncludeDevelopMenu -bool true
  defaults write com.apple.Safari WebKitDeveloperExtrasEnabledPreferenceKey -bool true
  ```

- **Set Default Encoding:**
  ```bash
  defaults write com.apple.Safari WebKitDefaultTextEncodingName -string 'utf-8'
  ```

- **Print Backgrounds:**
  ```bash
  defaults write com.apple.Safari WebKitShouldPrintBackgroundsPreferenceKey -bool false
  ```

- **Omit PDF Support:**
  ```bash
  defaults write com.apple.Safari WebKitOmitPDFSupport -bool YES
  ```

- **Disable Auto-Playing Video:**
  ```bash
  # Uncomment to disable auto-playing videos
  # defaults write com.apple.Safari WebKitMediaPlaybackAllowsInline -bool false
  ```

- **Allow WebGL:**
  ```bash
  # Uncomment to enable WebGL
  # defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2WebGLEnabled -bool true
  ```

- **Safari WebKit Preferences:**
  ```bash
  defaults write com.apple.Safari "WebKitPreferences.applePayEnabled" 0
  defaults write com.apple.Safari "WebKitPreferences.javaScriptCanOpenWindowsAutomatically" 0
  defaults write com.apple.Safari "WebKitPreferences.pushAPIEnabled" 1
  defaults write com.apple.Safari "WebKitPreferences.usesPageCache" 1
  ```

* **[Disable Hyperlink Auditing Beacon](https://gist.github.com/Kuret/d21ddcd94c23df07571924f77d080569#disable-hyperlink-auditing-beacon)**
The `<a ping>` attribute pings a website when clicking on a link, used for tracking.

Safari:
```sh
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2HyperlinkAuditingEnabled -bool false
```
Safari Preview:
```sh
defaults write com.apple.SafariTechnologyPreview com.apple.Safari.ContentPageGroupIdentifier.WebKit2HyperlinkAuditingEnabled -bool false
```
### WebRTC
- **Disable WebRTC:**
  - [Disable and Toggle WebRTC in macOS Safari](https://github.com/JayBrown/Disable-and-toggle-WebRTC-in-macOS-Safari)

### Bookmarks Sync and System Cleanup

Need to disable SIP for the following commands.

- **Remove Bookmarks Sync Agent:**
  ```bash
  sudo rm -rfv /Library/Apple/System/Library/CoreServices/SafariSupport.bundle
  ```

- **Remove Cloud Bookmarks Notifier:**
  ```bash
  sudo rm -rfv /Library/Apple/System/Library/Accounts/Notification/CloudBookmarksAccountsNotifier.bundle
  ```

- **Remove Siri Assistant Plugin:**
  ```bash
  sudo rm -rfv /Library/Apple/System/Library/Assistant/Plugins/Safari.assistantBundle
  ```

- **Remove Safari Application:**
  ```bash
  cd /Applications
  sudo rm -rfv Safari.app
  ```

- **Hide IP Address from Trackers:**
  ```bash
  defaults write /Users/meuthu/Library/Preferences/com.apple.Safari WBSPrivacyProxyAvailabilityTraffic -int 33422572
  ```
33422560 will set hide IP address from trackers to disabled. 
33422564 will enable from Trackers Only.
33422572 will enabled from Trackers and Websites.
[CIS](https://www.tenable.com/audits/items/CIS_Apple_macOS_13.0_Ventura_v2.0.0_L2.audit:6d9f0d90b7c71b0010149699d19323d5)
### App Store Preferences

- **Disable WebKit Developer Tools:**
  ```bash
  defaults write com.apple.appstore WebKitDeveloperExtras -bool false
  ```

- **Disable Debug Menu in App Store:**
  ```bash
  defaults write com.apple.appstore ShowDebugMenu -bool false
  ```

- **Disable Auto-Play Videos:**
  ```bash
  defaults write ~/Library/Preferences/com.apple.AppStore.plist AutoPlayVideoSetting -string "off"
  defaults write ~/Library/Preferences/com.apple.AppStore.plist UserSetAutoPlayVideoSetting -bool true
  ```

### Security Compliance Checks
[NIST](From https://github.com/usnistgov/macos_security/blob/main/rules/os/)

- **Advertising Privacy Protection:**
  ```bash
  defw com.apple.Safari "WebKitPreferences.privateClickMeasurementEnabled" 1
  ```

- **Prevent Cross-Site Tracking:**
  ```bash
  defw com.apple.Safari "WebKitPreferences.storageBlockingPolicy" 1
  defw com.apple.Safari "WebKitStorageBlockingPolicy" 1
  defw com.apple.Safari "BlockStoragePolicy" 2
  ```

- **Disable Automatic Opening of Safe Files:**
  ```bash
  defw com.apple.Safari "AutoOpenSafeDownloads" 0
  ```

- **Block Pop-Up Windows:**
  ```bash
  defw com.apple.Safari "safariAllowPopups" 0
  ```

---
