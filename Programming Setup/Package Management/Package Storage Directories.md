### Managing manually installed software
For managing packages manually without Homebrew, you have a few options for where to store binaries or source code on macOS. 
##### Common Directories for Storing Packages

1. **`/usr/local/bin`**:
   - This directory is typically used for user-installed binaries. You can place executable files here so they can be accessed from the terminal.

2. **`/usr/local/lib`**:
   - You can store shared libraries or other supporting files here.

3. **`/opt`**:
   - This directory is often used for installing third-party software that doesn't conform to the standard file hierarchy. You can create subdirectories for specific applications (e.g., `/opt/myapp`).

4. **`~/Applications` or `~/UserApplications`**:
   - If you want to keep things user-specific, you can create an `Applications` directory in your home folder. This is useful for applications that you don’t want to install system-wide.

5. **`~/bin`**:
   - For personal scripts or executables, you can create a `bin` directory in your home folder (`~/bin`). Make sure to add this directory to your PATH environment variable if you want to run binaries from it easily.

6. **`/usr/local/src`**:
   - For source code that you clone from repositories, `/usr/local/src` is a common place to keep the source code. This keeps it organized and separate from binaries.

Choose a location based on whether you want the software to be available system-wide or just for your user account. Here’s a quick guideline:

- **System-wide binaries**: `/usr/local/bin`
- **User-specific applications**: `~/Applications` or `~/bin`
- **Third-party software**: `/opt/myapp` or similar
- **Source code**: `/usr/local/src` 

