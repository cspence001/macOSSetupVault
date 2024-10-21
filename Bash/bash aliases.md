
# --- File and Directory Management ---
```bash
alias r="sudo -k; sudo rm -rfv"  # Removes files and directories recursively and forcefully with verbose output.
alias cp='cp -i'                 # Prompt before overwriting files.
alias mv='mv -i'                 # Prompt before overwriting files.
```
# --- Brew Management ---
```bash
alias uninstall="brew uninstall --zap"  # Uninstall apps installed via brew, delete associated files and folders.
alias setbrew='brew analytics off; brew untap homebrew/cask; brew untap homebrew/core; export HOMEBREW_NO_INSECURE_REDIRECT=1; export HOMEBREW_INSTALL_FROM_API=1; export HOMEBREW_NO_ANALYTICS=1; export HOMEBREW_NO_AUTO_UPDATE=1; export HOMEBREW_CASK_OPTS="--require-sha"'
```

# --- System Shutdown and Reboot ---
```bash
alias rb="/usr/local/bin/shutdown_with_cleanup.sh reboot"  # Runs cleanup script, closes all interactive BASH sessions before rebooting.
alias sd="/usr/local/bin/shutdown_with_cleanup.sh shutdown"  # Runs cleanup script, closes all interactive BASH sessions before shutting down.
#alias rb="sudo shutdown -r now" #Restarts the computer immediately.
#alias sd="sudo shutdown -h now" #Shuts down the computer immediately.
```

# --- Reloading and Environment ---
```bash
alias rs="source .bash_profile"  # Reloads the .bash_profile configuration.
alias ep="export PATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin/brew:$PATH"  # Updates the PATH environment variable.
```

# --- Power Management ---
```bash
alias caff="caffeinate -i -w"   # Prevents the system from sleeping while a process is running.
```
# --- Google Cloud SDK ---
```bash
alias gsutil="~/google-cloud-sdk/bin/gsutil"  # Google Cloud Storage CLI
alias gcloud="~/google-cloud-sdk/bin/gcloud"  # Google Cloud CLI
alias gcloudin="~/google-cloud-sdk/bin/gcloud auth login"  # Login to Google Cloud
alias gcloudout="~/google-cloud-sdk/bin/gcloud auth revoke --all"  # Revoke all Google Cloud authorizations.
alias gcloudauth="~/google-cloud-sdk/bin/gcloud auth list"  # List Google Cloud auths.

```
# --- Networking ---
```bash
alias spoof="sudo ifconfig en0 ether $(openssl rand -hex 6 | sed 's%\(..\)%\1:%g; s%.$%%')"  # Change MAC address to random value.
alias spcheck="ifconfig en0"  # Displays current configuration of the en0 network interface.
alias netinfo="scutil --nwi"  # Displays network interface information using scutil.
alias dnsinfo="scutil --dns"  # Shows DNS configuration details.
alias vpncheck="scutil --nc list"  # List active VPN connections.
alias apcheck="/System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport prefs"  # Airport preferences.
alias cycle="sudo ifconfig en0 down; sudo ifconfig en0 up;"  # Toggles the en0 network interface off and on.
alias flush="sudo route -n flush"  # Clears the IPv4 routing table.
alias flush6="sudo route -n flush -inet6" #Clears the IPv6 routing table.

alias spcheck2="scutil --get HostName; ifconfig en0 | grep ether | awk '{print $2}'; networksetup -listallhardwareports | awk -v RS= '/en0/{print $NF}'" #Retrieves and displays the hostname, current MAC address of the en0 interface, and the hardware port information for the network interface.
alias vpnconfig="system_profiler SPNetworkDataType | grep VPN" #VPN configurations
alias apset="sudo /System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport prefs RequireAdminIBSS=YES; /System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport prefs RequireAdminNetworkChange=YES; /System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport prefs RequireAdminPowerToggle=YES; /System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport prefs AllowLegacyNetworks=YES; /System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport prefs DisconnectOnLogout=YES"

```

# --- DNS Management ---
```bash
alias dnscheck='networksetup -getdnsservers "Wi-Fi"; scutil --dns | grep "nameserver\[[0-9]*\]"'  # Shows DNS servers and config.
alias setdns="ROUTER=\$(netstat -rn | grep 'en0' | grep -E 'UG|U' | awk '{print \$2}'); sudo networksetup -setdnsservers \"Wi-Fi\" \$ROUTER"  # Set DNS server for Wi-Fi.

alias fd="dscacheutil -flushcache;echo 'DNS cache flushed'" #Clears the DNS cache and displays a confirmation message.

alias dnsnameservers="python3 folder/macsetup/_scripts/reachable_dns.py"
alias dnsleaktest="python3 ~/folder/macsetup/_scripts/dnsleaktest.py" #tests VPN connection for DNS Leak. shows DNS leaks and your external IP. Provides both IPv4 and IPv6 addresses with country and ASN details.
alias dnsleaktest2="python3 ~/folder/macsetup/_scripts/dnsleaktest2.py" #uses a specific method to query DNS servers by creating a DNS request and sending it directly to the specified server or the system resolver. It may be limited to the IPv4 addresses that your system resolver is configured to use. Includes DNS hostname, ISP, and location.

alias ulmd="sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.mDNSResponderHelper.plist; sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist"
alias lmd="sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.mDNSResponderHelper.plist; sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist"
```

# --- etc/hosts ---
```bash
alias main_hosts="sudo ~/folder/macsetup/_scripts/apple_google_blockhosts.sh" #blocks apple, google domains
alias main_idhosts="sudo ~/folder/macsetup/_scripts/apple_google_blockhosts_allowid.sh" #blocks apple (except for id), google domains
alias apple_hosts="sudo ~/folder/macsetup/_scripts/apple_blockhosts.sh" #blocks apple 
alias apple_idhosts="sudo ~/folder/macsetup/_scripts/apple_blockhosts_allowid.sh" #blocks apple (except for id)
alias updatehosts="sudo python3 ~/folder/macsetup/_scripts/update_hosts_log_sub_restore.py" # specify list of host domains (file.txt), -d for directory of lists, -u for url link to domain list.
alias comphosts="sudo python3 ~/folder/macsetup/_scripts/compare_host_files.py" #compare domains from two host lists, input with command e.g. $ comphosts.py /path/to/file1.txt /path/to/file2.txt; or enter on prompt
alias hostlock="sudo chflags schg /etc/hosts" #Protecting with ‘schg’, the system immutable flag; Removal: sysctl kern.securelevel 1 means boot to single-user/recovery mode to run chflags noschg /etc/hosts, 0 means you can simply sudo chflags noschg /etc/hosts.
```
# --- Security & Permissions ---

```bash
alias securehome="sudo /bin/chmod -R og-rwx ~"  # Remove read, write, execute permissions for others on home directory.
alias securecheck="/bin/ls -l /Users/" #Lists the permissions and ownership of user directories.

alias l="sudo chmod -R 000"  # Remove all permissions for all users on files and directories recursively.
alias lrw="sudo chmod -R 700" #Sets the permissions to 700 (owner can read, write, execute; others have no permissions) recursively on files and directories.
alias lx="sudo chmod +x" #make file executable

alias slock="sudo chflags schg $1" #set the system immutable flag (super-user only)
alias ulock="sudo chflags uchg $1" #set the user immutable flag (owner or super-user only)
alias ulockr="sudo chflags -R uchg" #Sets the uchg (user immutable) flag on files and directories recursively.
alias removelock="sudo chflags nouchg,noschg $1" #clear the user/system immutable flag (owner or super-user only)

alias lsf='ls -lO'   # List files with long format and show file flags
alias lsa='ls -l@'   # List files with long format and show extended attributes
alias lsfa='ls -lO@' # List files with long format, show file flags and extended attributes
alias acl="sudo ls -lOae@" #list files (-l), flags (-O), xattr keys(-@), print acl's (-e) in long format, incl . dirs (-a)

alias xa="sudo xattr -r -l" #recursive names and vals xattr
alias xac="sudo xattr -r -c" #recursive clear xattr
alias xad="xattr -r -d" #recursive del (specific) xattr
alias xadq="xattr -d com.apple.quarantine" #delete quarantine xattr, removes warning when opening files downloaded from the internet (remove if you're confident file is safe)
alias xadqr="xattr -r -d com.apple.quarantine"
```

# --- Launch Daemons ---
```bash
alias lcu="sudo launchctl unload -w" #Unloads a Launch Daemon or Agent with superuser privileges.
alias lcl="sudo launchctl load -w" #Loads a Launch Daemon or Agent with superuser privileges.
alias alcu="launchctl unload -w" #Unloads a Launch Daemon or Agent without superuser privileges.
alias alcl="launchctl load -w" #Loads a Launch Daemon or Agent without superuser privileges.
alias lcp="sudo launchctl print"  # Print status and config of all loaded Launch Daemons and Agents.
alias slc="sudo launchctl print system"  # Print system-level Launch Daemons and Agents status.
alias ulc="sudo launchctl print user/501" # Print user-level Launch Daemons and Agents for user ID 501.
alias glc="sudo launchctl print gui/501" #Prints the status of GUI-based Launch Daemons and Agents for user ID 501 with superuser privileges.
alias sdis="sudo launchctl print-disabled system" #Displays disabled system-level Launch Daemons and Agents with superuser privileges.
alias udis="sudo launchctl print-disabled user/501" #Displays disabled user-level Launch Daemons and Agents for user ID 501 with superuser privileges.
alias ldis="sudo launchctl print-disabled gui/501" #Displays disabled GUI-based Launch Daemons and Agents for user ID 501 with superuser privileges.
alias enableuser="launchctl enable user/501/$1" #Enables a user service for user ID 501, taking the service name as an argument.
alias disableuser="launchctl disable user/501/$1" #Disables a user service for user ID 501, taking the service name as an argument.
sysdis() { sudo launchctl disable system/$1 ; } #Disables a specified system service using launchctl with elevated privileges.
sysen() { sudo launchctl enable system/$1 ; } #Enables a specified system service using launchctl with elevated privileges.
alias proc="sudo launchctl procinfo" #Displays detailed information about running processes managed by launchd with superuser privileges.
alias lhost="sudo launchctl hostinfo" #Retrieves and displays information about the launchd host environment with superuser privileges.
alias lrp="sudo launchctl resolveport" #Resolves and displays information about a specific port in the launchd environment with superuser privileges.
```

# --- Application Management ---
```bash
alias lsregister="/System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister"  # Manage app registration.
alias resetlcapps="/System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/LaunchServices.framework/Versions/A/Support/lsregister -kill -r -v -apps u,s,l,n"  # Resets app registrations, targeting user, system, and network apps.
alias resetlcfull="/System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/LaunchServices.framework/Versions/A/Support/lsregister -kill -seed -r -f -v -domain local -domain user -domain system -domain network" # Fully resets Launch Services registration, clearing all caches and registrations across local, user, system, and network domains.
```

# --- User Preferences ---
```bash
alias defr="defaults read" #Reads user preferences
alias defw="defaults write" #Writes user preferences
alias defd="defaults delete" #Deletes user preferences
alias deff="defaults find ${1}" #find plists containing ${word}

alias sdefr="sudo defaults read" #Reads system preferences with superuser privileges.
alias sdefw="sudo defaults write" #Writes system preferences with superuser privileges.
alias sdefd="sudo defaults delete" #Deletes system preferences with superuser privileges.
alias sdeff="sudo defaults find ${1}" #Searches for a specific key or value in the macOS user defaults system, requiring superuser privileges to access system-wide settings.
```

# --- Process Management ---
```bash
alias psa="ps aux"  # Displays a detailed list of all running processes on the system, including their user, process ID, CPU, and memory usage.
```

# --- Scripting and Utilities ---
```bash
alias listcomm="compgen -c"  # List commands, aliases, functions, etc.
alias listcommpaths="folder/macsetup/_scripts/list_commands.sh"  # List executable files in PATH directories.
alias emailleaktest="folder/macsetup/_scripts/emailleaktest.sh"  # Test SMTP server for IP leak.
```


# --- Git Aliases ---
```bash
alias gitconfig="git config --global --list"
clone_repo() {
    if [ -z "$1" ]; then
        echo "Usage: clone_repo <repository_name>"
        return 1
    fi
    git clone ssh://git@github.com:443/cspence001/$1.git
}
```

### **Cache Management**
```bash
# Open Directory Cache
alias odcache="sudo odutil reset cache; sudo odutil reset statistics;" 
alias flushcache="sudo AssetCacheManagerUtil flushCache;" 
alias personalcache="sudo AssetCacheManagerUtil flushPersonalCache;" 
alias sharedcache="sudo AssetCacheManagerUtil flushSharedCache;" 
alias cachecheck="AssetCacheManagerUtil settings;" 
alias cachelocator="/usr/bin/AssetCacheLocatorUtil" 
alias cachestat="AssetCacheManagerUtil status;" 

# Application Cache Clearing
alias orioncache="rm -rfv ~/Library/Application\ Support/Orion/Defaults/history/* ~/Library/WebKit/com.kagi.kagimacOS/WebsiteData/* ~/Library/HTTPStorages/com.kagi.kagimacOS/* ~/Library/HTTPStorages/com.kagi.kagimacOS.binarycookies ~/Library/Application\ Support/Orion/Defaults/Touch\ Icon\ Cache/* ~/Library/Application\ Support/Orion/Defaults/Thumbnail\ Cache/* ~/Library/Application\ Support/Orion/Defaults/SVG\ Cache/* ~/Library/Application\ Support/Orion/Defaults/Favicon\ Cache/* ~/Library/Caches/com.kagi.kagimacOS/*"
alias vscache="rm -rfv ~/Library/Application\ Support/Code/Cache/Cache_Data/* ~/Library/Application\ Support/Code/CachedData/* ~/Library/Application\ Support/Code/Service\ Worker/CacheStorage/* ~/Library/Application\ Support/Code/Service\ Worker/ScriptCache/*"
alias codiumcache="rm -rfv ~/Library/Application\ Support/VSCodium/Cache/Cache_Data/* ~/Library/Application\ Support/VSCodium/CachedData/* ~/Library/Application\ Support/VSCodium/CachedExtensionVSIXs/* ~/Library/Application\ Support/VSCodium/CachedProfilesData/* ~/Library/Application\ Support/VSCodium/Code\ Cache/* ~/Library/Application\ Support/VSCodium/DawnCache/* ~/Library/Application\ Support/VSCodium/GPUCache/* ~/Library/Application\ Support/VSCodium/Service\ Worker/CacheStorage/* ~/Library/Application\ Support/VSCodium/Service\ Worker/ScriptCache/* ~/Library/Application\ Support/VSCodium/Shared\ Dictionary/cache/* ~/Library/Application\ Support/VSCodium/WebStorage/*/CacheStorage/*"
alias codiumstorage="rm -rfv ~/Library/Application\ Support/VSCodium/Local\ Storage/leveldb/* ~/Library/Application\ Support/VSCodium/Session\ Storage/* ~/Library/Application\ Support/VSCodium/Service\ Worker/Database/* ~/Library/Application\ Support/VSCodium/Service\ Worker/Shared\ Dictionary/cache/* ~/Library/Application\ Support/VSCodium/User/History/*"
alias codiumlogs="rm -rfv ~/Library/Application\ Support/VSCodium/logs/*"
alias bravecookies="~/folder/macsetup/_dotfiles/bravecookies.sh" 
alias torcache="rm -rfv ~/Library/Application\ Support/TorBrowser-Data/Browser/Caches/*;rm -rfv ~/Library/Application\ Support/TorBrowser-Data/Browser/*.default/places.sqlite;rm -rfv ~/Library/Application\ Support/TorBrowser-Data/Browser/*.default/favicons.sqlite; rm -rfv ~/Library/Application\ Support/TorBrowser-Data/Browser/*.default/cookies.sqlite; rm -rfv ~/Library/Application\ Support/TorBrowser-Data/Browser/*.default/formhistory.sqlite; rm -rfv ~/Library/Application\ Support/TorBrowser-Data/Browser/*.default/storage*; rm -rfv ~/Library/Application\ Support/TorBrowser-Data/Tor/cached-*;"
alias stremiocache="rm -rfv ~/Library/Application\ Support/stremio-server/stremio-cache/*"
alias postgrescache="rm -rfv /usr/local/var/log/postgresql@15.log /usr/local/var/cache/* /usr/local/var/cache/*/Users/meuthu/.psql_history"
alias npmcache="npm cache clean --force; npm cache verify"
alias npmlogs="rm -rfv ~/.npm/_logs"
```

### **System Cache & Logs**
```bash
# General Cache Clearing
alias resetNet="rm -rfv /Library/Preferences/SystemConfiguration/com.apple.wifi.message-tracer.plist /Library/Preferences/SystemConfiguration/preferences*.plist /Library/Preferences/SystemConfiguration/NetworkInterfaces*.plist /Library/Preferences/SystemConfiguration/com.apple.airport.preferences.plist*"
alias cc="sudo rm -rfv ~/Library/Caches/* /Library/Caches/* /System/Library/Caches/* 2> /dev/null"
alias priv="sudo rm -rfv /private/var/folders/* /private/var/tmp/* /private/tmp/* 2> /dev/null"
alias sesh="rm -rfv ~/.zsh_sessions/* ~/.zsh_history ~/.zcompdump ~/.bash_sessions/* ~/.bash_history ~/.python_history ~/.psql_history ~/.lesshst ~/.sqlite_history"
alias logs="sudo rm -rfv ~/Library/Logs/* /Library/Logs/* /private/var/log/asl/* sudo rm -rfv /var/log/asl/* sudo rm -fv /var/log/asl.log rm -fv /var/log/asl.db rm -rfv /var/log/* sudo rm -fv /var/log/install.log rm -rfv /private/var/log/displaypolicy/iogdiagnose-last.bin /private/var/log/DiagnosticMessages/* /private/var/log/powermanagement/* 2> /dev/null"
alias ulib="rm -rfv ~/Library/Biome/* ~/Library/Calendars/* ~/Library/HomeKit/* ~/Library/News/* ~/Library/Reminders/* ~/Library/Trial/* ~/Library/Safari/* ~/Library/HomeKit/*"
alias savedstate="sudo rm -rfv ~/Library/Saved\ Application\ State/* 2> /dev/null"
alias httpstore="sudo rm -rfv ~/Library/HTTPStorages/* 2> /dev/null"
alias crashreports="sudo rm -rfv ~/Library/Application\ Support/CrashReporter/*"
alias con="sudo rm -rfv ~/Library/Containers/com.apple.TextEdit/Data/Library/Saved\ Application\ State/*;rm -rfv /Library/Containers/com.apple.Preview/Data/Library/Saved\ Application\ State/* 2> /dev/null"
alias diag="sudo rm -rfv /private/var/db/diagnostics/* /var/db/diagnostics/* /private/var/db/uuidtext/* /var/db/uuidtext/* /private/var/db/CoreDuet/* /private/var/db/systemstats/* /private/var/audit/* 2> /dev/null"
```

### **Cleaning Up**
```bash
# Finder & Application Data Cleanup
alias cleanupDS="sudo find . -type f -name '*.DS_Store' -ls -delete" 
alias cleanupLS="/System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -kill -r -v -all u,s,l && killall Finder" 
alias resetopenwith="/System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/LaunchServices.framework/Versions/A/Support/lsregister -kill -r -domain local -domain user;killall Finder;echo 'Open With has been rebuilt, Finder will relaunch'"
alias cleanupFinder="defaults delete com.apple.Finder FXRecentFolders; defaults delete com.apple.Finder RecentMoveAndCopyDestinations; killall Finder;" 
```
### **System Information & Tools**
```bash
# System Information Commands
alias installhistory="sudo cat /Library/Receipts/InstallHistory.plist"
alias sysinfoapp='open -a "System Information"'
alias console="open /System/Applications/Utilities/Console.app"
alias protonlog="open ~/Library/Containers/ch.protonvpn.mac/Data/Library/Logs/ProtonVPN.log -g -a Console"
```
### **Cache Management for Specific Applications**
```bash
# ProtonVPN Cache
alias protoncache="sudo rm -rfv ~/Library/Containers/ch.protonvpn.mac/Data/Library/HTTPStorages/* ~/Library/Containers/ch.protonvpn.mac/Data/Library/Caches/* ~/Library/Containers/ch.protonvpn.mac/Data/Library/Cookies/Cookies.binarycookies ~/Library/Containers/ch.protonvpn.mac/Data/Library/Application\ Support/database.sqlite ~/Library/Containers/ch.protonvpn.mac/Data/Library/Saved\ Application\ State/*"

# Quick Look Cache
alias ql="sudo qlmanage -r cache; qlmanage -r cache; 2> /dev/null"
alias ql2="sudo qlmanage -r; qlmanage -r; sudo purge;" # purge cleans up inactive mem
```

### **Miscellaneous**
```bash
# Miscellaneous Cleanup Commands
alias logsncache="sudo rm -rfv ~/Library/Containers/com.apple.mail/Data/Library/Logs/Mail/* /var/audit/* /private/var/audit/* /System/Library/LaunchDaemons/com.apple.periodic-*.plist /var/db/receipts/* /var/spool/cups/c0* /var/spool/cups/tmp/* /var/spool/cups/cache/job.cache* ~/.Trash/* &>/dev/null; sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder; sudo purge"
alias findDS='find . -name ".DS_Store"' # Find .DS_Store files in current directory (recursive)
```

---


find scripts in .bash_profile
`grep -nr '\.py\|\.sh' ~/.bash_profile > scripts_inbash.txt`