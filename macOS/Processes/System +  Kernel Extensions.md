

---

## Managing System Extensions

### Listing System Extensions

To list currently active system extensions:
```sh
systemextensionsctl list
```

### Removing or Disabling System Extensions

**Using `systemextensionsctl`:**

- **Remove a System Extension:**
  ```bash
  sudo systemextensionsctl uninstall <teamID> <bundleID>
  ```
  Replace `<teamID>` and `<bundleID>` with the appropriate values from your list.

  Example:
  ```bash
  sudo systemextensionsctl uninstall 2MMRE5MTB8 com.obsproject.obs-studio.mac-camera-extension
  ```

**Using System Preferences:**
1. **Open System Preferences:**
   - From the Apple menu (), select "System Preferences."
2. **Select Extensions:**
   - Go to the "Extensions" or "Profiles" section (the location may vary depending on your macOS version).
3. **Remove or Disable Extensions:**
   - Find the extension you want to manage and disable or remove it.

**Verify Changes:**
To confirm the removal or disabling of an extension, run:
```bash
systemextensionsctl list
```

**Reboot Your System:**
A reboot may be necessary to fully apply changes.

**Notes:**
- **Check for Software Updates:** Ensure that removing an extension does not impact other functionalities or applications.
- **Reinstalling Extensions:** If needed, reinstall extensions by re-downloading or reinstalling the associated application.

---

## Managing Kernel Extensions

### Listing Kernel Extensions

**Using `kextstat`:**

- **List Currently Loaded Kernel Extensions:**
  ```bash
  kextstat
  ```
  To filter out header information:
  ```bash
  kextstat | grep -v '^Index'
  ```

- **Search for a Specific Kernel Extension:**
  ```bash
  kextstat | grep <keyword>
  ```
  Replace `<keyword>` with the name or bundle identifier.

**Using `kextcache`:**

- **Show Details of Kernel Extensions in System Cache:**
  ```bash
  kextcache -l
  ```
  Note: This may provide less detail compared to `kextstat`.

### Removing or Disabling Kernel Extensions

**Using the Command Line:**

- **Unload a Kernel Extension:**
  ```bash
  sudo kextunload -b <bundleID>
  ```
  Replace `<bundleID>` with the kernel extension’s bundle identifier.

  Example:
  ```bash
  sudo kextunload -b com.example.kernelextension
  ```

### Checking Installed Kernel Extensions

**Checking Installed but Unloaded Kexts:**

- **Kernel Extensions Directory:**
  ```bash
  ls /Library/Extensions
  ```

- **System Extensions Directory:**
  ```bash
  ls /System/Library/Extensions
  ```
  Note: In macOS Catalina and later, system extensions may be managed differently and could be found in different locations or managed via System Preferences.

**Using System Information:**
1. **Open System Information:**
   - Click the Apple menu () and select "About This Mac."
   - Click the "System Report" button.
2. **Navigate to Extensions:**
   - In the "Software" section, select "Extensions."

   This section lists all loaded kernel extensions with details such as bundle identifiers and versions.
