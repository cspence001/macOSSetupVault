To check the integrity of your file system on macOS, you can use the built-in tools `diskutil` and `fsck`. Here’s how to use each tool to ensure your file system is in good health:

### Using `diskutil` to Check File System Integrity

1. **Open Terminal**:
   - You can open Terminal by going to **Applications** > **Utilities** > **Terminal**.

2. **List Available Volumes**:
   - To identify the disk or volume you want to check, use the command:
     ```sh
     diskutil list
     ```
   - This command lists all disks and volumes, providing information such as the identifier (e.g., `/dev/disk1s1`).

3. **Verify the File System**:
   - To check the integrity of a specific volume (e.g., your main macOS volume), use:
     ```sh
     diskutil verifyVolume /Volumes/YourVolumeName
     ```
   - Replace `YourVolumeName` with the name of the volume you want to check. If you’re checking the main macOS volume, you might use `/` for the root volume.

4. **Repair the File System (if needed)**:
   - If `diskutil verifyVolume` finds issues, you can attempt to repair them with:
     ```sh
     diskutil repairVolume /Volumes/YourVolumeName
     ```
   - For the main macOS volume, this usually requires booting into Recovery Mode (see instructions below).

### Using `fsck` to Check File System Integrity

The `fsck` command is used for checking and repairing the file system, but it must be run from Single User Mode or Recovery Mode because you cannot repair the file system while it is mounted and in use.

1. **Restart in Recovery Mode**:
   - Restart your Mac and hold down `Command (⌘) + R` immediately after you hear the startup chime. Keep holding these keys until you see the Apple logo or a spinning globe.

2. **Open Terminal in Recovery Mode**:
   - Once in Recovery Mode, you’ll see the macOS Utilities window. Go to the menu bar and select **Utilities** > **Terminal**.

3. **Identify the Disk**:
   - Use `diskutil` to list all disks and find the identifier for the volume you want to check:
     ```sh
     diskutil list
     ```
   - Note the identifier for the volume (e.g., `/dev/disk1s1`).

4. **Run `fsck`**:
   - Execute the `fsck` command on the identified volume. If you are checking the main volume, use:
     ```sh
     fsck_apfs -n /dev/disk1s1
     ```
   - Replace `/dev/disk1s1` with the correct identifier for your volume.

   - For HFS+ volumes (older macOS systems), use:
     ```sh
     fsck_hfs -n /dev/disk1s1
     ```

   - The `-n` option tells `fsck` to run in "no-write" mode, meaning it will only check the file system and not make any changes. Remove `-n` to allow `fsck` to repair any issues found.

5. **Restart Your Mac**:
   - After running `fsck`, restart your Mac normally.

### Summary

- **`diskutil`**: Useful for verifying and repairing volumes while macOS is running, though it’s best suited for basic checks and repairs.
- **`fsck`**: Provides deeper file system checks and repairs but requires booting into Recovery Mode or Single User Mode.

Always ensure you have backups of your important data before running file system checks and repairs, especially if repairs are necessary, to prevent potential data loss.