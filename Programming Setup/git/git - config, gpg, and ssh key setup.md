
---

## Table of Contents

1. [[#Generating and Managing SSH Keys]]
2. [[#SSH Configuration]]
3. [[#Git Configuration]]
4. [[#Managing Remote URLs in Git (Local)]]
5. [[#Signing Commits]]
	[[#GPG Configuration]]
	[[#Signing Commits using SSH Keys]]
	[[#GPG/SSH Commit Signing with Multiple Accounts]]
6. [[#Managing Git Repositories]]
	1. [[#Removing Files from Git Repository]]
	2. [[#.gitignore]]
7. [[#Resolve Divergent Branches]]
8. [[#Git Environment Variables (Debug)]]
9. [[#Advanced Git Topics]]
10. [[#Advanced SSH Topics]]

---

### Generating and Managing SSH Keys

- **Generate a New SSH Key and Add it to the SSH Agent**
  - [GitHub Docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

**Generate new SSH key (use GitHub email address):**
```shell
ssh-keygen -t ed25519 -C "your_email@example.com"
```

**Add your SSH key to ssh-agent:**
```shell
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```
Check `~/.ssh/config` [[#SSH Configuration]]

**Add your SSH private key to the ssh-agent and store your passphrase in the keychain:**
  - Use `--apple-use-keychain` to store the passphrase in your keychain.
  ```bash
  ssh-add --apple-use-keychain ~/.ssh/id_ed25519
  ```
  
**Check if your key is added to the SSH agent**: 
  ```bash
  ssh-add -l
  ```
This lists the identities currently held by the SSH agent. 
  
**Copy the SSH public key to your clipboard:**
```shell
$ pbcopy < ~/.ssh/id_ed25519.pub
```
1. GitHub > **Settings**.
2. In the "Access" section > **SSH and GPG keys** > New SSH Key 
3. Paste Public Key

Test your SSH connection to GitHub directly:
```bash
  ssh -T git@github.com
```

- **Removing Keys from SSH Agent**
  - Start SSH Agent: `$ eval "$(ssh-agent -s)"
  - Delete All Identities: `%ssh-add -D`
  - Delete Specific Identity: `%ssh-add -D /Users/meuthu/.ssh/id_ed25519.pub`
  - List Identities: `%ssh-add -l`
  - Changing a passphrase for existing private key: `ssh-keygen -p -f ~/.ssh/id_ed25519`

  *Note:* SSH key passphrase is stored in the local items keychain.

* [Generating new SSH key for Hardware Security key - github](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key-for-a-hardware-security-key)

---

### SSH Configuration
SSH into `git@ssh.github.com` over port 443,
override your SSH settings to force any connection to GitHub.com to run through that server and port.

- **SSH Configuration File (`~/.ssh/config`)**
- **Note:** This is your local (user) ssh config. Not the same as your `/etc/ssh` config for system client/server configuration. Create file if it doesn't exist to use ssh with git. See [[#Ensure Correct User / Local (`~/.ssh`) permissions]]
  ```plaintext
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

- **`Hostname ssh.github.com`**: GitHub supports SSH over port 443 using `ssh.github.com`.
- **`Hostname github.com`**: This is typically used for the default SSH connection (port 22).

  - `IdentitiesOnly yes` ensures only the specified key is used.
  - `UserKnownHostsFile`: [Stack Overflow](https://stackoverflow.com/a/15578473)
  - `StrictHostKeyChecking` option: [Unix Stack Exchange](https://unix.stackexchange.com/a/26282)

- **Test GitHub SSH Connection**
  - `%ssh -T git@github.com`
  - Alternate Port 443: `%ssh -T -p 443 git@ssh.github.com`

##### Cloning via SSH on Port 443 Without URL Rewrites / Default SSH (Port 22)

- **Without SSH Configuration**:
  - If you do not specify `hostname ssh.github.com` and `Port 443` in your `~/.ssh/config`, the default SSH settings will apply. For example:
    ```plaintext
    Host github.com
        Hostname github.com
        User git
        IdentityFile ~/.ssh/id_ed25519
    ```
  
- **Using SSH Over HTTPS Port**:
  - To clone a specific repository using SSH over port 443 (if not specified in `~/.ssh/config`), use the following command:
    ```sh
    git clone ssh://git@github.com:443/cspence001/mapapp_cloudfrontS3_tests.git
    ```
  - Refer to the GitHub documentation for more details on [Using SSH Over HTTPS Port](https://docs.github.com/en/authentication/troubleshooting-ssh/using-ssh-over-the-https-port).

##### Git Configuration Tweaks (`.gitconfig`) for SSH
- **Configuring URL InsteadOf** 
  - [Git URL Configuration](https://gist.github.com/Kovrinic/ea5e7123ab5c97d451804ea222ecd78a)
  - Commands:
- This rewrite would redirect HTTPS URLs to use SSH when using git operations.
    ```bash
git config --global url."git@github.com/".insteadOf "https://github.com/"
    ```

##### Ensure Correct User / Local (`~/.ssh`) permissions 
- **SSH Directory Permissions**
  - [Permissions in `~/.ssh`](https://gist.github.com/denisgolius/d846af3ad5ce661dbca0335ec35e3d39)
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
### Git Configuration

**List Git Configurations**
- List All Configurations: `% git config -l`
- List Configurations with File Origin: `% git config --list --show-origin`
- List Git Configs with osxkeychain: `%git config --get-all --show-origin credential.helper`

**Git Configuration Scopes**
- Global (User-Specific): `% git config --global --list --show-origin`
- Core (Read-Only): `/Library/Developer/CommandLineTools/usr/share/git-core/gitconfig`
- Local (Repository-Specific): `% git config --local --list --show-origin`
	- (Ensure you are within local repository directory)

##### Git Global Configuration 
**Setting Global Git Username + Email**
```sh
git config --global user.name "Your Name"
git config --global user.email "youremail@yourdomain.com"
```
* [Setting Git Username + Email for Single Repository](https://linuxize.com/post/how-to-configure-git-username-and-email/#setting-git-username-and-email-for-a-single-repository)

- **Sample `.gitconfig` (global)**
  ```plaintext
  [user]
	name = USERNAME
	email = ID+USERNAME@users.noreply.github.com
  [core]
    excludesfile = /Users/$USER/.gitignore
  ```

**Using environment variables**
1. `GIT_COMMITTER_EMAIL=your_email@abc.example`
2. `GIT_AUTHOR_EMAIL=your_email@abc.example`

**Overwrite the user's name and email in the last commit:**
If you mistakenly committed a change without setting the correct author name or email:
```sh
git -c user.name="your name" -c user.email=youremail@email.com
commit --amend --reset-author
```

See Also [[git - multiple identity management, vscode config]]

- **Additional Resources**
  - [More Git Config Settings](https://github.com/mathiasbynens/dotfiles/blob/master/.gitconfig)
  - [Git Config Commands, Environment Variables, and Scopes](https://git-scm.com/docs/git-config.html)
  - [Git Credential Storage](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage)
  - [Git Environment Variables](https://git-scm.com/book/en/v2/Git-Internals-Environment-Variables)
  - [Example Git Install and Config](https://gist.github.com/ChrisTollefson/ab9c0a5d1dd4dd615217345c6936a307)
  - [Git Credential Manager (GCM)](https://github.com/git-ecosystem/git-credential-manager/blob/main/docs/environment.md)
  - [Git Credential Manager Configuration](https://github.com/git-ecosystem/git-credential-manager/blob/main/docs/configuration.md)

---
### Managing Remote URLs in Git (Local)
Remote URLs are the addresses of remote repositories that your local Git repository interacts with. They specify the location of the remote repository and allow you to push your changes to, and pull updates from, that repository. When you clone a repository, Git automatically adds a remote URL named `origin` that points to the cloned repository. However, if you initialize a repository with `git init`, you'll need to add the remote URL manually.

**Remote URL Verification:**
List the remotes associated with your repository, showing both fetch and push URLs.
```bash
git remote -v
```

**Update Remote URL:**
This is particularly useful in scenarios such as:
- Switching from HTTPS to SSH for authentication.
- Changing the remote URL due to a change in the repository location or ownership.

**Steps to Update the Remote URL:**
1. Navigate to the Local Repository Directory: `cd /path/to/your/repository/`
2. Ensure your Git configuration is pointing to the correct URL: `git remote -v`
3. Update URL of the Remote Repository: `git remote set-url origin [new_URL]`
   For example:
   ```bash
git remote set-url origin git@github.com:cspence001/BraveFilterSearch.git
   ```
   After running this command, any future Git operations that interact with the `origin` remote (like `git push`, `git pull`, or `git fetch`) will use the specified SSH or HTTPS URL.

**Adding a Remote URL After `git init`:**
If you initialized a new repository using `git init` and want to add a remote URL:
1. Navigate to your local directory: `cd /path/to/your/vault`
2. Initialize a new Git repository: `git init`
3. After initializing, add the remote repository URL: `git remote add origin [URL]`
   For example:
   ```bash
   git remote add origin https://github.com/username/repo.git
   ```
   or using SSH:
   ```bash
   git remote add origin git@github.com:username/repo.git
   ```
**Note:** If you have added a `README.md`, `.gitignore`, `LICENSE`, etc. you will need to fetch these from the remote and merge them with your local changes. 
Pull with Merge: `git pull --rebase origin main`

---
### Signing Commits
[Signing Commits - Github Documentation](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits)
You can sign commits locally using GPG, SSH, or S/MIME.
![[Screenshot 2024-10-10 at 22.29.47.png]]
Traditionally, Git has relied on GPG (GNU Privacy Guard) for commit signing. 
#### GPG Configuration

- **GPG Setup for GitHub**
  - [Generating a New GPG Key](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key)
  - [Guide to Generating and Using GPG Keys on GitHub](https://github.com/devuri/A-Guide-to-Generating-and-Using-GPG-Keys-on-GitHub)
  - [Privacy Guides for GPG on macOS](https://github.com/sunknudsen/privacy-guides/blob/master/how-to-encrypt-sign-and-decrypt-messages-using-gnupg-on-macos/README.md)

- **GPG Installation on macOS**
  - [GPG for macOS](https://sourceforge.net/projects/gpgosx/)
- Commands to create signing key for git commits:
    - `% gpg --version`
    - `% gpg --full-generate-key`
    - `% gpg --list-secret-keys --keyid-format=long`
* Associate an email with your GPG key for signing commits + tags:
    - `% gpg --edit-key YOUR-GPG-KEY-ID`
    - Enter `gpg> adduid` to add your name, email, and any comments. (Use your GitHub-provided no-reply email if desired.)
    - Verify: `gpg> uid`will display the associated user IDs for that key, including any emails.
* Add GPG key to Github:
	- `% gpg --armor --export <gpg-key-id>
	* GitHub account settings > "SSH and GPG keys" > "New GPG key"
	* Paste in the exported ASCII-armored key.
* To sign all commits by default in any local repository on your computer:
    - `% git config --global user.signingkey <gpg-key-id>
    - `% git config --global commit.gpgsign true`

```sh
[user]
	name = USERNAME
	email = ID+USERNAME@users.noreply.github.com
	signingkey = <gpg-key-id>
[core]
	excludesfile = /Users/$USER/.gitignore
[commit]
	gpgsign = true
```

Download recommended defaults:
```sh
curl -o ~/.gnupg/gpg.conf https://raw.githubusercontent.com/drduh/config/master/gpg.conf
```

##### GPG - Creating keys 
https://github.com/remy-tiitre/secure-osx/tree/main?tab=readme-ov-file#add-master-key
* Master password
* Master key
* Signing subkey
* Encryption subkey
* Authentication subkey

* **More Information**
  * [GPG capabilities and other commands](https://github.com/lfit/itpol/blob/master/protecting-code-integrity.md#installing-openpgp-software)
  * [drduh/macOS-Security-and-Privacy-Guide - PGP/GPG](https://github.com/drduh/macOS-Security-and-Privacy-Guide?tab=readme-ov-file#pgpgpg)
	  * Links for GPG use-cases, Online Guides 
	  * [pwd.sh](https://github.com/drduh/pwd.sh) GnuPG symmetric password manager
		  * manage passwords and other text-based secrets.
		  * symmetrically (i.e., using a passphrase) encrypt + decrypt plaintext files.
	  * [Purse](https://github.com/drduh/Purse) GnuPG asymmetric password manager
  * [drduh/config - gpg-agent.conf](https://github.com/drduh/config/blob/master/gpg-agent.conf)
  * [drduh/config - gpg.conf](https://github.com/drduh/config/blob/master/gpg.conf)
  * See Also [[Web Content and Supply Chain Signing Security]]

#### Signing Commits using SSH Keys
You can also sign commits using your SSH keys (requires Allowed Signers file).
[Signing Commits with SSH](https://dev.to/mrtrom/git-config-a-beginners-guide-with-advanced-tips-393f)
* **Allowed Signers File**: Used specifically for verifying commit signatures when using SSH for signing. It specifies which keys are trusted for signing.
* If you're working with multiple accounts, using the SSH config file makes it easier to manage key mappings without additional complexity.
* Telling git about your SSH signing key:
```sh
# Use SSH to sign commits and tags
git config --global gpg.format ssh
# Set path to public key 
git config --global user.signingkey /PATH/TO/.SSH/KEY.PUB
```
#### GPG/SSH Commit Signing with Multiple Accounts:
[[git - multiple identity management, vscode config#GPG and SSH commit signing with multiple identities or accounts]]

---
### Managing Git Repositories

* [git guide](https://github.com/git-guides) Git, from getting started to advanced commands and workflows.
* [[Atlassian-Git-Cheatsheet.pdf]]

##### Removing Files from Git Repository
  - Remove `.vscode` files from Git Repo (recursive, doesn't remove locally): 
```sh
  git rm -r --cached .vscode 
  git commit -m "removed .vscode" 
  git push origin main
```
  - Remove `.DS_Store` files from Git repo 
```sh
find . -name .DS_Store -print0 | xargs -0 git rm -f --ignore-unmatch
git add .
git commit -m '.DS_Store banished!'
```

**Clearing sensitive data from Git history:**
- Use tools like [`BFG Repo-Cleaner`](https://github.com/rtyley/bfg-repo-cleaner) or [`git-filter-repo`](https://github.com/newren/git-filter-repo) to scan and remove sensitive data from the repository’s history.
- Employ the `git rm` command to remove the sensitive file and make a commit, followed by using `git filter-branch` or `git filter-repo` to erase it from the Git history.
[git + cyber security](https://www.gitwiki.org/git-and-cyber-security/)
##### .gitignore
- **Global Exclusion List**
  - Specify Exclusion File: `git config --global core.excludesfile ~/.gitignore`
  - Add `.DS_Store` to Exclusion List:
    ```bash
    touch ~/.gitignore
    echo .DS_Store >> ~/.gitignore
    ```

* Example .gitignore [[example - gitconfig, ssh config, gitignore#.gitignore example]]
* [Stackoverflow - Git Ignore Global List, .DS_Store](https://stackoverflow.com/questions/107701/how-can-i-remove-ds-store-files-from-a-git-repository)

---

### Resolve Divergent Branches

1. **Check the Status**:
   Start by checking the status of your branch:
   ```bash
   git status
   ```
2. **Fetch Changes**:
   Fetch the latest changes from the remote repository:
   ```bash
   git fetch origin
   ```
3. **Choose a Merge Strategy**:
   You need to decide how to reconcile the branches. Here are the options you can choose:
   - **Merge** (default behavior):
     ```bash
     git pull origin main
     ```
   - **Rebase**:
     If you prefer a cleaner commit history, you can rebase your changes on top of the remote branch:
     ```bash
     git pull --rebase origin main
     ```
   - **Fast-Forward Only**:
     If you want to only allow fast-forward merges (this will fail if the branches have diverged):
     ```bash
     git pull --ff-only origin main
     ```
4. **Set a Default Strategy** (Optional):
   You can set a default strategy for future pulls. For example, if you want to always rebase:
   ```bash
   git config --global pull.rebase true
   ```
   Or to always merge:
   ```bash
   git config --global pull.rebase false
   ```
5. **Resolve Any Conflicts**:
   If there are conflicts after the merge or rebase, Git will notify you. You'll need to resolve those conflicts manually in your files, then mark them as resolved:
   ```bash
   git add <file-with-conflict>
   ```
   After resolving conflicts, complete the merge or rebase with:
   ```bash
   git commit   # for merge
   ```
   or 
   ```bash
   git rebase --continue  # for rebase
   ```
6. **Push Changes**:
   Once everything is resolved and committed, you can push your changes back to the remote:
   ```bash
   git push origin main
   ```

**Summary**
1. Fetch the latest changes.
2. Choose a merge strategy (merge, rebase, or fast-forward).
3. Optionally set a default strategy.
4. Resolve any conflicts if necessary.
5. Push your changes back to the remote repository.

---

### Git Environment Variables (Debug)

- **`GIT_SSH_COMMAND`**: Customizes the SSH command used by Git, useful for debugging SSH connections.
  ```sh
  GIT_SSH_COMMAND="ssh -v" git clone git@github.com:cspence001/hindsight.git
  ```

- **`GIT_CURL_VERBOSE`**: Enables detailed HTTP(S) request/response logging for Git operations, useful for troubleshooting HTTP(S) connections.
  ```sh
  GIT_CURL_VERBOSE=1 git clone https://github.com/cspence001/hindsight.git
  ```

---
### Advanced Git Topics

* **Official Docs**
 * [github/docs/tree/main/content](https://github.com/github/docs/tree/main/content)

- **Git LFS**
  - [Git LFS](https://git-lfs.com/)
  - [Git LFS Issues](https://github.com/git-lfs/git-lfs/issues/5024)

- **SSH Agent Forwarding**
  - [SSH Agent Forwarding](https://www.howtogeek.com/devops/what-is-ssh-agent-forwarding-and-how-do-you-use-it/)
  - [SSH Agent Forwarding - GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding#troubleshooting-ssh-agent-forwarding)
  - [Managing Deploy Keys](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys)

- **Managing Multiple GitHub Accounts**
  - [Managing Multiple Accounts](https://stackoverflow.com/a/73329289)
  - [Another Method](https://stackoverflow.com/a/43009365)

- **2FA CLI**
  - [CLI-Based TOTP Generators](https://www.reddit.com/r/github/comments/1d743kp/comment/l720uj0)
  - [Gauth CLI Tool](https://github.com/pcarrier/gauth)
  - [TOTP CLI Tool](https://github.com/yitsushi/totp-cli)
  - [offline-capable TOTP & HOTP Authenticator](https://authenticator.inbrowser.app/)

---

### Advanced SSH Topics

---
##### Global (macOS) Server / Client SSH 
[drduh/macOS-Security-and-Privacy-Guide - SSH](https://github.com/drduh/macOS-Security-and-Privacy-Guide?tab=readme-ov-file#ssh)

For outgoing SSH connections, use hardware or password-protected keys, [set up](http://nerderati.com/2011/03/17/simplify-your-life-with-an-ssh-config-file/) remote hosts and consider [hashing](http://nms.csail.mit.edu/projects/ssh/) them for added privacy. 

* [ssh_config](https://github.com/drduh/config/blob/master/ssh_config) (recommended client options for outgoing connections)
* [sshd_config](https://github.com/drduh/config/blob/master/sshd_config) (recommended config when allowing incoming ssh connections, sshd disabled by default on macOS)

You can also use ssh to create an [encrypted tunnel](http://blog.trackets.com/2014/05/17/ssh-tunnel-local-and-remote-port-forwarding-explained-with-examples.html) to send traffic through, similar to a VPN. 

See [[Permissions]] for ensuring correct permissions for `/etc/ssh` directory.

---
