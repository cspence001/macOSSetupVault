lsregister
lsappinfo
launchctl
pbs

### **Launch Services Commands**

#### **Aliases for Resetting Launch Services**

- **General Cleanup and Rebuild:**
  ```bash
  # Comprehensive reset of Launch Services database, including verbose output and restarting Finder.
  alias cleanupLS="/System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -kill -r -v -all u,s,l && killall Finder"
  ```

- **Comprehensive Reset with Seed and Lint:**
  ```bash
  # Comprehensive reset and rebuild of Launch Services database, including seed reset and linting, for all specified domains.
  alias resetLaunchServices="sudo /System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -kill -seed -lint -r -domain local -domain system -domain user -domain network"
  ```

- **Full Reset with Detailed Dump and Diagnostics:**
  ```bash
  # Thorough reset and diagnostic of Launch Services database with detailed dump, seed reset, linting, and rebuild for all specified domains.
  alias fullLaunchServicesReset="sudo /System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -kill -seed -lint -r -f -v -dump -domain local -domain system -domain user -domain network"
  ```

#### **Dumping and Inspecting Launch Services Database**

- **Basic Dump:**
  ```bash
  # Dump Launch Services database and inspect specific attributes.
  /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/LaunchServices.framework/Versions/A/Support/lsregister -dump | grep -E "path:|bindings:|name:"
  ```

- **Extended Dump:**
  ```bash
  # Extended dump including UTI bindings.
  /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/LaunchServices.framework/Versions/A/Support/lsregister -dump | grep -E "path:|bindings:|name:|uti:"
  ```

- **Full Dump to File:**
  ```bash
  # Dump the entire Launch Services database to a file.
  /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/LaunchServices.framework/Versions/A/Support/lsregister -dump > /folder/macsetup/_outputs/dump.txt
  ```

- **URL Scheme Bindings Dump:**
  ```bash
  # Dump URL scheme bindings to a file.
  lsregister -dump URLSchemeBinding > urlsb.txt
  lsregister -dump URLSchemeBinding | grep "dict*" > urlsbdict.txt
  ```

#### **Additional Arguments and Options**

- **Linting and Domain Specifications:**
  ```bash
  # Perform linting and specify domains for operation.
  lsregister -lint -domain network,user,local,system
  ```

- **Garbage Collection and Rescan:**
  ```bash
  # Perform garbage collection and rescan the database with verbose output.
  lsregister -gc -R -v -apps u,s,l
  ```

- **Domain-Specific Dump:**
  ```bash
  # Dump URL scheme bindings for a specific domain.
  lsregister -dump -all local URLSchemeBinding
  ```

- **Filtered App Dump:**
  ```bash
  # Dump URL scheme bindings for user apps with detailed attribute filtering.
  lsregister -dump -apps user URLSchemeBinding | grep -E "path:|bindings:|name:|uti:"
  ```

#### **Unregistering and Re-registering Applications**

- **Unregister Specific Paths:**
  ```bash
  # Find and unregister specific entries related to an application.
  lsregister -dump | grep path: | grep <YOUR APPLICATION NAME>
  lsregister -u <Path>
  ```

- **Unregistering Applications:**
```sh
/System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/LaunchServices.framework/Versions/A/Support/lsregister -R -u /Developer/Applications/Xcode.app
  ```

- **Re-register Application:**
  ```bash
  # Re-register specific applications.
  lsregister -R -f /System/Library/CoreServices/TextInputSwitcher.app
  ```

#### **Setting Default Handlers**

- **Set Default Application for File Types:**
  ```bash
  # Set the default application for a specific file type (e.g., .txt files).
  defaults write com.apple.LaunchServices LSHandlers -array '{ LSHandlerContentType = "txt"; LSHandlerRoleAll = "com.apple.textedit"; }'
  ```

- **Set Default Application for Network Protocols:**
  You can also set the default application for network protocols (e.g., `smb://`, `rdp://`, `vnc://`, `ftp://`, `mailto:`, `http://`, and `https://` ).
  ```bash
  # Example of setting the default application for a specific protocol.
  defaults write com.apple.LaunchServices LSHandlers -array-add '{ LSHandlerURLScheme = "http"; LSHandlerRoleAll = "com.google.Chrome"; }'
  # Restart Finder to apply changes killall Finder
  killall Finder
  ```

#### **Inspecting Application File Type Associations**

`CFBundleTypeExtensions` specifies the types of files that the application can open or handle.

- **For Tor Browser:**
  ```bash
  # Check file type associations in Tor Browser's Info.plist.
  cd /Applications/Tor\ Browser.app/Contents/
  grep -A3 CFBundleTypeExtensions Info.plist | grep string
  ```

- **For Sublime Text:**
  ```bash
  # Check file type associations in Sublime Text's Info.plist.
  cd /Applications/Sublime\ Text.app/Contents/
  grep -A3 CFBundleTypeExtensions Info.plist | grep string
  ```

#### **Miscellaneous Commands**

- **Check Installed Applications:**
  ```bash
  # List installed applications and their details.
  lsappinfo list
  lsappinfo processList
  lsappinfo front
  lsappinfo info `lsappinfo front`
  lsappinfo info -only pid `lsappinfo front`
  lsappinfo info -only name `lsappinfo front`
  lsappinfo info -only bundlepath `lsappinfo front`
  lsappinfo -all list
  lsappinfo -all info "TextEdit"
  lsappinfo info "ASN:0x0-0x34034"
  lsappinfo info "org.mozilla.plugincontainer"
  ```

- **Manage Services:**
  ```bash
  # List and manage services.
  launchctl list
  ```
See [[launchctl]]

- **PBS (Preferences Background Service) Management:**
  ```bash
  # Dump and update services managed by pbs.
  /System/Library/CoreServices/pbs -dump_pboard > servicesdump.text
  /System/Library/CoreServices/pbs -flush
  /System/Library/CoreServices/pbs -update
  ```

This organized version includes detailed descriptions and options for working with the Launch Services database, setting default applications, and managing installed software and services.

---

#### Frameworks & Entitlements

Helper scripts to find which frameworks, launch daemons or applications use certain frameworks or entitlements 
https://gist.github.com/lechium/d37e4dee6cc9e909b2950d558b6f4771 - entitlementsfinder.sh 
e.g. 
```sh
% ./folder/dotfiles/entitlementFinder.sh /Volumes/HopeG16M568.J42dOS remote
% ./folder/dotfiles/linkFinder.sh /Volumes/HopeG16M568.J42dOS remote
```
