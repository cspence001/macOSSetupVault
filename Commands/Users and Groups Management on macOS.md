

[[#Listing Users and Groups]]
	[[#Users]]
	[[#Groups]]
[[#Group Membership Management]]
	[[#Viewing Membership]]
	[[#Modifying Membership]]
[[#Admin Group Management]]
	[[#Verify Membership]]
[[#User and Group Information]]
[[#User ID Management]]
[[#Security and Permissions]]
	[[#User Info Extraction]]
	[[#Root Account Management]]
	[[#Resetting Password]]
	[[#Cache Management]]
	[[#Printer Permissions Management]]

---

## Listing Users and Groups

### Users
- List all users:
  ```bash
  dscl . -list /Users > folder/u.txt
  ```

- List users with their Primary Group ID:
  ```bash
  dscl . -list /Users PrimaryGroupID
  ```

- List users with their Unique ID:
  ```bash
  dscl . list /Users UniqueID
  ```

- List users with Unique ID, sorted numerically:
  ```bash
  dscl . list /Users UniqueID | tr -s ' ' | sort -n -t ' ' -k2,2
  ```

### Groups
- List all groups:
  ```bash
  dscl . -list /Groups > folder/g.txt
  ```

- List groups with their Primary Group ID, sorted numerically:
  ```bash
  dscl . list /Groups PrimaryGroupID | tr -s ' ' | sort -n -t ' ' -k2,2
  ```

- Check group cache:
  ```bash
  dscacheutil -q group
  ```

- Query specific group:
  ```bash
  dscacheutil -q group -a name admin
  ```

## Group Membership Management

### Viewing Membership
- List group memberships:
  ```bash
  dscl . -list /Groups GroupMembership
  ```

- Read specific group membership:
  ```bash
  dscl . -read /Groups/mygroup GroupMembership
  ```

### Modifying Membership
- Remove user from a group:
  ```bash
  dscl . -delete /Groups/<group> GroupMembership "name"
  ```

- Add user to a new group:
  ```bash
  dscl . -create /Groups/brew GroupMembership "name"
  ```

- Append user to the admin group:
  ```bash
  dscl . -append /groups/admin GroupMembership USERNAME
  ```

## Admin Group Management

### Verify Membership
- Check current members of the admin group:
  ```bash
  dscl . -read /Groups/admin GroupMembership
  ```

- Verify changes:
  ```bash
  dscl . -read /groups/admin GroupMembership
  ```

## User and Group Information

- Read all users:
  ```bash
  dscl . readall /users
  ```

- Read all groups:
  ```bash
  dscl . readall /groups
  ```

- Read groups in plist format:
  ```bash
  dscl -plist . readall /groups
  ```

- List directories:
  ```bash
  dscl . ls /
  dscl "/Search" ls /
  dscl "/Contacts" ls /
  dscl "/Configure" ls /
  ```

## User ID Management

- Get user ID:
  ```bash
  id -u  # returns UID
  id -un # returns username
  id -g  # returns primary GID
  id -gn # returns primary group name
  ```

## Security and Permissions

### User Info Extraction

- Display user plist details:
  ```bash
  dscl . -list /Users | grep -vE '_|root|nobody|daemon|Guest' | xargs -n 1 sh -c 'sudo plutil -p /private/var/db/dslocal/nodes/Default/users/"$1.plist"' _
  ```
  
- Read all user info:
  ```bash
  for l in /var/db/dslocal/nodes/Default/users/*; do 
      if [ -r "$l" ]; then 
          echo "$l"; 
          defaults read "$l"; 
      fi; 
  done
  ```

* Non-Service accounts only
```sh
sudo bash -c 'for i in $(find /var/db/dslocal/nodes/Default/users -type f -regex "[^_]*"); do plutil -extract name.0 raw $i | awk "{printf \$0\":\$ml\$\"}"; for j in {iterations,salt,entropy}; do l=$(k=$(plutil -extract ShadowHashData.0 raw $i) && base64 -d <<< $k | plutil -extract SALTED-SHA512-PBKDF2.$j raw -); if [[ $j == iterations ]]; then echo -n $l; else base64 -d <<< $l | xxd -p -c 0 | awk "{printf \"$\"\$0}"; fi; done; echo ""; done'
```

### Root Account Management
- Ensure the root account is disabled:
  ```bash
  rootEnabled="$(dscl . -read /Users/root AuthenticationAuthority 2>&1 | grep -c "No such key")"
  rootEnabledRemediate="$(dscl . -read /Users/root UserShell 2>&1 | grep -c "/usr/bin/false")"

  if [ "$rootEnabled" = "1" ]; then
      # passed
  fi
  if [ "$rootEnabledRemediate" = "1" ]; then
      # passed
  fi

  # Fix:
  sudo dscl . -delete /Users/root UserShell  
  sudo dscl . -append /Users/root UserShell /usr/bin/false
  ```

### Resetting Password
- Reset user password:
  ```bash
  dscl . -passwd /Users/"$username" "$newpassword"
  ```
  
### Cache Management

**`dsmemberutil`** utility specifically focused on managing group memberships. Allows you to query and modify group memberships for users (access, roles).
- Flush member cache:
  ```bash
  dsmemberutil flushcache
  ```

**`dscacheutil`** manages the directory service cache. It can flush the cache to ensure that the system retrieves the latest user and group information from the directory service.
- Flush user cache:
  ```bash
  dscacheutil -flushcache
  ```

**`odutil`** used for managing Open Directory services. Reset caches and statistics, ensuring up-to-date user and group information. Aids in troubleshooting and performance monitoring ODS.
- Reset Open Directory cache:
  ```bash
  odutil reset cache
  odutil reset statistics
  ```

see also man pages for `opendirectoryd(8), DirectoryServiceAttributes(7), dsenableroot(8), dserr(8)`
## Printer Permissions Management

To enable anyone to access printer preferences:
```bash
/usr/bin/security authorizationdb read system.preferences.printing > /tmp/system.preferences.printing.plist
/usr/bin/defaults write /tmp/system.preferences.printing.plist group everyone
/usr/bin/security authorizationdb write system.preferences.printing < /tmp/system.preferences.printing.plist
```

- Allow everyone to manage printer issues:
```bash
dseditgroup -o edit -n /Local/Default -a "everyone" -t group lpadmin
```
