
---
##### Index

- [[#SQLite Commands for TCC Database]] 
  - [[#Root-Level TCC Database]]
  - [[#User-Level TCC Database]]
  - [[#Common Tables to Query]]
  - [[#Queries]]
  - [[#Example User Access Entries]]
	  - [[#TCC.db Fields + Descriptions]]
  - [[#Reviewing Permissions in TCC.db]]
    - [[#System database permissions listing]]
    - [[#User database permissions listing]]
    - [[#Checking Specific Permission]]
    - [[#Checking Specific App]]
    - [[#TCC.db backup]]
    - [[#Making changes to TCC.db]]
    - [[#Other schema fields in TCC.db]]
- [[#Ventura’s System Privacy Settings]] 
	- [[#Overview]]
	- [[#Determining System Privacy Settings]]
	- [[#Internal Code Names]]
	- [[#iCloud Services Access]]
	- [[#TCC Reset]]
	- [[#TCC Services]]
	- [[#TCC Clients]]
		- [[#CloudKit Access (kTCCServiceLiverpool)]] agent subsystems and their function
		- [[#iCloud Drive Access (kTCCServiceUbiquity)]] agent subsystems and their function
	- [[#Restoration of Services]]
	- [[#Conclusions]]
- [[#Checking persistent permissions between TCC.db reset + reboot]]
- [[#AdhocSignatureCache]]
	- [[#Understanding UUIDs in AdhocSignatureCache]]
- [[#Logging]]
- [[#TCC Privacy Policy Configuration Profiles]]
- [[#More Reference Articles]]
- [[#3-Party Tools for Interacting with TCC.db]]


---
## SQLite Commands for TCC Database

### Root-Level TCC Database

```bash
sudo sqlite3 /Library/Application\ Support/com.apple.TCC/TCC.db
```

### User-Level TCC Database

```bash
sudo sqlite3 ~/Library/Application\ Support/com.apple.TCC/TCC.db
```

#### Important 
**When querying TCC.db with sqlite**
* For `Error: unable to open database "/Library/Application Support/com.apple.TCC/TCC.db": authorization denied` Enable Full Disk access permission for Terminal.app in System Settings > Privacy & Security > Full Disk Access.
* All sqlite3 queries listed below are given with examples for either the Root-Level or User-Level TCC database, but all commands are interchangeable for both databases by either adding or removing the tilde `~` as shown above for each database.
### Common Tables to Query

- `admin`
- `issuers`
- `groups`
- `sqlite_sequence`
- `serials`
- `hashes`
- `dates`
- `access`

### Queries

```sql
select * from admin;
select * from policies;
select * from active_policy;
select * from access;  -- user-specific
select * from access_overrides;
select * from active_policy_id;
```

### Example User Access Entries

`select * from access;`
```
kTCCServiceLiverpool|com.apple.securityd|0|2|4|1|||0|UNUSED||0|1676520760
kTCCServiceLiverpool|com.apple.cloudpaird|0|2|4|1|||0|UNUSED||0|1676520760
kTCCServiceLiverpool|com.apple.identityservicesd|0|2|4|1|||0|UNUSED||0|1676520760
kTCCServiceLiverpool|com.apple.shortcuts|0|2|4|1|||0|UNUSED||0|1676520760
kTCCServiceLiverpool|com.apple.syncdefaultsd|0|2|4|1|||0|UNUSED||0|1676520760
kTCCServiceLiverpool|com.apple.transparencyd|0|2|4|1|||0|UNUSED||0|1676520760
kTCCServiceLiverpool|com.apple.sociallayerd|0|2|4|1|||0|UNUSED||0|1676520760
kTCCServiceLiverpool|com.apple.assistant.assistantd|0|2|4|1|||0|UNUSED||0|1676520760
kTCCServiceLiverpool|com.apple.willowd|0|2|4|1|||0|UNUSED||0|1676520760
kTCCServiceLiverpool|com.apple.appleaccountd|0|2|4|1|||0|UNUSED||0|1676520761
kTCCServiceLiverpool|com.apple.donotdisturbd|0|2|4|1|||0|UNUSED||0|1676520761
kTCCServiceLiverpool|com.apple.textinput.KeyboardServices|0|2|4|1|||0|UNUSED||0|1676520761
kTCCServiceLiverpool|com.apple.suggestd|0|2|4|1|||0|UNUSED||0|1676520761
kTCCServiceLiverpool|com.apple.passd|0|2|4|1|||0|UNUSED||0|1676520761
kTCCServiceLiverpool|com.apple.amsengagementd|0|2|4|1|||0|UNUSED||0|1676520763
kTCCServiceLiverpool|com.apple.StatusKitAgent|0|2|4|1|||0|UNUSED||0|1676520770
kTCCServiceLiverpool|com.apple.Passbook|0|2|4|1|||0|UNUSED||0|1676520771
kTCCServiceLiverpool|com.apple.icloud.fmfd|0|2|4|1|||0|UNUSED||0|1676520800
```

#### TCC.db Fields + Descriptions
`service | client | client_type | auth_value | auth_reason | auth_version | csreq | policy_id | indirect_object_identifier_type | indirect_object_identifier | indirect_object_code_identity | flags | last_modified | pid | pid_version | boot_uuid | last_reminded `

- **service** – What service access is being restricted to. E.g. **kTCCServiceMicrophone**. See below for a [full list of services](https://rainforest.engineering/2021-02-09-macos-tcc/#all-services).
- **client** – [Bundle Identifier](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW1) or absolute path to the program that wants to use **service** (e.g. **com.apple.finder** or **/usr/libexec/sshd-keygen-wrapper)**
- **client_type** – For the previous value, whether it’s a Bundle Identifier(0) or an absolute path(1)
- **allowed** – (_Prior to big sur_) Whether to allow access(1) to this service or deny it(0)
- **prompt_count** – (_Prior to big sur_) How many times the user has been prompted about this access. Programs can repeatedly prompt if they are not granted access the first time so this can be used to track if that’s happening.
- **auth_value** – Whether the access is denied(0), unknown(1), allowed(2), or limited(3). An example for limited access: an application might be allowed ask the user to select some photos, but without being granted access to the User’s entire Photo library. It’s unclear if this is actually implemented in macOS or if this is a carryover from iOS.
- **auth_reason** – A code indicating how this **auth_value** was set. A common value is 3 which means “User Set”. See below for [full list of values and their meaning](https://rainforest.engineering/2021-02-09-macos-tcc/#auth-reason-values).
- **auth_version** – Always 1. Since these **auth_*** fields are new with macOS Big Sur, this seems expected. Presumably this will change with future macOS releases.
- **csreq** – Binary code signing requirement blob that the **client** must satisfy in order for access to be granted. This is used to prevent spoofing/impersonation if another program uses the same bundle identifier. There’s an excellent [stack overflow answer](https://stackoverflow.com/a/57259004/841300)(written by yours truly) that covers how this field can be decoded(or generated).
- **policy_id** – I believe this is related to [MDM](https://support.apple.com/guide/mdm/mdm-overview-mdmbf9e668/web)(Mobile Device Management) policies, which can be used by organizations to allow TCC access for some applications at a global level. Policies are not able to automatically grant camera or microphone access however. One tool that can generate these profiles is [github.com/carlashley/tccprofile](https://github.com/carlashley/tccprofile).
- **indirect_object_identifier** – For some services (**kTCCServiceAppleEvents**) this is what the client is asking to interact with. It’s an absolute path or bundle identifier just like client. This will be set to UNUSED in cases where it doesn’t make sense.
- **indirect_object_identifier_type** – For the previous value, whether it’s a Bundle Identifier(0) or an absolute path(1)
- **indirect_object_code_identity** – Same as **csreq**, but for the **indirect_object_identifier** instead of client.
- **flags** – I wasn’t able to find any documentation on this, and it always seems to be 0. I believe it is likely used along with the MDM policies.
- **last_modified** – The last time this entry was modified(seconds since the start of the unix [epoch](https://en.wikipedia.org/wiki/Epoch_(computing)))
[Fields Definitions Reference](https://www.rainforestqa.com/blog/macos-tcc-db-deep-dive)

### Reviewing Permissions in TCC.db
#### System database permissions listing 

List distinct services (system database):
```sh
sudo sqlite3 "/Library/Application Support/com.apple.TCC/TCC.db" "SELECT DISTINCT service FROM access;"
```
Output:
```sh
kTCCServiceAccessibility
kTCCServiceListenEvent
kTCCServicePostEvent
kTCCServiceScreenCapture
kTCCServiceSystemPolicyAllFiles
```

Services typically include:
- `kTCCServiceSystemPolicyAllFiles` (Full Disk Access)
- `kTCCServiceAccessibility` (Accessibility)
- `kTCCServiceCamera` (Camera)
- `kTCCServiceMicrophone` (Microphone)
- `kTCCServiceScreenCapture` (Screen Recording)
- `kTCCServiceLocation` (Location)
See [[#TCC Services]] for full listing. 

List apps that have been granted or denied access (system database):
```sh
sudo sqlite3 /Library/Application\ Support/com.apple.TCC/TCC.db "SELECT service, client, auth_value FROM access;"
```
Output:
```sh
kTCCServiceListenEvent|com.hnc.Discord|0
kTCCServiceAccessibility|com.hnc.Discord|0
kTCCServiceSystemPolicyAllFiles|net.sourceforge.sqlitebrowser|0
kTCCServiceAccessibility|com.apple.ScriptEditor2|0
kTCCServiceSystemPolicyAllFiles|/usr/libexec/sshd-keygen-wrapper|0
kTCCServicePostEvent|com.apple.screensharing.agent|0
kTCCServiceScreenCapture|com.apple.screensharing.agent|0
kTCCServiceAccessibility|/System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/AE.framework/Versions/A/Support/AEServer|0
kTCCServiceScreenCapture|com.brave.Browser|2
kTCCServiceAccessibility|com.alienator88.Pearcleaner|0
kTCCServiceSystemPolicyAllFiles|com.apple.Terminal|2
kTCCServiceSystemPolicyAllFiles|com.alienator88.Pearcleaner|0
```

List apps and binaries that have been granted access (system database):
```sh
sudo sqlite3 /Library/Application\ Support/com.apple.TCC/TCC.db "SELECT service, client, auth_value FROM access where auth_value;"
```

#### User database permissions listing 

List Distinct Services (user database):
```sh
sudo sqlite3 ~/Library/Application\ Support/com.apple.TCC/TCC.db "SELECT DISTINCT service FROM access;"
```
Output:
```sh
kTCCServiceAddressBook
kTCCServiceAppleEvents
kTCCServiceBluetoothAlways
kTCCServiceCalendar
kTCCServiceLiverpool
kTCCServiceMicrophone
kTCCServicePhotos
kTCCServiceReminders
kTCCServiceSystemPolicyDesktopFolder
kTCCServiceSystemPolicyDocumentsFolder
kTCCServiceSystemPolicyDownloadsFolder
kTCCServiceSystemPolicyNetworkVolumes
kTCCServiceUbiquity
kTCCServiceWebBrowserPublicKeyCredential
```

List apps that have been granted or denied access (user database):
```sh
sudo sqlite3 ~/Library/Application\ Support/com.apple.TCC/TCC.db "SELECT service, client, auth_value FROM access;"
```
Output:
```sh
kTCCServiceLiverpool|com.apple.identityservicesd|2
kTCCServiceLiverpool|com.apple.sociallayerd|2
kTCCServiceLiverpool|com.apple.securityd|2
kTCCServiceLiverpool|com.apple.shortcuts|2
kTCCServiceLiverpool|com.apple.transparencyd|2
kTCCServiceLiverpool|com.apple.biomesyncd|2
kTCCServiceLiverpool|com.apple.syncdefaultsd|2
kTCCServiceLiverpool|com.apple.knowledge-agent|2
kTCCServiceLiverpool|com.apple.textinput.KeyboardServices|2
kTCCServiceLiverpool|com.apple.siriknowledged|2
kTCCServiceLiverpool|com.apple.willowd|2
kTCCServiceLiverpool|com.apple.cloudpaird|2
kTCCServiceLiverpool|com.apple.assistant.assistantd|2
kTCCServiceLiverpool|com.apple.icloud.searchpartyuseragent|2
kTCCServiceLiverpool|com.apple.donotdisturbd|2
kTCCServiceLiverpool|com.apple.passd|2
kTCCServiceUbiquity|/System/Library/PrivateFrameworks/PhotoLibraryServices.framework/Versions/A/Support/photolibraryd|2
kTCCServiceLiverpool|com.apple.amsengagementd|2
kTCCServiceLiverpool|com.apple.Passbook|2
kTCCServiceLiverpool|com.apple.icloud.fmfd|2
kTCCServiceLiverpool|com.apple.protectedcloudstorage.protectedcloudkeysyncing|2
kTCCServiceUbiquity|com.apple.finder|2
kTCCServiceUbiquity|com.apple.TextEdit|2
kTCCServiceCalendar|com.apple.Terminal|0
kTCCServiceReminders|com.apple.Terminal|0
kTCCServiceUbiquity|com.apple.identityservicesd|2
kTCCServiceLiverpool|com.apple.imagent|2
kTCCServiceLiverpool|com.apple.upload-request-proxy.com.apple.photos.cloud|2
kTCCServiceMicrophone|com.brave.Browser|0
kTCCServiceLiverpool|com.apple.Safari|2
kTCCServiceLiverpool|com.apple.triald|2
kTCCServiceLiverpool|com.apple.StatusKitAgent|2
kTCCServiceLiverpool|com.apple.appleaccountd|2
kTCCServiceLiverpool|com.apple.avatarsd|2
kTCCServiceUbiquity|com.apple.Preview|2
kTCCServiceSystemPolicyDownloadsFolder|com.spotify.client|0
kTCCServiceMicrophone|com.apple.Terminal|0
kTCCServicePhotos|com.apple.Terminal|0
kTCCServiceAddressBook|com.apple.Terminal|0
kTCCServiceMicrophone|com.vscodium|0
kTCCServiceSystemPolicyDesktopFolder|com.apple.Terminal|0
kTCCServiceSystemPolicyDocumentsFolder|com.apple.Terminal|0
kTCCServiceMicrophone|com.hnc.Discord|0
kTCCServiceSystemPolicyNetworkVolumes|com.apple.Terminal|0
kTCCServiceLiverpool|com.apple.gamed|2
kTCCServiceMicrophone|com.apple.logic10|0
kTCCServiceBluetoothAlways|com.brave.Browser|0
kTCCServiceAppleEvents|com.apple.Automator|2
kTCCServiceSystemPolicyDocumentsFolder|org.python.python|0
kTCCServiceUbiquity|com.apple.QuickTimePlayerX|2
kTCCServiceLiverpool|com.apple.iBooks.BookDataStoreService|2
kTCCServiceLiverpool|com.apple.iBooksX|0
kTCCServiceUbiquity|com.apple.iBooksX|0
kTCCServiceWebBrowserPublicKeyCredential|com.brave.Browser|0
kTCCServiceAppleEvents|com.sublimetext.4|0
kTCCServiceAppleEvents|com.alienator88.Pearcleaner|0
```

List apps and binaries that have been granted access (user database):
```sh
sudo sqlite3 ~/Library/Application\ Support/com.apple.TCC/TCC.db "SELECT service, client, auth_value FROM access where auth_value;"
```

#### Checking Specific Permission 

List apps and binaries that have been granted Full Disk Access (system database):
```sh
sqlite3 /Library/Application\ Support/com.apple.TCC/TCC.db \
  'select client from access where auth_value and service = "kTCCServiceSystemPolicyAllFiles"'
```

List apps and binaries that have not been granted Full Disk Access (system database):
```sh
sqlite3 /Library/Application\ Support/com.apple.TCC/TCC.db \ 'select client from access where NOT auth_value and service = "kTCCServiceSystemPolicyAllFiles"'
```

List all apps and binaries who appear in list for Full Disk Access (0 - no permission, 2 - permission) (system database):
```sh
sudo sqlite3 /Library/Application\ Support/com.apple.TCC/TCC.db 'select service, client,auth_value from access where service = "kTCCServiceSystemPolicyAllFiles"'
```

List all apps and binaries who appear in list for Ubiquity (iCloud Drive) (0 - no permission, 2 - permission) (user database):
```sh
sudo sqlite3 ~/Library/Application\ Support/com.apple.TCC/TCC.db 'select service,client,auth_value from access where service = "kTCCServiceUbiquity"'
```

#### Checking Specific App 

Check Full Disk Access permissions for (specific) app (system database):
```sh
sudo sqlite3 "/Library/Application Support/com.apple.TCC/TCC.db" 'SELECT client, auth_value FROM access WHERE service = "kTCCServiceSystemPolicyAllFiles" AND client LIKE "%Pearcleaner%"'
```

Check all permissions for (specific) app (system database):
```sh
sudo sqlite3 "/Library/Application Support/com.apple.TCC/TCC.db" 'SELECT service, client, auth_value FROM access WHERE client LIKE "%Pearcleaner%"'
```


**To keep track of which (3-p) applications have requested or been granted accessibility permissions (not necessarily active, but at some point have requested):**
```sh
defaults read ~/Library/Preferences/com.apple.universalaccessAuthWarning.plist
```

#### TCC.db backup

Make backup of user TCC.db before making changes:
```sh
# This AppleScript will copy the user TCC database into /tmp
osascript <<EOD
do shell script "cp " & quoted form of (POSIX path of (path to home folder as text) & "Library/Application Support/com.apple.TCC/TCC.db") & " /tmp/TCC.db.user.bak"
EOD
```

Make backup of system TCC.db before making changes:
```sh
# This AppleScript will copy the system TCC database into /tmp
osascript <<EOD
do shell script "cp " & quoted form of "/Library/Application Support/com.apple.TCC/TCC.db" & " /tmp/TCC.db.sys.bak"
EOD
```

#### Making changes to TCC.db using sqlite
SIP needs to be disabled for making changes to the **system TCC.db** using sqlite3.
See Also [[#TCC Reset]] for making changes via `tccutil` (works without disabling SIP).

Allowing Full Disk Access for app:
```sh
sudo sqlite3 "/Library/Application Support/com.apple.TCC/TCC.db" 'UPDATE access SET auth_value = 2 WHERE client LIKE "%Pearcleaner%" AND service = "kTCCServiceSystemPolicyAllFiles"'
```

Disallowing Full Disk Access for app:
```sh
sudo sqlite3 "~/Library/Application Support/com.apple.TCC/TCC.db" 'UPDATE access SET auth_value = 0 WHERE client LIKE "%Pearcleaner%" AND service = "kTCCServiceSystemPolicyAllFiles"'
```

When making changes such as allowing/ disallowing:
For `Error: stepping, attempt to write a readonly database (8)` Review the steps above for temporarily changing file permissions. Add the write permissions for the owner, run the command for desired change, then restore the original permissions for the `TCC.db` file (removes write permission from the owner making it read-only again). (SIP needs to be disabled)
If the error persists, verify permissions for the database to ensure the file is writable:
`ls -lO "/Library/Application Support/com.apple.TCC/TCC.db"`
If restricted attribute exists, remove it to make changes:
`sudo chflags noschg "/Library/Application Support/com.apple.TCC/TCC.db"`
After making changes, restore restricted attribute:
`sudo chflags schg "/Library/Application Support/com.apple.TCC/TCC.db"`

#### Other schema fields in TCC.db
See [TCC Database Schema](https://www.rainforestqa.com/blog/macos-tcc-db-deep-dive) for full list of fields and their meaning.

**auth_reason** – A code indicating how this **auth_value** was set. A common value is 3 which means “User Set”.

**auth_reason** can take the following values:  
	**1** – Error  
	**2** – User Consent  
	**3** – User Set  
	**4** – System Set  
	**5** – Service Policy  
	**6** – MDM Policy  
	**7** – Override Policy  
	**8** – Missing usage string  
	**9** – Prompt Timeout  
	**10** – Preflight Unknown  
	**11** – Entitled  
	**12** – App Type Policy

---

### Ventura’s System Privacy Settings

- [What Are Ventura’s System Privacy Settings?](https://eclecticlight.co/2023/02/14/what-are-venturas-system-privacy-settings/)
#### Overview
- **TCC (Transparency, Consent, and Control) Subsystem**: Manages access to protected services, features, and locations in macOS.
- **Databases**:
  - **Root-Level**: `/Library/Application Support/com.apple.TCC/TCC.db`
  - **User-Level**: `~/Library/Application Support/com.apple.TCC/TCC.db`

#### Determining System Privacy Settings
- **Method**: Reset all TCC settings using `tccutil` in a clean Ventura VM instance to identify system-set privacy settings.
- **Objective**: Identify which privacy settings are set by macOS itself and not by user consent.

#### Internal Code Names
- **kTCCServiceLiverpool**: Controls CloudKit access, not related to Location Services as previously misinterpreted.
- **kTCCServiceUbiquity**: Controls iCloud services.

#### iCloud Services Access
- **CloudKit**: For sharing and syncing data.
- **iCloud Drive**: For sharing and syncing discrete files.
- **VMs on Apple Silicon**: Cannot sign in to iCloud with an Apple ID but can still use CloudKit. Public data access remains available.

#### TCC Reset
- **Command**: `sudo tccutil reset All`
- **Process**: Deletes and restores TCC settings for various services.
- `sudo tccutil reset <Service> <bundleID>`
- `sudo tccutil reset ScreenCapture com.apple.Safari`
- `sudo tccutil reset AppleEvents`
- `sudo tccutil reset All com.google.Chrome`

#### TCC Services: 
Services are what you allow client access to.
 - `All` 
 - `Accessibility`: Allows app to control your computer.
 - `AddressBook`: Contacts
 - `AppleEvents`: Automation
 - `Calendar` 
 - `Camera` 
 - `ContactsFull`: Contacts Full Info
 - `ContactsLimited`: Contacts Basic Info
 - `DeveloperTool`: Allows app to run software locally that do not meet the system’s security policy
 - `Facebook` 
 - `LinkedIn` 
 - `ListenEvent`: Input Monitoring; to monitor input from your keyboard
 - `Liverpool`: CloudKit
 - `Location` 
 - `MediaLibrary`: Apple Music, your music and video activity, and your media library.
 - `Microphone` 
 - `Motion`: Motion & Fitness Activity.
 - `Photos`: Access to your Photos
 - `PhotosAdd`: Add to your Photos
 - `PostEvent`: Allows to send keystrokes
 - `Reminders` 
 - `ScreenCapture`: Screen Recording
 - `ShareKit` 
 - `SinaWeibo` 
 - `Siri` 
 - `SpeechRecognition` 
 - `SystemPolicyAllFiles`: Full Disk Access
 - `SystemPolicyAppBundles`: *client* would like access to Modify Apps on your Mac
 - `SystemPolicyDesktopFolder`: _client_ would like to access files in your Desktop folder.
 - `SystemPolicyDeveloperFiles`: _client_ would like to access a file used in Software Development.
 - `SystemPolicyDocumentsFolder`: _client_ would like to access files in your Documents folder.
 - `SystemPolicyDownloadsFolder`: _client_ would like to access files in your Downloads folder.
 - `SystemPolicyNetworkVolumes`: *client* would like to access files on a network volume.
 - `SystemPolicyRemovableVolumes`: *client* would like to access files on a removable volume.
 - `SystemPolicySysAdminFiles`: _client_ would like to administer your computer.
 - `TencentWeibo` 
 - `Twitter` 
 - `Ubiquity`: iCloud 
 - `WebBrowserPublicKeyCredential`: *client* would like to access and use your saved passkeys.
 - `Willow`: Home data

For full list of services in TCC.db:
```sh
strings /System/Library/PrivateFrameworks/TCC.framework/Support/tccd | fgrep kTCCService | fgrep -v ' ' | sed -e s/kTCCService// | sort
```

For list of Services along with what user will be prompted with:
```sh
plutil -p /System/Library/PrivateFrameworks/TCC.framework/Versions/A/Resources/Localizable.loctable | awk '/"en" => {/ {flag=1; print; next} /"[^"]+" => {/ {flag=0} flag'
```
#### TCC Clients (bundleID)
##### CloudKit Access (kTCCServiceLiverpool)
- **27 Subsystems**:
  - `com.apple.Passbook`: Wallet services
  - `com.apple.Safari`: Safari
  - `com.apple.amsengagementd`: Apple media services engagement
  - `com.apple.appleaccountd`: Apple ID services
  - `com.apple.assistant.assistantd`: Siri, dictation, and semantic understanding
  - `com.apple.avatarsd`: Memoji and Animoji
  - `com.apple.biomesyncd`: iCloud-based suggestions
  - `com.apple.callhistory.sync-helper`: Syncing call history
  - `com.apple.cloudpaird`: Bluetooth pairing for Continuity features
  - `com.apple.donotdisturbd`: Do Not Disturb and possibly Focus
  - `com.apple.icloud.fmfd`: Find My Friends service
  - `com.apple.icloud.searchpartyuseragent`: Additional Find My support
  - `com.apple.identityservicesd`: Identity management services
  - `com.apple.imagent`: FaceTime invitations
  - `com.apple.knowledge-agent`: Siri suggestions and knowledge services
  - `com.apple.passd`: Apple Pay and Wallet services
  - `com.apple.protectedcloudstorage.protectedcloudkeysyncing`: Protected iCloud storage syncing
  - `com.apple.securityd`: Main security services
  - `com.apple.shortcuts`: Shortcuts app
  - `com.apple.siriknowledged`: Siri suggestions
  - `com.apple.sociallayerd`: Privacy-sensitive social media data
  - `com.apple.suggestd`: Content-based suggestions
  - `com.apple.syncdefaultsd`: Syncing of settings data
  - `com.apple.textinput.KeyboardServices`: Text-based services
  - `com.apple.transparencyd`: Authorization of private data access
  - `com.apple.triald`: Machine learning services
  - `com.apple.willowd`: HomeKit support and features

##### iCloud Drive Access (kTCCServiceUbiquity)
- **5 Subsystems**:
  - `photolibraryd`: Photos library services
  - `com.apple.finder`: Finder
  - `com.apple.stocks.detailintents`: Stocks
  - `com.apple.weather`: Weather
  - `com.apple.weather.widget`: Weather widget

#### Restoration of Services
- **Process**: After deletion, TCC restores most services to "Allowed" status almost immediately.
- **15 Subsystems Restored Quickly**:
  - `com.apple.amsengagementd`
  - `com.apple.assistant.assistantd`
  - `com.apple.cloudpaird`
  - `com.apple.donotdisturbd`
  - `com.apple.identityservicesd`
  - `com.apple.knowledge-agent`
  - `com.apple.passd`
  - `com.apple.securityd`
  - `com.apple.shortcuts`
  - `com.apple.siriknowledged`
  - `com.apple.sociallayerd`
  - `com.apple.suggestd`
  - `com.apple.syncdefaultsd`
  - `com.apple.transparencyd`
  - `com.apple.willowd`

- **Remaining 12 Subsystems**: May be restored later.

#### Conclusions
- **Access Without Apple ID**: 27 macOS subsystems access CloudKit and 5 access iCloud Drive without an Apple ID.
- **Data Access**: Subsystems can access public data but not private iCloud data without a connected iCloud account.


---
### Checking persistent permissions between TCC.db reset + reboot

##### Reset Liverpool:
09/11/2024 18:15
`sudo tccutil reset Liverpool` (* indicates restored after reset)
```
kTCCServiceLiverpool|com.apple.sociallayerd|2 *
kTCCServiceLiverpool|com.apple.securityd|2 *
kTCCServiceLiverpool|com.apple.cloudpaird|2 *
kTCCServiceLiverpool|com.apple.identityservicesd|2 *
kTCCServiceLiverpool|com.apple.shortcuts|2 *
kTCCServiceLiverpool|com.apple.syncdefaultsd|2 *
kTCCServiceLiverpool|com.apple.knowledge-agent|2 *
kTCCServiceLiverpool|com.apple.assistant.assistantd|2 *
kTCCServiceLiverpool|com.apple.textinput.KeyboardServices|2 *
kTCCServiceLiverpool|com.apple.willowd|2 *
kTCCServiceLiverpool|com.apple.donotdisturbd|2 *
kTCCServiceLiverpool|com.apple.siriknowledged|2 *
kTCCServiceLiverpool|com.apple.passd|2 *
kTCCServiceLiverpool|com.apple.transparencyd|2 *
kTCCServiceLiverpool|com.apple.amsengagementd|2 *
kTCCServiceLiverpool|com.apple.icloud.fmfd|2 *
kTCCServiceLiverpool|com.apple.Passbook|2 *

kTCCServiceLiverpool|com.apple.biomesyncd|2
kTCCServiceLiverpool|com.apple.protectedcloudstorage.protectedcloudkeysyncing|2
kTCCServiceLiverpool|com.apple.imagent|2
kTCCServiceLiverpool|com.apple.upload-request-proxy.com.apple.photos.cloud|2
kTCCServiceLiverpool|com.apple.Safari|2
kTCCServiceLiverpool|com.apple.triald|2
kTCCServiceLiverpool|com.apple.StatusKitAgent|2
kTCCServiceLiverpool|com.apple.appleaccountd|2
kTCCServiceLiverpool|com.apple.avatarsd|2
kTCCServiceLiverpool|com.apple.gamed|2
kTCCServiceLiverpool|com.apple.iBooks.BookDataStoreService|2
```

##### Reset Ubiquity:
09/11/2024 18:27
`sudo tccutil reset Ubiquity` (Did not restore after re-setting)
```
kTCCServiceUbiquity|/System/Library/PrivateFrameworks/PhotoLibraryServices.framework/Versions/A/Support/photolibraryd|2
kTCCServiceUbiquity|com.apple.Preview|2
kTCCServiceUbiquity|com.apple.QuickTimePlayerX|2
kTCCServiceUbiquity|com.apple.TextEdit|2
kTCCServiceUbiquity|com.apple.finder|2
kTCCServiceUbiquity|com.apple.iBooksX|0
kTCCServiceUbiquity|com.apple.identityservicesd|2
```

##### Reset All:
09/11/2024 19:29
```sh
sudo tccutil reset All
```

09/11/2024 19:29
re-enabled FullDiskAccess for com.apple.Terminal

Remaining to check after reboot:
```sh
$ sudo sqlite3 /Library/Application\ Support/com.apple.TCC/TCC.db "SELECT service, client, auth_value FROM access;"

kTCCServiceSystemPolicyAllFiles|com.apple.Terminal|2

$ sudo sqlite3 ~/Library/Application\ Support/com.apple.TCC/TCC.db "SELECT service, client, auth_value FROM access;"

kTCCServiceLiverpool|com.apple.sociallayerd|2
kTCCServiceLiverpool|com.apple.securityd|2
kTCCServiceLiverpool|com.apple.cloudpaird|2
kTCCServiceLiverpool|com.apple.identityservicesd|2
kTCCServiceLiverpool|com.apple.shortcuts|2
kTCCServiceLiverpool|com.apple.syncdefaultsd|2
kTCCServiceLiverpool|com.apple.knowledge-agent|2
kTCCServiceLiverpool|com.apple.assistant.assistantd|2
kTCCServiceLiverpool|com.apple.textinput.KeyboardServices|2
kTCCServiceLiverpool|com.apple.willowd|2
kTCCServiceLiverpool|com.apple.donotdisturbd|2
kTCCServiceLiverpool|com.apple.siriknowledged|2
kTCCServiceLiverpool|com.apple.passd|2
kTCCServiceLiverpool|com.apple.transparencyd|2
kTCCServiceLiverpool|com.apple.amsengagementd|2
kTCCServiceLiverpool|com.apple.icloud.fmfd|2
kTCCServiceLiverpool|com.apple.Passbook|2
```

After Reboot:
```sh
$ sudo sqlite3 /Library/Application\ Support/com.apple.TCC/TCC.db "SELECT service, client, auth_value FROM access;"

kTCCServiceSystemPolicyAllFiles|com.apple.Terminal|2

$ sudo sqlite3 ~/Library/Application\ Support/com.apple.TCC/TCC.db "SELECT service, client, auth_value FROM access;"

kTCCServiceLiverpool|com.apple.sociallayerd|2
kTCCServiceLiverpool|com.apple.securityd|2
kTCCServiceLiverpool|com.apple.cloudpaird|2
kTCCServiceLiverpool|com.apple.identityservicesd|2
kTCCServiceLiverpool|com.apple.shortcuts|2
kTCCServiceLiverpool|com.apple.syncdefaultsd|2
kTCCServiceLiverpool|com.apple.knowledge-agent|2
kTCCServiceLiverpool|com.apple.assistant.assistantd|2
kTCCServiceLiverpool|com.apple.textinput.KeyboardServices|2
kTCCServiceLiverpool|com.apple.willowd|2
kTCCServiceLiverpool|com.apple.donotdisturbd|2
kTCCServiceLiverpool|com.apple.siriknowledged|2
kTCCServiceLiverpool|com.apple.passd|2
kTCCServiceLiverpool|com.apple.transparencyd|2
kTCCServiceLiverpool|com.apple.amsengagementd|2
kTCCServiceLiverpool|com.apple.icloud.fmfd|2
kTCCServiceLiverpool|com.apple.Passbook|2

kTCCServiceUbiquity|com.apple.identityservicesd|2
kTCCServiceUbiquity|com.apple.finder|2
kTCCServiceLiverpool|com.apple.protectedcloudstorage.protectedcloudkeysyncing|2
kTCCServiceUbiquity|/System/Library/PrivateFrameworks/PhotoLibraryServices.framework/Versions/A/Support/photolibraryd|2
```

---
### AdhocSignatureCache 

macOS stores TCC (Transparency, Consent, and Control) user consent data in the following locations:
- `/Library/Application Support/com.apple.TCC/`
- `/Users/*/Library/Application Support/com.apple.TCC/`

Within these directories, you'll find:
- `TCC.db` (a SQLite database)
- `AdhocSignatureCache` (a directory containing files related to app signatures)

**Purpose of AdhocSignatureCache:**
- Manages and verifies app signatures to ensure that only trusted apps access sensitive data and system features.
- Speeds up the verification process by caching digital signatures, reducing the need for repeated checks.

#### Understanding UUIDs in AdhocSignatureCache

**1. UUIDs in AdhocSignatureCache**
- UUIDs are unique identifiers for app signatures.
- Each UUID file contains a "Mac OS X Detached Code Signature" for a specific app.
- The `keys` file in this directory maps UUIDs to app-related data.

**2. Inspecting the `keys` File**
- The `keys` file is a plist (property list) that provides metadata about cached signatures.
  
  **Using `plutil` Command:**
  ```bash
  plutil -convert xml1 /Library/Application\ Support/com.apple.TCC/AdhocSignatureCache/keys -o - | less
  ```
  - This converts the `keys` file to an XML format for easier reading.
  - Look for UUIDs and associated metadata to identify which apps they correspond to.

---
### Logging

**TCC / Privacy-related logging** 

Filter log entries related to the TCC subsystem:
```sh
log show --debug  --info --predicate 'subsystem == "com.apple.TCC" AND eventMessage BEGINSWITH "AttributionChain"'
```
**Attribution Chain**: In the context of TCC, the attribution chain often refers to how permissions and privacy-related requests are tracked and attributed to specific apps or processes. This can involve logging details about which applications are requesting permissions and how these requests are handled or attributed within the system.

Search the log for entries containing `kTCCServiceLiverpool`:
```sh
log show --predicate 'eventMessage contains "kTCCServiceLiverpool"' --info --debug
```

Use specific predicates:
```sh
log show --predicate 'eventMessage contains "com.apple.upload-request-proxy.com.apple.photos.cloud"' --info --debug
```

Check `/private/var/log/system.log`:
```sh
grep "kTCCServiceLiverpool" /private/var/log/system.log
```

Example Log Search Command:
```sh
log show --predicate 'eventMessage contains "kTCCServiceLiverpool" AND eventMessage contains "com.apple.upload-request-proxy.com.apple.photos.cloud"' --info --debug
```

---
### TCC Privacy Policy Configuration Profiles

**Find CodeRequirement details for a TCC Privacy Policy Configuration Profile** 
```sh
codesign --display -r- /path/to/app
```
Then parse the contents of the `designated =>` attribute

**Example:**
```sh
$ codesign --display -r- /Applications/Brave\ Browser.app 
Executable=/Applications/Brave Browser.app/Contents/MacOS/Brave Browser
designated => identifier "com.brave.Browser" and anchor apple generic and certificate 1[field.1.2.840.113635.100.6.2.6] /* exists */
```

Alternatively, use:
* [appblock.py](https://gist.github.com/cspence001/e8ef293a1a70d58d6895399483371788) Script to extract App Info for fields required for Custom XML Payload
* `appblock.py --list`
* `appblock.py --app `

**Usage in XML Configuration Profile**:
```sh
<?xml version="1.0" encoding="UTF-8"?>
<plist version="1.0">
<dict>
    <key>PayloadContent</key>
    <array>
        <dict>
            <key>PayloadContent</key>
            <array>
                <dict>
                    <key>PayloadType</key>
                    <string>com.apple.TCC.configuration-profile-policy</string>
                    <key>PayloadUUID</key>
                    <string>EXAMPLE-UUID-1</string>
                    <key>PayloadDisplayName</key>
                    <string>Brave Browser Bluetooth Access</string>
                    <key>PayloadIdentifier</key>
                    <string>com.example.tcc.brave.bluetooth</string>
                    <key>PayloadOrganization</key>
                    <string>Example Organization</string>
                    <key>PayloadVersion</key>
                    <integer>1</integer>
                    <key>Allowed</key>
                    <array>
                        <dict>
                            <key>Services</key>
                            <array>
                                <string>com.apple.bluetooth</string>
                            </array>
                            <key>Identifiers</key>
                            <array>
                                <string>com.brave.Browser</string>
                            </array>
                            <key>CodeRequirement</key>
                            <string>identifier "com.brave.Browser" and anchor apple generic and certificate 1[field.1.2.840.113635.100.6.2.6] /* exists */</string>
                        </dict>
                    </array>
                </dict>
            </array>
            <key>PayloadType</key>
            <string>Configuration</string>
            <key>PayloadUUID</key>
            <string>EXAMPLE-UUID-2</string>
            <key>PayloadDisplayName</key>
            <string>Example TCC Policy Profile</string>
            <key>PayloadIdentifier</key>
            <string>com.example.tcc</string>
            <key>PayloadOrganization</key>
            <string>Example Organization</string>
            <key>PayloadVersion</key>
            <integer>1</integer>
        </dict>
    </array>
    <key>PayloadDescription</key>
    <string>Configuration profile to grant Bluetooth access to Brave Browser.</string>
    <key>PayloadType</key>
    <string>Configuration</string>
    <key>PayloadUUID</key>
    <string>EXAMPLE-UUID-3</string>
    <key>PayloadDisplayName</key>
    <string>Brave Browser TCC Configuration</string>
    <key>PayloadIdentifier</key>
    <string>com.example.tcc.brave-config</string>
    <key>PayloadOrganization</key>
    <string>Example Organization</string>
    <key>PayloadVersion</key>
    <integer>1</integer>
</dict>
</plist>
```
Using the `CodeRequirement` in the profile, you ensure that only the Brave Browser app, signed with the specified identifier and certificate, receives the Bluetooth permissions



---
### More Reference Articles

- [Cached Medium Article on Macintosh Network Traffic](https://webcache.googleusercontent.com/search?q=cache:https://medium.com/cloud-security/apple-macintosh-network-traffic-2b172d084fd)
- [MORE Program Listings](https://github.com/drduh/macOS-Security-and-Privacy-Guide/blob/master/launchd/comments.csv)
- [Privacy: what TCC does and doesn’t](https://eclecticlight.co/2023/02/10/privacy-what-tcc-does-and-doesnt/)
- [TCC Database Schema](https://www.rainforestqa.com/blog/macos-tcc-db-deep-dive) - Review
- [Managing TCC.db with tccutil and sqlite](https://jetforme.org/2023/12/transparency-consent-control/)
- [comment on macOS layers of access restrictions/permissions](https://apple.stackexchange.com/a/430592) - Review
- [comment on checking Full Disk Access permissions](https://apple.stackexchange.com/a/398389)
- [sshd-keygen-wrapper](https://medium.com/@utkarsh.kapoor/advanced-ci-cd-on-headless-macos-ec2-navigating-sip-and-tcc-db-for-full-disk-access-in-gitlab-4fa98e32ac00)
- [TCC directories/bypasses](https://github.com/HackTricks-wiki/hacktricks/tree/master/macos-hardening/macos-security-and-privilege-escalation/macos-security-protections/macos-tcc) Thorough & Detailed Info on TCC.db - Review


### 3-Party Tools for Interacting with TCC.db
* [tccplus](https://github.com/jslegendre/tccplus) - (SIP needs to be disabled)
* [tcc.sh](https://gist.github.com/lechium/0281d6e2ea2d57153b01589a39e5471f -) - script to add access


PearCleaner
https://www.reddit.com/r/macapps/comments/17ukqqt/comment/k977liv/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button