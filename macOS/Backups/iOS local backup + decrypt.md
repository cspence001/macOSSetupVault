
### [dump-iphone-backup](https://github.com/PeterUpfold/dump-iphone-backup) Full Local Backup Decryption 
* Wraps [iphone-backup-decrypt](https://github.com/jsharkey13/iphone_backup_decrypt) to decrypt whole backup on command-line

**Installation**
```sh
git clone https://github.com/PeterUpfold/dump-iphone-backup.git
cd dump-iphone-backup
pipenv install -r requirements
pipenv shell
```

May receive error about #egg, change requirements file:
`requirements.txt`
```plaintext
git+https://github.com/jsharkey13/iphone_backup_decrypt#egg=iphone_backup_decrypt
```
Then again run:
```sh
pipenv install -r requirements
pipenv shell
```

**Decryption**
Create directory to store decrypted backup
```sh
mkdir ~/decrypted_backup
```
* Note: Ensure sufficient space exists on your computer to store full decrypted backup.
`System Settings` > `General` > `Storage`

To decrypt full backup:
```sh
 ./dump-iphone-backup.py -b ~/Library/Application\ Support/MobileSync/Backup/SOME-GUID/ -o ~/decrypted_backup/ 
 # This will prompt interactively for the backup passphrase

 ./dump-iphone-backup.py -b ~/Library/Application\ Support/MobileSync/Backup/SOME-GUID/ -o ~/decrypted_backup/ -p some-passphrase
 # If comfortable, you can pass the passphrase on the command line

 # Or, set an environment variable for the passphrase:
 export DUMP_IPHONE_BACKUP_PASSPHRASE='some-passphrase'
 ./dump-iphone-backup.py -b ~/Library/Application\ Support/MobileSync/Backup/SOME-GUID/ -o ~/decrypted_backup/ 
```

**command options:**
| -p | --passphrase | The passphrase of the backup. If not passed, the tool will look for `DUMP_IPHONE_BACKUP_PASSPHRASE` in the environment, or prompt interactively for the passphrase | 
| -b | --backup-path | The path to the backup directory (~/Library/Application Support/MobileSync/EXAMPLE) | 
| -o | --output-path | The path of the output directory where the flat files will be dumped. | 
| -z | --no-create-parent-dirs | Do not create any parent directories required to create the output directory | 
| -e | --remove-empty-domains | Remove any directories for backup domains that do not contain any files |


---

### [iOSbackup](https://github.com/avibrazil/iOSbackup)
To decrypt parts of backup.

**Installation + Environment Set-up**
```sh
mkdir ios-backup-package
cd ios-backup-package
pipenv install iOSbackup
pipenv shell
```

Ensure Terminal has Full Disk Access:
`System Preferences > Security & Privacy > Full Disk Access`
Note: may need to enable Full Disk Access for IDE if running from IDE shell (e.g. VSCode, PyCharm, etc.)

From within shell - Create Python (or Jupyter Notebook) file for working with the tool:
```sh
touch decrypt_backup.py
```

##### Example logic

**Get Decrypt Key for faster decryption:**
```python
from iOSbackup import iOSbackup
b=iOSbackup(
	udid="00456030-000E4412342802E",
	cleartextpassword="mypassword"
)
print(b)
```

The output from above will be used for your `derivedkey` below.

**Decrypt all photos from `CameraRollDomain`, excluding `.MOV` and `.mp4` files:**
```python
from iOSbackup import iOSbackup
  
b=iOSbackup(udid="00008030-001270E93CA2802E",
derivedkey="562c875e34a31732465a9f6490be85ed947895a4ff11cb9cbc0f187fa5e918f7")

b.getFolderDecryptedCopy(
	'Media',
	targetFolder='restored-photos',
	includeDomains='CameraRollDomain',
	excludeFiles=['%.MOV', '%.mp4']
)
```

**Decrypt `HomeDomain`:**
```python
 b.getFolderDecryptedCopy(
	includeDomains='HomeDomain',
	targetFolder='my-folder',
)
```

[[#List of Domains]]

##### To restore all backed up data files of an app, search for its domain name (see below) and use this technique:

**Get List of All Installed Apps:**
```python
>>> apps=list(b.manifest['Applications'].keys())
>>> apps
['group.com.apple.Maps',
 'group.net.whatsapp.family',
 'it.joethefox.XBMC-Remote',
 'com.google.GoogleMobile.NotificationContentExtension',
 'group.com.dendrocom.uniconsole',
 ...
]
```

**Get List of All Apps, Groups and Plugins with “_whatsapp_” In Their Name:**
```python
>>> [s for s in list(b.manifest['Applications'].keys()) if "whatsapp" in s]
['group.net.whatsapp.WhatsApp.shared',
 'net.whatsapp.WhatsApp.ShareExtension',
 'group.net.whatsapp.WhatsAppSMB.shared',
 'net.whatsapp.WhatsApp.NotificationExtension',
 'net.whatsapp.WhatsApp.Intents',
 'net.whatsapp.WhatsApp.TodayExtension',
 'net.whatsapp.WhatsApp.IntentsUI',
 'group.net.whatsapp.WhatsApp.private',
 'net.whatsapp.WhatsApp.ServiceExtension',
 'net.whatsapp.WhatsApp']
```

**Restore All Files of Apps, Groups and Plugins Matching “_whatsapp_”:**

I had to use previous method to find the list of app IDs and see that “_whatsapp_” is a good word to match them. Each app component has its own backup domain prefixed by `AppDomain`, `AppDomainGroup` or `AppDomainPlugin`. So we'll iterate over all possibilities of fabricated domain names with `getFolderDecryptedCopy()`.

```python
for id in [s for s in list(b.manifest['Applications'].keys()) if "whatsapp" in s]:
    for prefix in ["AppDomain", "AppDomainGroup", "AppDomainPlugin"]:
        b.getFolderDecryptedCopy(includeDomains=prefix + '-' + id)
```

Other apps might have a less intuitive name. For example, Telegram can be matched by “_telegra_” (without ‘m’):

```python
for id in [s for s in list(b.manifest['Applications'].keys()) if "telegra" in s]:
    for prefix in ["AppDomain", "AppDomainGroup", "AppDomainPlugin"]:
        b.getFolderDecryptedCopy(includeDomains=prefix + '-' + id)
```

---

##### Notes 
Read a copy of iOS Notes backup on macOS Notes application.

Back-up Notes application files to `Notes-Decrypted-folder`:
```python
b.getFolderDecryptedCopy(
	includeDomains=AppDomainGroup-group.com.apple.notes,
	targetFolder='Notes-Decrypted-folder'
)
```

After decrypting this domain you will find the following 3 files:
```
Notes-Decrypted-folder/AppDomainGroup-group.com.apple.notes/NoteStore.sqlite-shm
Notes-Decrypted-folder/AppDomainGroup-group.com.apple.notes/NoteStore.sqlite-wal
Notes-Decrypted-folder/AppDomainGroup-group.com.apple.notes/NoteStore.sqlite
```

Make a backup of your macOS notes:
```sh
mkdir ~/folder/macsetup/_backups/Notes_macos_backup
cp ~/Library/Group\ Containers/group.com.apple.notes/NoteStore.sqlite* ~/folder/macsetup/_backups/Notes_macos_backup/
```

Replace with new files from iOS backup from the `Notes-Decrypted-folder`:
```sh
cp -pf Notes-Decrypted-folder/AppDomainGroup-group.com.apple.notes/NoteStore.sqlite* ~/Library/Group\ Containers/group.com.apple.notes/
```
* `-f` forces a replace of original files
* `-p` preserve the original permissions, timestamps, and ownership when copying the files.

From here you can read your iOS notes from the Notes Application on macOS. 

**To restore original macOS notes:**
```sh
cp -f ~/folder/macsetup/_backups/Notes_macos_backup/NoteStore.sqlite* ~/Library/Group\ Containers/group.com.apple.notes/
```

**Copy media files (images) in notes**
```sh
find ~/folder/macsetup/_packages/ios_backup/Notes-Decrypted-folder/AppDomainGroup-group.com.apple.notes/Media -name "*.png" -exec cp {} ~/folder/macsetup/_backups/NoteImages/ \;
```

**Error checking:**

**If backup notes do not open in Notes app, run Integrity check for corrupted database:**
```sh
sqlite3 ~/Library/Group\ Containers/group.com.apple.notes/Backups/Backup-2024-09-26_18-01-17-D2129E30-8804-4599-B2AD-3C1A92245CA3/NoteStore.sqlite "PRAGMA integrity_check;"
```

**Recover Process for corrupted database files:**
- Make a copy of the iOS backup original (corrupted/damaged) NoteStore.sqlite, work on this copy.
```
sqlite3 CorruptedNoteStore.sqlite ".recover" | sqlite3 RecoveredNoteStore.sqlite
```

- Rename the `RecoveredNoteStore.sqlite` to `NoteStore.sqlite` and paste it where the original one was in the `~/Library/Group Containers/group.com.apple.notes` folder, leaving the other two original files (`shm` and `wal`).
- Restart your MacBook, and then re-open Notes App. If notes still don't appear - replace the `wal` file with copy of the iOS backup `wal` file. Close and re-open Notes App. 
[sqlite3 .recover Notes](https://apple.stackexchange.com/a/408095)

**Logging for other errors:**
Filtering the historical log data using "process == "Notes" OR process == "syncdefaultsd" OR process == "syncserver"":
```sh
$ log show --predicate 'process == "Notes" OR process == "syncdefaultsd" OR process == "syncserver"' --info --start "2024-09-19 16:00:00" --end "$(date +'%Y-%m-%d %H:%M:%S')"
```

Run this Log stream after replacing NoteStore.sqlite files from backup into Notes group container, monitor when attempting to open Notes app with backup files:
```sh
$ log stream --predicate 'process == "Notes" OR process == "syncdefaultsd" OR process == "syncserver"' --info
```


---

##### SMS/ iMessage

[sql query for sms.db](https://gist.githubusercontent.com/aaronhoffman/cc7ee127f00b6b5462fa7fc742c23d4f/raw/f5afe2caa335e478ea492a0c47ce122a8f461b44/iphone-text-message-sqlite.sql)
- After decrypting HomeDomain, navigate to `SMS > sms.db`
- Open the `sms.db` in [DB Browser for SQLite.app](https://github.com/sqlitebrowser/sqlitebrowser)
- Navigate to execute SQL query, copy and paste the above query and Run. 
* updated query to retrieve missing to/from numbers:
```sql
SELECT
    m.rowid,
    COALESCE(m.cache_roomnames, h.id) AS ThreadId,
    m.is_from_me AS IsFromMe,
    CASE 
        WHEN m.is_from_me = 1 THEN m.account  -- Sender's account (FromPhoneNumber)
        ELSE h.id 
    END AS FromPhoneNumber,
    CASE 
        WHEN m.is_from_me = 0 THEN m.account  -- Receiver's account (ToPhoneNumber)
        ELSE COALESCE(h2.id, h.id) 
    END AS ToPhoneNumber,
    m.service AS Service,
    datetime((m.date / 1000000000) + 978307200, 'unixepoch', 'localtime') AS TextDate,
    m.text AS MessageText,
    c.display_name AS RoomName
FROM
    message AS m
LEFT JOIN
    handle AS h ON m.handle_id = h.rowid
LEFT JOIN
    chat_message_join AS cm ON m.rowid = cm.message_id
LEFT JOIN
    chat AS c ON cm.chat_id = c.rowid
LEFT JOIN
    chat_handle_join AS ch ON c.rowid = ch.chat_id
LEFT JOIN
    handle AS h2 ON ch.handle_id = h2.rowid
WHERE
    (h2.service IS NULL OR m.service = h2.service)
ORDER BY
    ThreadId,
    m.date;
```

[modifying iOS sms database](https://medium.com/@thehackpatrol/modifying-ios-sms-database-93a3f64f8c94) hack to edit `sms.db`

[iOS sms viewer](https://github.com/alexandreblin/ios-sms-viewer) view decrypted `sms.db` in html webpage (no images)
**Note:** update `timestampToDate` function in `smsviewer.py` before running.

```python
def timestampToDate(self, timestamp):
    if timestamp < 0:
        return "Invalid Date"  # Handle any negative timestamps
    # Convert from nanoseconds to seconds
    timestamp_in_seconds = timestamp / 1_000_000_000
    # Adjust for the custom epoch
    adjusted_timestamp = timestamp_in_seconds + 978307200
    # Convert to a datetime object
    return datetime.datetime.fromtimestamp(adjusted_timestamp)
```

- image attachments, formatting

---

##### [iOS Backup Overview](https://github.com/A-DONALD/iBackup-Extractor/wiki/Iphone-backup)

Found in `~/Library/Application\ Support/MobileSync/Backup/<udid>`
- `Info.plist` (plain text plist) stores data about the backed up device (such as the device name, GUID, ICC-ID, IMEI, and serial number) and the iTunes software used to build the backup (such as iTunes settings and the iTunes version number).
- `Manifest.plist` (binary plist) describes the contents of the backup
- `Status.plist` (binary plist) seems to store information about the status of the backup itself, such as whether the backup is complete or not
- `Manifest.db` : file in an iPhone backup is a SQLite database that contains metadata about the various files and directories that are stored in the backup. 
- Headers in Manifest.db database :

| Header       | Description                                                                                                  |
| ------------ | ------------------------------------------------------------------------------------------------------------ |
| fileID       | The numbers and letters under fileID are the file names in the backup as in the folders with the manifest.db |
| domain       | What sandbox the file is in                                                                                  |
| relativePath | File relative to domain                                                                                      |
| flags        | Unix file flags?                                                                                             |
| file         | Binary plist containing poperties                                                                            |
`backupFile` is the file name on your computer. Basically SHA1([Domain]/[FilePath]).
`name` is the original semi-complete path of file name in the device.
`domain` is the file group this file is member, see below list of domains.
`file` is binary plist content with some file metadata.
`relativePath` the actual file or folder that resided on the device 
`fileID` correlate to a folder and a file in the directory where the backups are stored. Encoded as sha-1 hashes: sha-1(domain + ‘-’ + relative_filename)
`flags`shows if the item is a file or a folder: 1 = File, 2 = Folder, 4 = Unknown, and not actually present in the backup folders.

##### Overview of Domains

|iOS backup domain|Data found in this domain|
|---|---|
|Application Groups|Data stored here by apps is able to be shared slightly more freely on the device. Many applications will store a small amount of data here, but rarely will you see valuable content.|
|Application Plugins|Apps that store data here usually do so as part of an iOS extension. That may be as a Watch plugin, a third-party keyboard, a widget, a sharing extension, or an Animoji. This domain rarely contains user data.|
|Applications|This is the core domain for apps to store user data on an iPhone. Each app has its own namespace, as described in the “What’s in a namespace?” section above.|
|Camera Roll|The camera roll domain includes all photos, videos and metadata stored on an iOS device. If the device is configured to “Optimise Storage”, and not to store all photos locally, this domain may contain less information than one expects.|
|Databases|Little information is stored here on modern installs of iOS.|
|Health|HealthKit and medical data is stored in the Health domain, along with activity data being shared with a paired Apple Watch.|
|Home|The Home domain is a goldmine of information for many of Apple’s built-in applications, such as Messages, Notes and Calendar.|
|HomeKit|This domain stores a limited amount of information on the state of Apple HomeKit configuration.|
|Install|The Install domain contains metadata to indicate which built-in Apple apps are installed on the iOS device.|
|Keyboard|Language and keyboard configuration is stored in this domain.|
|Keychain|The iOS device’s keychain (a collection of user passwords) is stored here, in a SQLite file.|
|Managed Preferences|This domain contains data around the management of the iOS device. If your device is enrolled in an MDM (mobile device management) program, perhaps by your school or employer, some metadata will be stored here.|
|Media|Many types of media are stored under this domain. For instance, users will find attachments to SMS messages, recordings, and PhotoStream data here.|
|Root|The root domains contains fundamental configuration files for the setup of the iOS device.|
|System Containers|The system containers domain contains limited metadata from the App Store app, and some other iOS processes.|
|System Preferences|This domain contains low-level information on an iOS device’s status. For instance, you can learn about its networking configuration, or about the wi-fi networks or VPNs it has recently connected to.|
|System Shared Containers|Some iOS system processes which can share data across iOS store their data here. It is a good place to start when looking to learn more about Bluetooth activity on a device, for instance.|
|Wireless|The wireless domain contains a rich set of information on iOS’s use of cellular and wi-fi networks, and of its recent IP address assignations.|

##### List of Domains
https://github.com/avibrazil/iOSbackup?tab=readme-ov-file#list-of-domains

|Domain|Contains|
|---|---|
|**AppDomain-...**|Backup of each installed app's files|
|**AppDomainGroup-...**|Backup of each installed app's files|
|**AppDomainPlugin-...**|Backup of each installed app's files|
|**CameraRollDomain**|Photos|
|**DatabaseDomain**||
|**HealthDomain**|Health app databases|
|**HomeDomain**|Many interesting databases, such as contacts and call history|
|**HomeKitDomain**||
|**InstallDomain**||
|**KeyboardDomain**||
|**KeychainDomain**||
|**ManagedPreferencesDomain**||
|**MediaDomain**||
|**MobileDeviceDomain**||
|**RootDomain**||
|**SysContainerDomain-com.apple.Preferences.SettingsSpotlightIndexExtension**||
|**SysContainerDomain-com.apple.Preferences.indexSettingsManifests**||
|**SysContainerDomain-com.apple.accessibility.AccessibilityUIServer**||
|**SysContainerDomain-com.apple.adid**||
|**SysContainerDomain-com.apple.akd**||
|**SysContainerDomain-com.apple.appstored**||
|**SysContainerDomain-com.apple.apsd**||
|**SysContainerDomain-com.apple.backboardd**||
|**SysContainerDomain-com.apple.fairplayd.H2**||
|**SysContainerDomain-com.apple.geod**||
|**SysContainerDomain-com.apple.icloud.findmydeviced**||
|**SysContainerDomain-com.apple.icloud.ifccd**||
|**SysContainerDomain-com.apple.icloud.searchpartyd**||
|**SysContainerDomain-com.apple.lsd**||
|**SysContainerDomain-com.apple.lskdd**||
|**SysContainerDomain-com.apple.metrickitd**||
|**SysContainerDomain-com.apple.mobilesafari**|Bookmarks and cookies ?|
|**SysContainerDomain-com.apple.springboard**||
|**SysSharedContainerDomain-systemgroup.com.apple.AssetCacheServices.diskCache**||
|**SysSharedContainerDomain-systemgroup.com.apple.DiagnosticsKit**||
|**SysSharedContainerDomain-systemgroup.com.apple.ReportMemoryException**||
|**SysSharedContainerDomain-systemgroup.com.apple.VideoSubscriberAccount**||
|**SysSharedContainerDomain-systemgroup.com.apple.WiFiAssist**||
|**SysSharedContainerDomain-systemgroup.com.apple.bluetooth**||
|**SysSharedContainerDomain-systemgroup.com.apple.cfpreferences.managed**||
|**SysSharedContainerDomain-systemgroup.com.apple.configurationprofiles**||
|**SysSharedContainerDomain-systemgroup.com.apple.coreanalytics**||
|**SysSharedContainerDomain-systemgroup.com.apple.icloud.findmydevice.managed**||
|**SysSharedContainerDomain-systemgroup.com.apple.icloud.fmipcore.MockingContainer**||
|**SysSharedContainerDomain-systemgroup.com.apple.icloud.ifccd**||
|**SysSharedContainerDomain-systemgroup.com.apple.icloud.searchpartyd.sharedsettings**||
|**SysSharedContainerDomain-systemgroup.com.apple.itunesu.shared**||
|**SysSharedContainerDomain-systemgroup.com.apple.lsd**||
|**SysSharedContainerDomain-systemgroup.com.apple.lsd.iconscache**||
|**SysSharedContainerDomain-systemgroup.com.apple.lskdrl**||
|**SysSharedContainerDomain-systemgroup.com.apple.media.books.managed**||
|**SysSharedContainerDomain-systemgroup.com.apple.media.shared.books**||
|**SysSharedContainerDomain-systemgroup.com.apple.mobile.installationhelperlogs**||
|**SysSharedContainerDomain-systemgroup.com.apple.mobilegestaltcache**||
|**SysSharedContainerDomain-systemgroup.com.apple.nsurlstoragedresources**||
|**SysSharedContainerDomain-systemgroup.com.apple.ondemandresources**||
|**SysSharedContainerDomain-systemgroup.com.apple.osanalytics**||
|**SysSharedContainerDomain-systemgroup.com.apple.sharedpclogging**||
|**SystemPreferencesDomain**||
|**TonesDomain**||
|**WirelessDomain**|

##### List of Interesting Files

| Backup file                              | Domain           | File name                                                           | Contains                                                                                                                                                                                                                               |
| ---------------------------------------- | ---------------- | ------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ed1f8fb5a948b40504c19580a458c384659a605e | WirelessDomain   | Library/Databases/CellularUsage.db                                  | Table `subscriber_info` apparently contains all SIM phone numbers ever inserted in the phone since about iOS 11 or 12. Data here is related to `Library/Preferences/com.apple.commcenter.plist`.                                       |
| 0d609c54856a9bb2d56729df1d68f2958a88426b | WirelessDomain   | Library/Databases/DataUsage.sqlite                                  | A rich database that apparently contains app WWAN usage through time. Chack tables `ZPROCESS` and `ZLIVEUSAGE`.                                                                                                                        |
| 1570a95f5dc7f4cd6b54bc17c427eda95288b8fa | HomeDomain       | Library/SpringBoard/LockVideo.mov                                   | Video used as background on lock screen                                                                                                                                                                                                |
| .                                        | HomeDomain       | Library/Passes/Cards/*                                              | Wallet passes and items                                                                                                                                                                                                                |
| 8d0167b67f664a3816b4c00115c2dfa6a8f81388 | WirelessDomain   | Library/Preferences/com.apple.AppleBasebandManager.Statistics.plist | Apparently contains times of last boot and restores.                                                                                                                                                                                   |
| dafec408e48be2700704dd3e763014c39f6de6b3 | WirelessDomain   | Library/Preferences/com.apple.AppleBasebandManager.plist            |                                                                                                                                                                                                                                        |
| 9329979c8298f9cd3fb110fa387570a8b957e912 | WirelessDomain   | Library/Preferences/com.apple.CommCenter.counts.plist               | Has `CellularBytesRecved` and `CellularBytesSent`                                                                                                                                                                                      |
| 3dec38ca46c9e37ffebacf2611463eb47a65eb09 | WirelessDomain   | Library/Preferences/com.apple.commcenter.audio.plist                |                                                                                                                                                                                                                                        |
| 7e5f642f6da5e2345c0893bdf944da9c53902756 | WirelessDomain   | Library/Preferences/com.apple.commcenter.callservices.plist         |                                                                                                                                                                                                                                        |
| bfecaa9c467e3acb085a5b312bd27bdd5cd7579a | WirelessDomain   | Library/Preferences/com.apple.commcenter.plist                      | Cellular network informations and configurations, including all ever inserted SIM and eSIM cards, their phone numbers and nicknames as configured under Settings➔Celular. Data here is related to `Library/Databases/CellularUsage.db` |
| 160600e9c2e408c69e4193d325813a2a885bce2a | WirelessDomain   | Library/Preferences/com.apple.ipTelephony.plist                     |                                                                                                                                                                                                                                        |
| 12b144c0bd44f2b3dffd9186d3f9c05b917cee25 | CameraRollDomain | Media/PhotoData/Photos.sqlite                                       | Photo library data, albums, tagged faces, moments, places, geolocations, date and times, formats etc.                                                                                                                                  |
| .                                        | CameraRollDomain | Media/DCIM/*APPLE                                                   | Original master photos/videos taken by your camera or imported through AirDrop etc                                                                                                                                                     |
| .                                        | CameraRollDomain | Media/PhotoData/Mutations/DCIM/*APPLE                               | Edited version of photos/videos                                                                                                                                                                                                        |
| .                                        | HomeDomain       | Library/Mobile Documents/iCloud~...                                 | Apps documents on iCloud                                                                                                                                                                                                               |
| .                                        | HomeDomain       | Library/Mobile Documents/com~apple~CloudDocs/...                    | Documents folder on iCloud                                                                                                                                                                                                             |
| 5a4935c78a5255723f707230a451d79c540d2741 | HomeDomain       | Library/CallHistoryDB/CallHistory.storedata                         | Call History database with only the last 600 calls                                                                                                                                                                                     |
| 31bb7ba8914766d4ba40d6dfb6113c8b614be442 | HomeDomain       | Library/AddressBook/AddressBook.sqlitedb                            | User contacts and address book. Table `ABPerson` is the central one with facts about contact creation and modification, while `ABMultiValue*` tables contain contact details.                                                          |
| cd6702cea29fe89cf280a76794405adb17f9a0ee | HomeDomain       | Library/AddressBook/AddressBookImages.sqlitedb                      | Contact photos                                                                                                                                                                                                                         |
| 9db3e5a6f1672cc306cd785809811e79cc43a2f8 | HomeDomain       | Library/AddressBook/backup/AddressBook.sqlitedb                     |                                                                                                                                                                                                                                        |
| 2a87d5bcdb9753f1462dd1e929b17e6a971c5b01 | HomeDomain       | Library/AddressBook/backup/AddressBookImages.sqlitedb               |                                                                                                                                                                                                                                        |
| 1a0e7afc19d307da602ccdcece51af33afe92c53 | HomeDomain       | Library/Safari/History.db                                           |                                                                                                                                                                                                                                        |
| .                                        | MediaDomain      | Library/SMS/Attachments/*                                           |                                                                                                                                                                                                                                        |
| .                                        | MediaDomain      | Library/SMS/StickerCache/*                                          |                                                                                                                                                                                                                                        |
| .                                        | .                | Library/Caches/locationd/consolidated.db                            | Apparently list of known iBeacons                                                                                                                                                                                                      |

---

##### More iOS backup decryption tools

- [KnugiHK/iphone_backup_decrypt](https://github.com/KnugiHK/iphone_backup_decrypt/tree/master), a fork of [iphone-backup-decrypt](https://github.com/jsharkey13/iphone_backup_decrypt) and part of [Whatsapp-Chat-Exporter](https://github.com/KnugiHK/Whatsapp-Chat-Exporter). Decrypts local backup selectively based on relativePath of file. + WhatsApp Database parser. 
- [jfarley248/iTunes_Backup_Reader](https://github.com/jfarley248/iTunes_Backup_Reader), uses an older version of [iphone-backup-decrypt](https://github.com/jsharkey13/iphone_backup_decrypt). Alternative to [dump-iphone-backup](https://github.com/PeterUpfold/dump-iphone-backup/tree/main).
- [datatags/mount-ios-backup](https://github.com/datatags/mount-ios-backup), uses an older version of [iphone-backup-decrypt](https://github.com/jsharkey13/iphone_backup_decrypt), mount the backup as a [FUSE filesystem](https://github.com/nrclark/pyfuse) to selectively access and recover files. Read-only.
- [MaxiHuHe04/iTunes-Backup-Explorer](https://github.com/MaxiHuHe04/iTunes-Backup-Explorer), a Java based alternative with a GUI. Can extract and replace files from encrypted and non-encrypted iOS backups, uses an older version of [iphone-backup-decrypt](https://github.com/jsharkey13/iphone_backup_decrypt). *Works on actual backup file (doesn't create copy) so make sure to create copy of your backup first before using in case of accidental file corruption.*