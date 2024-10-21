
[[#Using Different Git Emails for Personal and Work Repositories on the Same Machine]]
	[[#Method 1 Conditional Includes in Global Config]]
	[[#Method 2 Set Email Per Repository]]
[[#Why use separate SSH keys]]
[[#Configuring Git and SSH for Personal and Public Repositories]]
	[[#SSH Configuration (`~/.ssh/config`)]]
	[[#Git Configuration (`~/.gitconfig`)]]
[[#GPG and SSH commit signing with multiple identities or accounts]]
	[[#GPG Commit Signing with Multiple Identities]]
	[[#SSH Commit Signing with Multiple Accounts]]
[[#VSCodium Git Protocol Configuration]]

---

### Using Different Git Emails for Personal and Work Repositories on the Same Machine
To manage multiple Git accounts (personal and work) on the same machine without changing the global Git email, you can use conditional configuration in your global Git config file.

##### Method 1: Conditional Includes in Global Config
**Separation of Identities**: Easily manage different identities for personal and work projects.
**Automatic Configuration**: Git automatically selects the appropriate email based on the repository path.

1. **Rename Existing Global Config**
   - Rename your existing global `.gitconfig` file to `.gitconfig-personal` to keep it as your personal configuration.
   ```bash
   mv ~/.gitconfig ~/.gitconfig-personal
   ```

2. **Create Work Config File**
   - Create a new configuration file for your work account.
   ```bash
   touch ~/.gitconfig-work
   ```
   - Add the following content to `~/.gitconfig-work`:
```
[user]
   email = work@email.com
   name = Your Name
```

**You can also set variables using the `--file` option with `git config`:**
```sh
# For Work Repository
git config --file ~/.gitconfig-work user.name "Name Work" 
git config --file ~/.gitconfig-work user.email "name.work@example.com" git config --file ~/.gitconfig-work user.signingKey ~/.ssh/id_rsa_work.pub
```

3. **Create a New Global Config File**
   - Create a new global `.gitconfig` file that will include your personal and work configurations based on the repository path.
   ```bash
   touch ~/.gitconfig
   ```
   - Add the following content to `~/.gitconfig`:
```
# Conditional Includes
[includeIf "gitdir:~/work/"]
   path = ~/.gitconfig-work
[includeIf "gitdir:~/personal/"]
   path = ~/.gitconfig-personal
```
- The `[includeIf]` directive allows Git to include specific config files based on the directory of the repository. 
- When you work within `~/work/`, Git will use your work email, while repositories in `~/personal/` will use your personal email.

##### Method 2: Set Email Per Repository
You can also set the email address for a single repository if needed:
```bash
git config --local user.email work@email.com
```
This command only affects the current repository.

If you've already committed changes with the wrong email, you can update the email in those commits:
```bash
git commit --amend --author="Your Name <correct-email@example.com>" --no-edit
```


---

### Why use separate SSH keys 
**Using separate SSH keys enhances security and access management**

**Use-Cases for Separate SSH Keys:**
1. **Security Isolation:** Reduces risk of compromise by isolating different keys.
2. **Access Control:** Allows granular control over repository access.
3. **Auditability:** Facilitates easier tracking and auditing of key usage.
4. **Compliance:** Meets security and accountability standards.
5. **Convenience:** Simplifies key management and revocation.
6. **Performance:** May improve SSH key selection efficiency.

**Example Use Cases**
- **Personal vs. Work Projects:** Separate keys for personal and work repositories.
- **Different GitHub Organizations:** Use different keys for each organization.
- **Service Accounts:** Manage automation or CI/CD access with separate keys.

**Considerations**
- **Key Management:** Use secure methods to manage multiple keys.
- **Backup:** Maintain secure backups of your SSH keys.

---
### Configuring Git and SSH for Personal and Public Repositories

By following these configurations, you can use SSH over port 443 for both personal and public repositories and manage different SSH keys for different purposes:

#### SSH Configuration (`~/.ssh/config`)

**Set Up SSH Configuration**

```sh
# ~/.ssh/config
#Default Configuration for GitHub
Host github.com
  Hostname ssh.github.com
  Port 443
  User git
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519  # Default key
  IdentitiesOnly yes
  UserKnownHostsFile ~/.ssh/known_hosts
#Configuration for Personal Repositories
Host github-cspence001
  Hostname ssh.github.com
  Port 443
  User git
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519_cspence001  # Personal key
  IdentitiesOnly yes
  UserKnownHostsFile ~/.ssh/known_hosts
#Configuration for Work Repositories
Host github-work
  Hostname ssh.github.com
  Port 443
  User git
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519_cspence001  # Personal key
  IdentitiesOnly yes
  UserKnownHostsFile ~/.ssh/known_hosts
```

#### Git Configuration (`~/.gitconfig`)

**Git URL Rewrites**: Rewrites urls when using `git clone`, `git commit`, etc.

1. **For Public Repositories**
To default to SSH with port 443 for public repositories:
```sh
git config --global url."ssh://git@github.com:443/".insteadOf "https://github.com/"
git config --global url."ssh://git@github.com:443/".insteadOf "git@github.com"
```

**In `~/.gitconfig` this will appear as:** 
```sh
[url "ssh://git@github.com:443/"]
  insteadOf = git@github.com:
[url "ssh://git@github.com:443/"]
  insteadOf = https://github.com/
```
* *(Example for Public Repository Rewrites)*

2. **For Personal Repositories**
To use SSH with port 443 for personal repositories (default ssh key):
```sh
git config --global url."ssh://git@github.com:443/cspence001".insteadOf "https://github.com/cspence001"
git config --global url."ssh://git@github.com:443/cspence001".insteadOf "git@github.com/cspence001"
```
To use SSH with port 443 for personal repositories (with personal key):
```sh
git config --global url."ssh://git@github-cspence001".insteadOf "https://github.com/cspence001" 
git config --global url."ssh://git@github-cspence001".insteadOf "git@github.com/cspence001"
```

3. **For Conditional Includes** 
To use SSH with port 443 for specified configurations (with specified key):
```sh
# For Personal Configuration: `~/.gitconfig-personal` 
git config --file ~/.gitconfig-personal url."ssh://git@github-cspence001".insteadOf "git@github.com/cspence001"
# For Work Configuration:`~/.gitconfig-work`
git config --file ~/.gitconfig-work url."ssh://git@github-work".insteadOf "git@github.com/workuser"
```


**Using the Configuration**

1. Cloning Personal Repositories
```sh
git clone https://github.com/cspence001/repo.git
# or
git clone git@github.com:cspence001/repo.git
```

2. Cloning Public Repositories
```sh
git clone https://github.com/someuser/somepublicrepo.git
# or
git clone git@github.com:someuser/somepublicrepo.git
```

3. Specify SSH Key
To use the personal key:
```sh
git clone git@github-cspence001:cspence001/repo.git
```
To use the work key:
```sh
git clone git@github-work:workuser/repo.git
```

---

### GPG and SSH commit signing with multiple identities or accounts

##### GPG Commit Signing with Multiple Identities / Accounts

**Setup**
1. **Generate Multiple GPG Keys**:
   - You generate a separate GPG key for each identity (e.g., personal and work).

   ```bash
   gpg --full-generate-key
   ```

   - You'll create keys with different email addresses:
     - Personal: `yourname.personal@example.com`
     - Work: `yourname.work@example.com`

2. **Configure Git for Each Identity**:
   * Specify GPG key per config file: (Method 1: Conditional Includes in Global Config)
```sh
   # If using conditional includes:
   git config --file ~/.gitconfig-work user.signingKey <work-gpg-key-id>
```
   - Specify GPG key based on the repository. (Method 2: Set Email Per Repository)
   ```bash
   # For Personal Repository
   git config user.name "Your Name Personal"
   git config user.email "yourname.personal@example.com"
   git config user.signingKey <personal-gpg-key-id>

   # For Work Repository
   git config user.name "Your Name Work"
   git config user.email "yourname.work@example.com"
   git config user.signingKey <work-gpg-key-id>
   ```

**Commit Signing**
- When you create a commit, Git will use the configured GPG key based on the repository’s settings:
```bash
git commit -S -m "Your commit message"
```
- The signature will correspond to the identity set in that repository, ensuring the commit is properly attributed.

##### SSH Commit Signing with Multiple Accounts

**Setup**
1. **SSH Key Configuration**:
   - You generate separate SSH keys for each account and configure them in the `~/.ssh/config` file.
   ```bash
   ssh-keygen -t rsa -C "yourname.personal@example.com" -f ~/.ssh/id_rsa_personal
   ssh-keygen -t rsa -C "yourname.work@example.com" -f ~/.ssh/id_rsa_work
   ```

2. **SSH Config**:
   - You map the keys to specific hosts in the SSH config file.
   ```plaintext
   Host github-personal
       Hostname github.com
       User git
       IdentityFile ~/.ssh/id_rsa_personal

   Host github-work
       Hostname github.com
       User git
       IdentityFile ~/.ssh/id_rsa_work
   ```

3. **Git Configuration**:
   * Specify SSH key per config file: (Method 1: Conditional Includes in Global Config)
```sh
   git config --file ~/.gitconfig-work user.signingKey ~/.ssh/id_rsa_work.pub
```
   - Specify SSH key per repo (Method 2: Set Email Per Repository) and `allowed_signers` file, if needed.
```bash
# For Personal Repository
git config user.name "Your Name Personal"
git config user.email "yourname.personal@example.com"
git config user.signingKey ~/.ssh/id_rsa_personal.pub

# For Work Repository
git config user.name "Your Name Work"
git config user.email "yourname.work@example.com"
git config user.signingKey ~/.ssh/id_rsa_work.pub
```

**Commit Signing**
- When creating a commit, you would specify the host based on which identity you want to use:
```bash
# For personal commits
git clone git@github-personal:cspence001/repo.git
git commit -S -m "Your commit message"

# For work commits
git clone git@github-work:yourname/workrepo.git
git commit -S -m "Your commit message"
```


**Differences**
1. **Key Management**:
   - **GPG**: Each identity has its own GPG key, allowing for easy switching between identities based on repository settings.
   - **SSH**: Uses different SSH keys for different accounts, managed via an SSH config file, which is easier to set up for GitHub but involves setting up an allowed signers file for signature verification.
2. **Configuration Complexity**:
   - **GPG**: Requires managing multiple keys and configuring Git to use the correct key for each repository.
   - **SSH**: Involves fewer steps if you're already using SSH keys for authentication, but requires additional steps for commit signing.
3. **Use Case**:
   - **GPG**: Better for scenarios requiring strong cryptographic guarantees and multiple identities.
   - **SSH**: More straightforward for users who already have SSH infrastructure and want a simple way to sign commits without extra key management. 



---

### VSCodium Git Protocol Configuration

- **Git: Enable Commit Signing:** Enable it in VSCodium settings.
- **GitHub: Git Protocol:** Set to SSH in VSCodium settings.
- **SSH Configuration:** Ensure correct setup in `~/.ssh/config`, and verify key addition and SSH connection.

This setup ensures VSCodium uses SSH over port 443 for GitHub and handles commit signing if configured.

1. **Enable Commit Signing**

To enable commit signing with SSH:
1. Open VSCodium.
2. Go to `File` -> `Preferences` -> `Settings`.
3. Search for `Git: Enable Commit Signing` and check the box to enable it.

**Configure Signing Key**
For GPG key signing:
```sh
git config --global user.signingkey YOUR_GPG_KEY_ID
git config --global commit.gpgSign true
```
For SSH key signing, configure your setup accordingly (GPG is the standard for commit signing).

2. Set GitHub Protocol to SSH

	1. Open VSCodium.
	2. Go to `File` -> `Preferences` -> `Settings`.
	3. Search for `GitHub: Git Protocol`.
	4. Set it to `ssh`.

3. Confirm SSH Configuration
Ensure your SSH config is set up correctly:
```sh
# ~/.ssh/config
Host github.com
  Hostname ssh.github.com
  Port 443
  User git
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
  IdentitiesOnly yes
  UserKnownHostsFile ~/.ssh/known_hosts
```

**Additional Steps**
1. **Add SSH Key to Agent:**
   ```sh
   ssh-add --apple-use-keychain ~/.ssh/id_ed25519
   ```

2. **Test SSH Connection:**
   ```sh
   ssh -T git@github.com
   ```

3. **Verify Git Configurations:**
   ```sh
   git config --global user.signingkey YOUR_GPG_KEY_ID
   git config --global commit.gpgSign true
   git config --global url."ssh://git@ssh.github.com:443/".insteadOf "https://github.com/"
   ```




---

