The difference between `pip install` and `pip install --user` lies in where the installed packages are placed and how they affect your Python environment. Here's a detailed comparison:

### `pip install`

**Command:**
```bash
pip install package_name
```

**Description:**
- **Global Installation**: By default, `pip install` installs packages globally for the Python interpreter that is currently active. This means the packages are available to all users on the system and all projects using that Python interpreter.
- **Permissions**: Requires administrative (root) permissions on Unix-like systems or the use of an elevated command prompt on Windows if you are installing into the system-wide Python environment.
- **Virtual Environments**: When used within a virtual environment, it installs packages into that virtual environment, isolated from the global Python installation.

**Use Case:**
- Suitable when you want to install packages for all users on the system or when using a virtual environment.

### `pip install --user`

**Command:**
```bash
pip install --user package_name
```

**Description:**
- **User-Specific Installation**: Installs packages into the user’s local directory rather than the system-wide site-packages directory. This directory is typically located in `~/.local/` on Unix-like systems and `%APPDATA%\Python\PythonXX\site-packages` on Windows.
- **Permissions**: Does not require administrative (root) permissions, as it installs the packages in the user’s home directory. This is useful if you do not have permission to install packages system-wide or prefer not to affect other users on the system.
- **Environment Isolation**: Packages installed with `--user` are available only to the user who installed them and are not isolated like packages installed within a virtual environment.

**Use Case:**
- Useful for installing packages when you do not have administrative rights or prefer not to affect other system users. It is also helpful for avoiding conflicts with system-wide packages.

### Summary of Differences

1. **Installation Location:**
   - **`pip install`**: Installs packages globally or into the virtual environment, affecting all users or projects using the global or virtual environment Python interpreter.
   - **`pip install --user`**: Installs packages in the user’s local directory, isolating them from the global Python environment and affecting only the current user.

2. **Permissions:**
   - **`pip install`**: May require administrative permissions on the system unless used within a virtual environment.
   - **`pip install --user`**: Does not require administrative permissions, suitable for users without elevated rights.

3. **Impact:**
   - **`pip install`**: Can affect all users and all projects using the Python interpreter.
   - **`pip install --user`**: Affects only the user’s Python environment, avoiding potential conflicts with other users or system-wide packages.

4. **Virtual Environments:**
   - **`pip install`**: When used within a virtual environment, installs packages into that environment.
   - **`pip install --user`**: Ignored if used within a virtual environment; the package is installed in the user’s local directory regardless of the virtual environment.

### When to Use Each

- **Use `pip install`** when you need to install packages globally (with the necessary permissions) or within a specific virtual environment.
- **Use `pip install --user`** when you want to install packages without administrative rights or when you want to avoid modifying the global Python installation.

By understanding these differences, you can better manage your Python package installations based on your specific needs and environment.