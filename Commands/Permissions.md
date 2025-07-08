
---

### File and Directory Permissions

#### List Permissions
Check current permissions:
`ls -la` e.g. `ls -la ~/.npm`
List files with long format and show file flags:
`ls -lO`  
List files with long format and show extended attributes:
`ls -l@`  
List files with long format, show file flags and extended attributes:
`ls -lO@` 
List files (`-l`), flags (`-O`), xattr keys(`-@`), print acl's (`-e`) in long format, incl . dirs (`-a`):
`ls -lOae@` 

---
#### `chmod` (Change Mode)

**Purpose:**
- Modify file or directory permissions.

**Common Permissions:**
- **`chmod 777`** `drwxrwxrwx`
  - **Owner:** Read, Write, Execute (rwx)
  - **Group:** Read, Write, Execute (rwx)
  - **Others:** Read, Write, Execute (rwx)
  - **Description:** Full access for all users (generally insecure).

- **`chmod 755`** `drwxr-xr-x`
  - **Owner:** Read, Write, Execute (rwx)
  - **Group:** Read, Execute (r-x)
  - **Others:** Read, Execute (r-x)
  - **Description:** Full access for owner, read and execute for others (common for directories/scripts).

* **`chmod 750`** `drwxr-x---`
  - **Owner:** Read, Write, Execute (rwx)
  - **Group:** Read, Execute (r-x)
  - **Others:** No permissions (---)
  - **Description:** Full access for the owner, read and execute for the group, and no access for others (suitable for private directories).

- **`chmod 700`** `drwx------`
  - **Owner:** Read, Write, Execute (rwx)
  - **Group:** No permissions (---)
  - **Others:** No permissions (---)
  - **Description:** Full access for owner only (secure, private).

- **`chmod 660`** `rw-rw----`
  - **Owner:** Read, Write (rw-)
  - **Group:** Read, Write (rw-)
  - **Others:** No permissions (---)
  - **Description:** Read and write access for owner and group only.

- **`chmod 600`** `-rw-------`
  - **Owner:** Read, Write (rw-)
  - **Group:** No permissions (---)
  - **Others:** No permissions (---)
  - **Description:** Private file (only the owner has access).

- **`chmod 000`** `----------`
  - **Owner:** No permissions (---)
  - **Group:** No permissions (---)
  - **Others:** No permissions (---)
  - **Description:** No access for anyone (completely locked out).

#### Adding + Removing Permissions with `chmod`

**Remove read access for Others:**
`chmod o-r <directory_name>`
**Remove read access for All (Owner, Group, Others):**
`chmod a-r <directory_name>`

##### Ensuring correct User / Local (`~/.ssh`) permissions with `chmod`
- [**~/.ssh Directory Permissions**](https://gist.github.com/denisgolius/d846af3ad5ce661dbca0335ec35e3d39)
- Check Permissions: `ls -la ~/.ssh`
- **Directory (`~/.ssh`)**:
    - `drwx------` 
    - `chmod 700 ~/.ssh`
- **Private Key (`id_ed25519`)**:
    - `-rw-------` 
    - `chmod 600 ~/.ssh/*`
- **Public Key (`id_ed25519.pub`)**:
    - `-rw-r--r--` 
    - `chmod 644 ~/.ssh/*.pub`
- **Config File (`config`)**:
    - `-rw-r--r--`
    - `chmod 644 ~/.ssh/config`
- **Known Hosts (`known_hosts`)**:
    - `-rw-r--r--`
    - `chmod 644 ~/.ssh/known_hosts`
- **Old Known Hosts (`known_hosts.old`)**:
    - `-rw-r--r--` 
    - `chmod 644 ~/.ssh/known_hosts`
- **Special Files (`authorized_keys`)**
	- `-rw-r--r--`
	- `chmod 644 ~/.ssh/authorized_keys`

---

#### `chflags` (Change Flags)

**Purpose:**
- Set or remove special file system attributes.

**Common Flags:**
- **`schg`**: System immutable flag (prevents modifications).
- **`uchg`**: User immutable flag (prevents modifications by owner).
- **`restricted`**: SIP restricted flag (macOS-specific).

**Usage Examples:**
- **Set immutable flag:**
  ```sh
  sudo chflags schg filename
  ```
- **Remove immutable flag:**
  ```sh
  sudo chflags noschg filename
  ```

---

#### `chown` (Change Owner)

**Purpose:**
- Change the owner and/or group of a file or directory.

**Usage Examples:**
- **Change owner and group:**
  ```sh
  sudo chown username:groupname filename
  ```
- **Change only owner:**
  ```sh
  sudo chown username filename
  ```
- **Change only group:**
  ```sh
  sudo chown :groupname filename
  ```

**Troubleshooting `chown: username: illegal group name` Error:**
1. **Set Ownership to Current User Only:**
   ```sh
   sudo chown -R $(whoami) ~/.npm
   ```
2. **Ensure Group Exists:**
   ```sh
   groups $(whoami)
   sudo chown -R $(whoami):$(whoami) ~/.npm
   ```
3. **Determine and Use Correct Group:**
   ```sh
   id -gn
   sudo chown -R $(whoami):$(id -gn) ~/.npm
   ```

#### `chgrp` (Change Group)

Change group ownership:
```bash
sudo chgrp groupname /path/to/file
```

---

#### Directory-Specific Permissions
##### System-level Directory permissions

 **`/etc/ssh/`**

**Permissions Setup:**
```sh
sudo chmod 755 /etc/ssh
sudo chmod 644 /etc/ssh/moduli
sudo chmod 644 /etc/ssh/ssh_config
sudo chmod 755 /etc/ssh/ssh_config.d
sudo chmod 600 /etc/ssh/ssh_host*
sudo chmod 644 /etc/ssh/ssh_host*.pub
sudo chmod 644 /etc/ssh/sshd_config
sudo chmod 755 /etc/ssh/sshd_config.d
```
- **Resource:** [Apple Stack Exchange](https://apple.stackexchange.com/questions/431367/ssh-permissions-in-files-etc)

**`/etc/hosts`**

**Locking and Protection:**
- **Using System Immutable Flag (`schg`):**
  ```sh
  sudo chflags schg /etc/hosts
  ```
  - **Removal:**
    ```sh
    sysctl kern.securelevel
    sudo chflags noschg /etc/hosts
    ```

- **Using System Integrity Protection (SIP) (El Capitan and Later):**
  ```sh
  chflags restricted /etc/hosts
  csrutil disable
  ```
  - **Resource:** [Apple Stack Exchange](https://apple.stackexchange.com/a/282341)

---
##### User-level Directory Permissions

**`~/.npm`**

**Check Current Permissions:**
```sh
ls -la ~/.npm
```
**Apply Permissions:**
```sh
chmod 700 ~/.npm
```
**Recursively Set Permissions:**
```sh
chmod -R 700 ~/.npm
```
**Set Ownership:**
```sh
sudo chown -R $(whoami):$(whoami) ~/.npm
```
**Verify Changes:**
```sh
ls -la ~/.npm
```


**`~/.vscode-oss`**

**Check Current Permissions:**
```sh
ls -la ~/.vscode-oss
```
**Set Correct Permissions:**
```sh
chmod 755 ~/.vscode-oss
```
**Recursively Set Permissions:**
```sh
chmod -R 755 ~/.vscode-oss
```
**Set Ownership:**
```sh
sudo chown -R $(whoami):$(whoami) ~/.vscode-oss
```
**Verify Changes:**
```sh
ls -la ~/.vscode-oss
```


---

