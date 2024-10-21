
---
### Python Versions and Paths

#### Listing Python Versions on Your System
To enumerate all Python versions installed on your system, use:
```bash
% ls -l /usr/local/bin/ | grep python
% ls -l /usr/bin/ | grep python
```

#### Python Versions with Homebrew
Here are some symlinks created by Homebrew for Python 3.11:
```plaintext
lrwxr-xr-x  1 $USER  admin    42 Jun  5 21:51 2to3-3.11 -> ../Cellar/python@3.11/3.11.9/bin/2to3-3.11
lrwxr-xr-x  1 $USER  admin    41 Jun  5 21:51 idle3.11 -> ../Cellar/python@3.11/3.11.9/bin/idle3.11
lrwxr-xr-x  1 $USER  admin    40 Jun  5 21:51 pip3.11 -> ../Cellar/python@3.11/3.11.9/bin/pip3.11
lrwxr-xr-x  1 $USER  admin    42 Jun  5 21:51 pydoc3.11 -> ../Cellar/python@3.11/3.11.9/bin/pydoc3.11
lrwxr-xr-x  1 $USER  admin    38 Jun  5 21:51 python-build -> ../Cellar/pyenv/2.4.1/bin/python-build
lrwxr-xr-x  1 $USER  admin    43 Jun  5 21:51 python3.11 -> ../Cellar/python@3.11/3.11.9/bin/python3.11
lrwxr-xr-x  1 $USER  admin    50 Jun  5 21:51 python3.11-config -> ../Cellar/python@3.11/3.11.9/bin/python3.11-config
lrwxr-xr-x  1 $USER  admin    42 Jun  5 21:51 wheel3.11 -> ../Cellar/python@3.11/3.11.9/bin/wheel3.11
```

#### Python Versions Installed via Python.org or Other Installers
For Python 3.11 and 3.7 installations:
```plaintext
lrwxr-xr-x  1 $USER  admin    78 Aug  1  2023 python3-intel64 -> ../../../Library/Frameworks/Python.framework/Versions/3.11/bin/python3-intel64
lrwxr-xr-x  1 $USER  admin    81 Aug  1  2023 python3.11-intel64 -> ../../../Library/Frameworks/Python.framework/Versions/3.11/bin/python3.11-intel64

lrwxr-xr-x  1 $USER  admin    71 Mar  1  2023 python3.7 -> ../../../Library/Frameworks/Python.framework/Versions/3.7/bin/python3.7
lrwxr-xr-x  1 $USER  admin    78 Mar  1  2023 python3.7-config -> ../../../Library/Frameworks/Python.framework/Versions/3.7/bin/python3.7-config
lrwxr-xr-x  1 $USER  admin    72 Mar  1  2023 python3.7m -> ../../../Library/Frameworks/Python.framework/Versions/3.7/bin/python3.7m
lrwxr-xr-x  1 $USER  admin    79 Mar  1  2023 python3.7m-config -> ../../../Library/Frameworks/Python.framework/Versions/3.7/bin/python3.7m-config
```

#### Checking Symlinks
To list all symlinks related to Python, use:
```bash
% ls -la /usr/local/bin | grep "../Frameworks/Python.framework/Versions"
```

#### Default and System Python Versions

**Check Python versions and paths**:
```bash
% which python3
/usr/bin/python3

% which python3.7
/usr/local/bin/python3.7

% which python3.11
/usr/local/bin/python3.11
```

**Inspect embedded library file links**:
```bash
% otool -L /usr/bin/python3
/usr/bin/python3:
    /usr/lib/libxcselect.dylib
    /usr/lib/libSystem.B.dylib
```

**Inspect embedded library file links from within your virtual environment**:
```
% otool -L bin/python
python:
    @executable_path/../Python
    /usr/lib/libSystem.B.dylib
```

### Python Site Configurations

- **User and Site Packages**:
User site-packages is where Python installs packages available only for you. But the packages will still be visible to all Python projects that you create.
  ```bash
  % python3 -m site --user-base
  /Users/$USER/Library/Python/3.11
  % python3 -m site --user-site
  /Users/$USER/Library/Python/3.9/lib/python/site-packages
  ```
- --user-base
	Print the path to the user base directory.
- --user-site
	Print the path to the user site-packages directory.
	User site-packages is where Python installs packages available only for you. But the packages will still be visible to all Python projects that you create

- `/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/site.py [--user-base --user-site]`

- Check global site-packages directory:
 Global site-packages is where Python installs packages that will be available to all users and all Python applications on the system. Shows both user-specific and global directories, user base paths, and whether user site packages are enabled.
  ```sh
  % python3 -m site
  ```

  ```sh
  % python3 -m site --help
```

Lists only the global site-packages directories, not user-specific paths or enablement status:
```sh
  % python3 -c 'import site; print(site.getsitepackages())'
```

Output:
```sh
['/Library/Frameworks/Python.framework/Versions/3.11/lib/python3.11/site-packages']
```

- **Site Config Scripts**: [Python Site Configurations](https://pymotw.com/3/site/) (Review)
  ```sh
  % cd ~/folder/macsetup/_scripts/python_siteconfigs
  % python3 site_import_path.py
  % python3 site_user_base.py #print user-base, site location
  % python3 site_enable_user_site.py
  ```
`site_user_base.py`
	This Python script, `site_user_base.py`, uses the `site` module to display information about user-specific site directories in Python. It prints the `USER_BASE` and `USER_SITE` paths, which are locations where Python installs user-specific packages and configurations. It also explains the role of these paths and their defaults across different operating systems.
`site_enable_user_site.py`
	The Python script `site_enable_user_site.py` uses the `site` module to check and display the status of the `ENABLE_USER_SITE` flag, which determines whether the user-specific site directory is enabled. It provides an explanation of the flag’s status, including its default behavior and how it can be disabled for security reasons. The script also shows how to explicitly disable the user site directory using the command line option `-s`.
`site_import_path.py`
	The Python script examines how the `site` module automatically extends `sys.path` with site-specific directories upon interpreter startup. It demonstrates how `site` combines prefix values (from `sys.prefix` and `sys.exec_prefix`) with various suffixes to construct paths for site-packages. The script lists and checks the existence of these paths on the system and whether they are included in `sys.path`. It provides specific information for both Windows and Unix-like platforms, showing how paths are determined and tested.

---
### Pip and Virtualenv

- **Default Pip Locations**:
  ```sh
  % which pip
  /usr/local/bin/pip 

  % which pip3
  /usr/bin/pip3 

  % pip3 --version 
  pip 23.2.1 from /usr/local/lib/python3.11/site-packages/pip (python 3.11)
  ```
- Note: the directory output by `which pip` determines the location of packages installed via `pip/pip3 install`. https://stackoverflow.com/a/37894455
- (e.g. Symlink to `/Library/Frameworks/Python.framework/Versions/3.11/bin/pip3`)
 
 - **Pip Configuration Files**:
  - **Global**: `$HOME/Library/Application Support/pip/pip.conf`
  - **User**: `$HOME/.pip/pip.conf`
  - **Site**: `$VIRTUAL_ENV/pip.conf`
    *The environment variable PIP_CONFIG_FILE can be used to specify a configuration file that’s loaded first*
    [pip config](https://pip.pypa.io/en/stable/topics/configuration/)
    - `pip3 config debug` displays global, site, user env
    
- **Virtualenv**:
  ```sh
  % virtualenv --version
  virtualenv 20.26.3 from /Users/$USER/Library/Python/3.9/lib/python/site-packages/virtualenv/__init__.py
  ```
  
```sh
% virtualenv -h
```

* [VIRTUALENV_CONFIG_FILE](https://virtualenv.pypa.io/en/latest/cli_interface.html#configuration-file) `~/Library/Application Support/virtualenv/virtualenv.ini` 
* [Environment Variables](https://virtualenv.pypa.io/en/latest/cli_interface.html#environment-variables)

---
### Notes on Installation and Paths

- **System Paths**:
  - `/System/Library/Frameworks`: Reserved for Apple-provided frameworks.
  - `/Library/Frameworks`: For public frameworks and dynamic linking.
  - `~/Library/Frameworks`: For frameworks linked to specific applications.
[Framework Location](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPFrameworks/Tasks/InstallingFrameworks.html)

- **Command Line Tools Installation**:
Check installation paths:
  ```bash
  % xcode-select -p
  ```
Python3 system configuration:
```bash
  % python3 -m sysconfig
```
System-level paths:
  ```bash
  % python3 -c "import sysconfig; print(sysconfig.get_paths())"
  ```

Output:
```sh
{'stdlib': '/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9', 
'platstdlib': '/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9', 
'purelib': '/Library/Python/3.9/site-packages', 
'platlib': '/Library/Python/3.9/site-packages', 
'include': '/Library/Python/3.9/include', 
'platinclude': '/Library/Python/3.9/include', 
'scripts': '/usr/local/bin', 
'data': '/Library/Python/3.9'}
```

**stdlib**: directory containing the standard Python library files that are not platform-specific.
**platstdlib**: directory containing the standard Python library files that are platform-specific.
**platlib**: directory for site-specific, platform-specific files.
**purelib**: directory for site-specific, non-platform-specific files.
**include**: directory for non-platform-specific header files.
**platinclude**: directory for platform-specific header files.
**scripts**: directory for script files.
**data**: directory for data files.

### Useful Links
- [Homebrew Python Symlink Warning](https://stackoverflow.com/a/19961550)
- [Python Framework Installation](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPFrameworks/Tasks/InstallingFrameworks.html)
- [Python Site Configurations](https://pymotw.com/3/site/)
- [Virtualenv CLI Interface](https://virtualenv.pypa.io/en/latest/cli_interface.html#configuration-file)
- [PYTHONPATH vs PYTHON_BIN_PATH](https://stackoverflow.com/a/47111756/14436643)

---

### macOS Python Installations

#### System Python
- **Location**: `/System/Library/Frameworks/Python.framework` and `/usr/bin/python`
- **Supported on**: macOS 10.8-12.3

#### Python Command Line Tools (CLT) 3.9.6
- **Location**: `/usr/bin/python3`
- **Framework Locations**: `/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9`
- **Library Paths**:
  - `/Library/Python/3.9`
- **User Library**: `~/Library/Python/3.9` (use `python3 -m pip install <pkg>`)
- **Site Packages**:
  - `/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/site-packages`
  - `/Library/Python/3.9/site-packages`
- **Internal Locations**:
  - `/AppleInternal/Library/Python/3.9/site-packages`
  - `/AppleInternal/Tests/Python/3.9/site-packages`
  
When you have installed Xcode and/or Command Line Tools for Xcode (`/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/bin`), they install and link an older (3.9.6) version of Python3 into `/usr/bin/{python3, pip3}`. 
Any third-party Python modules installed by that version of pip3 will be placed in:
`~/Library/Python/3.9/lib/python/site-packages`.

```bash
 $ python3 -c 'import sysconfig; print(sysconfig.get_paths()["purelib"])'
/Library/Python/3.9/site-packages
```
#### Official Python 3.11.4 (3rd-p install) official Python website installer.
- **Location**: `/usr/local/bin/python3`
- **Framework**: `/Library/Frameworks/Python.framework/Versions/3.11`
  - **Executable Path**: `/Library/Frameworks/Python.framework/Versions/3.11/bin` (sets `PYTHON_ROOT="/Library/Frameworks/Python.framework/Versions/3.11"/bin` in PATH)
  - **Symlink**: `/usr/local/bin/python3` (points to the above executable)
- **User Base**: `~/Library/Python/3.11` (use `python3 -m pip install <pkg>`)
- **Additional Installations**:
  - Python 3.11 folder in `Applications`
  
When you install Python3 (3.12.1) from Python.org, it is installed into:
`/Library/Frameworks/Python.framework/Versions/3.12`
and its binaries are linked into `/usr/local/bin/{python3, idle3, pip3}`. 
Any third-party Python modules are then installed into:
`/Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages`.
#### Python Homebrew 3.11.9
- **Executable Location**: `/usr/local/bin/`
- **Path**: `../Cellar/python@3.11/3.11.9/bin/python3.11`
	homebrew note: (https://stackoverflow.com/a/19961550)
	Warning: /usr/bin occurs before /usr/local/bin
	This means that system-provided programs will be used instead of those provided by Homebrew.
#### Python Version 3.7
- **Executable Locations**:
  - `/usr/local/bin/python3.7`
  - `../../../Library/Frameworks/Python.framework/Versions/3.7/bin/python3.7`
  - `../../../Library/Frameworks/Python.framework/Versions/3.7/bin/python3.7m`
