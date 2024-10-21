
download node.js/npm installer for macOS
https://nodejs.org/en/download/prebuilt-installer

The error message indicates that there is a permissions issue when trying to copy the `irestore` binary to `/usr/local/bin/irestore`. This is a common issue on macOS when attempting to install packages globally without sufficient permissions.

Here are a few ways you can resolve this issue:

##### 1. Use `sudo`
You can run the installation command with `sudo` to grant elevated permissions:
```bash
sudo npm install
```
This will prompt you for your password and then run the command with administrative rights.
##### 2. Change Ownership of `/usr/local/bin`
If you prefer not to use `sudo` every time, you can change the ownership of the `/usr/local/bin` directory to your user account:
```bash
sudo chown -R $(whoami) /usr/local/bin
```
After changing the ownership, try running the `npm install` command again.
##### 3. Use nvm (Node Version Manager)
If you're not already using [nvm](https://github.com/nvm-sh/nvm), consider installing it. `nvm` allows you to manage multiple versions of Node.js and installs them in your home directory, eliminating the need for elevated permissions:
1. **Install nvm**:
   Follow the instructions on the [nvm GitHub page](https://github.com/nvm-sh/nvm#installing-and-updating).
2. **Install Node.js via nvm**:
   ```bash
   nvm install node
   ```
3. **Use nvm's Node.js**:
   Set the default Node.js version if necessary:
   ```bash
   nvm alias default node
   ```
4. **Try Installing Again**:
   Now run the `npm install` command again in your project directory.
##### 4. Check npm's Prefix Configuration
If you want to avoid using `sudo` and don't want to change permissions, you can configure npm to use a different directory for global installs:
1. **Create a directory in your home**:
   ```bash
   mkdir "${HOME}/.npm-global"
   ```
2. **Configure npm to use this directory**:
   ```bash
   npm config set prefix "${HOME}/.npm-global"
   ```
3. **Add the new directory to your PATH**:
   Add the following line to your shell profile (e.g., `~/.bashrc`, `~/.zshrc`, etc.):
   ```bash
   export PATH="$HOME/.npm-global/bin:$PATH"
   ```
4. **Reload your shell**:
   ```bash
   source ~/.bashrc  # or source ~/.zshrc
   ```
5. **Try Installing Again**:
   Now run `npm install` again.


---

### npm Commands and Tools

#### Create a React App
- **Create a new React application:**
  ```sh
  npx create-react-app <project_name>
  ```

---

### List Dependencies

- **List all installed dependencies in the current project:**
  ```sh
  npm list
  ```
- **List globally installed dependencies:**
  ```sh
  npm list -g
  ```
- **List dependencies with different depths:**
  - **Depth 0 (only top-level dependencies):**
    ```sh
    npm list --depth=0
    ```
  - **Depth 1:**
    ```sh
    npm list --depth=1
    ```
  - **Depth 2:**
    ```sh
    npm list --depth=2
    ```
  - **Depth 3:**
    ```sh
    npm list --depth=3
    ```

- **Print the dependency tree for production packages (omit dev dependencies):**
  ```sh
  npm list --all --omit=dev
  ```

- **List dependencies of a specific package (e.g., `react-scripts` version `5.0.1`):**
  ```sh
  npm view react-scripts@5.0.1 dependencies
  ```

- **View the dependency tree of an npm module:**
  - [Dependency Tree Listing](https://bobbyhadz.com/blog/view-dependency-tree-of-npm-module)

---

### Track Dependencies

- **`depcheck` - Track unused dependencies:**
  - **Install:**
    ```sh
    npm install -g depcheck
    ```
  - **Run:**
    ```sh
    depcheck
    ```

- **`npm-check` - Interactive dependency check and cleanup:**
  - **Install:**
    ```sh
    npm install -g npm-check
    ```
  - **Run:**
    ```sh
    npm-check
    ```

- **Guide to removing unused dependencies in React:**
  - [How to Remove Unused Dependencies](https://www.pluralsight.com/resources/blog/guides/how-to-remove-unused-dependencies-in-react)

---

### Uninstall Packages

- **Uninstall a package:**
  ```sh
  npm uninstall <package_name>
  ```

---

### Clean Cache

- **Force clean npm cache:**
  ```sh
  npm cache clean --force
  ```

---

### `.npmrc` Configuration

- **Flags for npm installations:**
  - **`--force`**: Forces npm to perform actions that might be considered unsafe.
  - **`--legacy-peer-deps`**: Ignores peer dependencies when building a package tree. This was default behavior in npm versions 3 through 6.
    - [More about `--legacy-peer-deps`](https://weekendprojects.dev/posts/how-to-use-npm-legacy-peer-deps-command/#what-does-the---legacy-peer-deps-flag-do)

---

### Common Create React App Tasks

- **Create React App example and tasks:**
  - [Create React App Example](https://github.com/react-navigation/create-react-app-example)

---

### React App Cleaners

- **`npkill` - Clean up unused npm packages:**
  - [npkill GitHub](https://github.com/voidcosmos/npkill)

- **`react-app-cleaner` - Tool to clean React app files:**
  - [React App Cleaner GitHub](https://github.com/vinc86/react-app-cleaner)

- **Remove unnecessary files in a React app:**
  - [Creating Shell Command for Formatting Files](https://medium.com/developer-student-clubs-tiet/creating-shell-command-for-formatting-files-in-newly-created-create-react-app-c2e74ad488c0)

---

#### Updating packages

**Check outdated packages/ dependencies:**
```sh
npm outdated
```

##### Using `npm update`

1. **Open your terminal** and navigate to your project directory.
2. **Run the following command**:
   ```bash
   npm update
   ```
   This command will update all the packages to the latest version according to the version ranges specified in your `package.json` file.

##### Full Update (node_modules):

* **Clean the Project**: Start fresh by removing `node_modules` and the lock file.
```sh
rm -rf node_modules package-lock.json
npm install
```

Before `npm install` **Check the `package.json`**: Check your `package.json` for unused packages, non-update dependencies `==` etc.  if you intend to use a plugin that has a specific range. If you need a specific version, you can specify it there.

##### Update specific package:

**Update specific package/dependency to latest version:**
```sh
npm install package@latest
```

**Manually Edit `package.json`**
* If you want to update to the latest versions (including breaking changes), you can manually edit your `package.json`:

1. Open `package.json` and change the version numbers for the dependencies and devDependencies to `"latest"` or the specific version you want.
   Example:
   ```json
   "dependencies": {
     "package-name": "latest"
   }
   ```

2. After making changes, run:
   ```bash
   npm install
   ```

##### Using `npm-check-updates`
[npm-check-updates](https://github.com/raineorshine/npm-check-updates)
* For more control and to update to the latest versions without manually editing the `package.json`, you can use the `npm-check-updates` package:

1. **Install `npm-check-updates` globally**:
   ```bash
   npm install -g npm-check-updates
   ```

2. **Run `npm-check-updates`**:
   ```bash
   ncu
   ```
   This will show you which packages can be updated.

3. **Update all packages**:
   ```bash
   ncu -u
   ```

4. **Install the updated packages**:
   ```bash
   npm install
   ```

##### After Updating

1. **Check for Vulnerabilities**:
   After updating, it's a good idea to check for vulnerabilities:
   ```bash
   npm audit
   ```
2. **Run Your Tests**:
   Make sure to run your tests to confirm that everything works as expected after the updates.

