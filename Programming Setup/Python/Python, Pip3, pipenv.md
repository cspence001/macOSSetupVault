r  
---
### Xcode Installation

- **Brew Install**: [Xcode Command Line Tools Install Guide](https://mac.install.guide/commandlinetools/3)
- **Direct Install**: Use `xcode-select --install`

---
### Python Installation Paths and Checks
- **Python Documentation**: [Python on macOS](https://docs.python.org/3/using/mac.html)
- **Official Installer**: [Download macOS Python Installer](https://www.python.org/downloads/macos/)
- **Check Default Python Version**: Run `% python3 -V`
- **Find Python Location**: `% whereis python3`
- **Help for System Python**: `/usr/bin/python3 -h`

---
### Removing Python Versions

[Removing Python versions, Reinstall](https://gist.github.com/MuhsinFatih/ee0154199803babb449b5bb98d3475f7?permalink_comment_id=4376984)

- **Remove Third-Party Installations**:
  ```sh
  sudo rm -rf /Library/Frameworks/Python.framework
  sudo rm -rf "/Applications/Python 2.7"
  ```
- **Remove pip/pipenv Installs**:
  ```sh
  pip uninstall -y -r <(pip freeze)
  pipenv uninstall --all
  ```
- **Remove 3-p installed modules:
  ```sh
  sudo rm -rf ~/Library/Python/3.8/lib/python/site-packages/
  ```
- **Remove VCS-Installed Packages**:
  ```sh
  pip freeze --exclude-editable | xargs pip uninstall -y
  ```
- **Handle GitHub/GitLab Packages**:
  ```sh
  pip freeze | cut -d "@" -f1 | xargs pip uninstall -y
  ```
  [Stack Overflow on package removal](https://stackoverflow.com/a/11250821)

##### Cleaning up orphaned package dependencies
###### Method 1: Tracking Dependencies installed by package (manual)
```sh
$ pip3 list > beforeinstallrequirements.txt
$ pip3 install pyhindsight
$ pip3 list > afterinstallrequirements.txt
comm -23 <(sort afterinstallrequirements.txt) <(sort beforeinstallrequirements.txt) > unique_packages.txt
#outputs
XlsxWriter                3.2.0
backports.tarfile         1.2.0
bottle                    0.12.25
importlib_metadata        8.4.0
jaraco.classes            3.4.0
jaraco.context            6.0.1
jaraco.functools          4.0.2
keyring                   25.3.0
more-itertools            10.4.0
puremagic                 1.27
pycryptodomex             3.20.0
pyhindsight               20230327.0
zipp                      3.20.0
#uninstall orphaned dependencies
$ xargs -n 1 pip3 uninstall -y < unique_packages.txt
#verify
$ pip3 list > comprequirements.txt
$ comm -23 beforeinstallrequirements.txt comprequirements.txt
#if output clear, cleanup 
$ rm -rfv beforeinstallrequirements.txt afterinstallrequirements.txt unique_packages.txt comprequirements.txt
```
*TO-DO: Write script to save results for all installed packages in hash-type json file (anytime using pip install)*

###### Method 2: Use `pip check` and `pip list`
1. **Check for Broken Dependencies:**
*  This will list any dependencies that are no longer satisfied, which can help you identify orphaned packages.
   ```sh
   pip3 check
   ```
   
2. **List All Packages:**
*  Cross-reference this list with the dependencies required by your remaining packages to identify and remove unnecessary packages.*
   ```sh
   pip3 list
   ```

###### Method 3: Reinstall and Uninstall Cleanly
1. **Create a Virtual Environment:**
   Create a new virtual environment to isolate and test the cleanup process:
   ```sh
   python3 -m venv cleanup-env
   source cleanup-env/bin/activate
   ```

2. **Install `pyhindsight`:**
   Install `pyhindsight` in this clean environment:
   ```sh
   pip3 install pyhindsight
   ```

3. **Uninstall `pyhindsight`:**
   Uninstall `pyhindsight` in this isolated environment and observe what gets removed:
   ```sh
   pip3 uninstall pyhindsight
   ```

4. **Check Remaining Dependencies:**
   Check the remaining packages in this virtual environment to see what’s left and manually remove any that are no longer needed.

5. **Delete the Virtual Environment:**
   After confirming that the cleanup process is correct, you can delete the virtual environment:
   ```sh
   deactivate
   rm -rf cleanup-env
   ```


---

### Python Site-Packages

- **Global Site-Packages**:
  ```sh
  % python3 -m site
  ```
- **User Site-Packages**:
  ```sh
  % python3 -m site --user-base
  ```

---

### Environment Configuration

- **.bash_profile / .zshrc**:
  - **Add Python Path**:
    ```sh
    export PYTHONPATH="/Library/Frameworks/Python.framework/Versions/3.11/bin/python3"
    export PATH="$PYTHONPATH:${PATH}"
    ```
  - **User Python Binary Path**:
    ```sh
    export PYTHON_BIN_PATH="$(python3 -m site --user-base)/bin"
    export PATH="$PYTHON_BIN_PATH:$PATH"
    ```
    *(bin contains pip, pip3, pipenvs, etc)*
    [PYTHON_BIN_PATH](https://stackoverflow.com/a/47111756/14436643)
    [3rd party Install - installing python3 using the installer from python.org](https://apple.stackexchange.com/a/381657)

#### `pythonrc`

A `pythonrc` file is a script that can be used to configure and customize the Python interactive shell environment. It is essentially a Python script that gets executed every time you start an interactive Python session, allowing you to set up custom imports, define functions, and configure settings that you want to be available in every interactive session.

##### Key Points About `pythonrc`:

1. **Purpose**: It allows users to personalize their Python interactive environment. For instance, you can pre-import commonly used libraries, set up convenient functions, or configure other aspects of the interactive shell.

2. **Usage**: To use a `pythonrc` file, you typically set the `PYTHONSTARTUP` environment variable to the path of your `pythonrc` file. For example, in a Unix-like system, you might add the following line to your `.bashrc` or `.zshrc` file:
   ```sh
   export PYTHONSTARTUP=~/.pythonrc.py
   ```
   Here, `~/.pythonrc.py` would be the path to your `pythonrc` file.

3. **Contents**: The contents of a `pythonrc` file can vary widely depending on what the user wants to set up. In your example, the `pythonrc` file includes imports for commonly used modules, such as `pprint` for pretty-printing, `json`, `urllib`, `base64`, `datetime`, and more. It also defines some global constants like `true`, `false`, and `null` to represent boolean and `None` values, which can be convenient for quick testing or interactive work.

Here’s a brief explanation of what the provided `pythonrc` file does:

- **Imports Modules**: It imports a number of standard Python modules that are often used in various tasks, such as `json` for JSON handling, `random` for generating random numbers, `re` for regular expressions, and `yaml` for YAML parsing.
  
- **Aliasing and Constants**: It aliases `pprint` to `p` for convenience and sets up common constants (`true`, `false`, `null`) which are often used in scripting.

This setup can streamline your workflow by preloading libraries and defining frequently used constants or functions, making your interactive Python sessions more productive and customized to your needs.

`pythonrc`
```sh
# https://github.com/drduh/config/blob/master/pythonrc
from pprint import pprint as p
import json
import urllib as u
import base64
import datetime
import itertools
import os
import pdb
import random
import re
import string
import sys
import time
import yaml
from dateutil.tz import tzutc
true = True
false = False
null = None
```

**More Examples**:
- https://github.com/0xf4/pythonrc
- https://github.com/lonetwin/pythonrc/

---

### `pip`/`pip3`

- **Default pip Directory**:
  ```sh
  % which pip
  /usr/local/bin/pip
  ```
- **Default pip3 Directory**:
  ```sh
  % which pip3
  /usr/bin/pip3
  ```
  (Symlink to `/Library/Frameworks/Python.framework/Versions/3.11/bin/pip3`)
  - Note: the directory output by `which pip` determines the location of packages installed via `pip/pip3 install`. https://stackoverflow.com/a/37894455
- **pip3 Version**:
  ```sh
  % pip3 --version
  ```
- **List Installed Packages**:
  ```sh
  % pip3 list
  % pip3 list -v
  ```
- **Install Packages**:
  ```sh
  % pip3 install pandas
  ```
  Installs in `/Users/$USER/Library/Python/3.11/lib/python/site-packages` - latest version PATH (pip3) (manually configured?)
*  If you want to fetch metadata from a PyPI repository without installing, you can use:
```sh
	pip3 install --dry-run hindsight
```
* Or check specific metadata with:
```sh
	pip3 search hindsight
```
- **Upgrading Packages**:
  ```sh
  % pip3 install --upgrade pip
  % pip3 install --user --upgrade pipenv
  ```
  Adding a `--user` flag to `pip install` puts the installed package at `~/Library` instead of `/Library`
- **Install Specific Package Versions**:
  ```sh
  % pip3 uninstall webdriver_manager
  % pip3 install webdriver_manager==3.9
  ```
- **Show Package Info**:
  ```sh
  % pip3 show pytest
  ```

- **Packages Location (Installed via pip3)**: `/Library/Frameworks/Python.framework/Versions/3.11/lib/python3.11/site-packages`

- **Pip Configuration Files**:
  - **Global**: `$HOME/Library/Application Support/pip/pip.conf`
  - **User**: `$HOME/.pip/pip.conf`
  - **Site**: `$VIRTUAL_ENV/pip.conf`
    *The environment variable PIP_CONFIG_FILE can be used to specify a configuration file that’s loaded first*
    [pip config](https://pip.pypa.io/en/stable/topics/configuration/)
    - `pip3 config debug` displays global, site, user env

- **Pip Cache Commands**:
  - `% pip3 cache dir`
  - `% pip3 cache remove <package>` (e.g., `matplotlib`)
  - `% pip3 cache purge`
    - **pip3 cache dir**: Display location of pip cache directory
    - **pip3 cache remove**: Removes all wheel files related to `<package>` from pip's cache
    - **pip3 cache purge**: Clears all wheel files from pip's cache
    [pip cache](https://stackoverflow.com/questions/9510474/pip-uses-incorrect-cached-package-version-instead-of-the-user-specified-version)
  
- **Python Requirements**:
  ```sh
  % pip3 freeze > requirements.txt 
  % pip3 install -r requirements.txt
  ```


**Check for outdated packages**
```bash
pip3 list --outdated
```
This shows all installed packages that have newer versions available, displaying current and latest versions.

**Check a specific package**
```bash
pip3 show packagename
```
This shows detailed info about a specific package, including the current version. You can then check PyPI manually or use:
```bash
pip3 index versions packagename
```
This shows all available versions of a specific package.

**More detailed outdated info**
```bash
pip3 list --outdated --format=columns
```
Formats the outdated packages in a cleaner table format.

**Check what would be upgraded (dry run)**
```bash
pip3 install --upgrade --dry-run packagename
```
Shows what would happen if you upgraded a package without actually doing it.

**Upgrade all outdated packages**
While pip doesn't have a built-in "upgrade all" command, you can combine commands:
```bash
# List outdated packages and upgrade them
pip3 list --outdated | tail -n +3 | awk '{print $1}' | xargs -n1 pip3 install --upgrade
```

Or for a safer approach, upgrade packages one by one:
```bash
pip3 install --upgrade package1 package2 package3
```

**Using pip-review (third-party tool)**
Install it first:
```bash
pip3 install pip-review
```
Then use:
```bash
pip-review --local --interactive
```
This gives you an interactive way to review and selectively upgrade packages.


---

### `venv`
- **Create a Virtual Environment**: `python3 -m venv venv`
- **Activate**: `source ./venv/bin/activate`
- **Deactivate**: `deactivate`
  [Python venv](https://docs.python.org/3/tutorial/venv.html) (Python 3.12.4)

Review Process of dual-env activation:
```sh
% source .venv/bin/activate
% pipenv shell
% pipenv update werkzeug
```

---

### `pipenv`
[pipenv repo](https://github.com/pypa/pipenv)
[pipenv CLI commands](https://fig.io/manual/pipenv/shell)
[Basic Usage](https://docs.pipenv.org/basics/)
  
- **Installation**:
  ```sh
  % pip3 install --user pipenv
  ```
  - This installs `pipenv` in the macOS user base binary directory: "$(python3 -m site --user-base)/bin". If `pipenv` isn't available in your shell after installation, you’ll need to add this directory to your PATH.
  - For example, this will typically print `~/.local` (with `~` expanded to the absolute path to your home directory) so you’ll need to add `~/.local/bin` to your `PATH`. You can set your `PATH` permanently by [modifying ~/.profile](https://stackoverflow.com/a/14638025). 
  - MacOS Framework builds: `~/Library/Python/X.Y` and adding `/bin` to the end `export PATH=~/Library/Python/3.11/bin:$PATH`
	[Pipenv Installation Guide](https://pipenv.pypa.io/en/latest/installation.html)
	[virtualenvs](https://docs.python-guide.org/dev/virtualenvs/)
	[User Installs --user](https://pip.pypa.io/en/stable/user_guide/#user-installs) 

- Alternatively, you can use the Python `-m` module approach:
  ```sh
  % python3 -m pip install pipenv
  ```
  This ensures that `pipenv` is installed with the Python interpreter specified. [Stack Overflow on pipenv installation issues](https://stackoverflow.com/a/59210844)

- **Create Virtual Environment**:
  ```sh
  % pipenv install
  ```
  - The above command will create a new environment if a `Pipfile` doesn’t exist and activate it. Creates `Pipenv` and `Pipfile.lock`.
* Use Python 3 when creating virtual environment
```sh
  % pipenv --three
  ```
- `--two` — Performs the installation in a virtualenv using the system `python2` link.
- `--three` — Performs the installation in a virtualenv using the system `python3` link.
- `--python` — Performs the installation in a virtualenv using the provided Python interpreter.

- **Activate Environment**:
  ```sh
  % pipenv shell
  ```

- **Removing Environment**:
  ```sh
  % pipenv --rm
  ```

- **Environment Variables**:
  ```sh
  #for creating the virtualenv inside your project’s directory.
  export PIPENV_VENV_IN_PROJECT=1
  ```
See: 'Pipenv virtual mapping caveat'

- **Using Pipenv**:
  ```sh
  % pipenv install
  % pipenv install django
  % pipenv uninstall django
  % pipenv install -r requirements.txt
  ```
  
 - If you [(accidentally)](https://stackoverflow.com/a/70609105) installed packages using `pip install` 
 ```sh
% pip freeze #list packages installed using pip
% pip freeze > requirements.txt 
```
- **Install from `requirements.txt`**: 
  ```sh
  % pipenv install -r requirements.txt
  ```
- **Generate `requirements.txt` from `Pipfile.lock`**: 
  ```sh
  % pipenv requirements > requirements.txt
  ```
- **Update Packages**: 
  ```sh
  % pipenv update cryptography
  ```
* **Full Update Process:** (environment, Python version, etc.)
```sh
pipenv --rm
rm Pipfile.lock
#Update python versions, package versions manually in Pipfile
pipenv install
```

- **Run Custom Scripts**: Add scripts in `Pipfile`, e.g., `server = "python manage.py runserver"`
  ```sh
  % pipenv run server
  ```
- **Install dev dependencies**:
  ```sh
  % pipenv install nose --dev
  % pipenv install pytest --dev
  ```
- **Install all dev packages**:
  ```sh
  % pipenv install --dev
  ```
  - **Example Pipfile**:
    ```toml
    [[source]]
    url = "https://pypi.org/simple"
    verify_ssl = true
    name = "pypi"
    [packages]
    django = "*"
    [dev-packages]
    nose = "*"
    [scripts]
    server = "python manage.py runserver"
    [requires]
    python_version = "3.8"
    ```
	**`==` (Exact Version)**
	**`>=` (Minimum Version)**
	**`*` (Any Version)**
	- [PEP 440 – Version Identification and Dependency Specification](https://www.python.org/dev/peps/pep-0440/)
	- [pip version specifiers](https://pip.pypa.io/en/stable/user_guide/#requirements-file-format)
	- **pinned versions:** Convert requirements from == to open specifiers (e.g. * or >=)

- **Replace `.venv` with Pipenv**:
  - Remove `.venv`, `Pipfile`, `requirements.txt`
  - Set environment variable: `export PIPENV_VENV_IN_PROJECT=1`
  ```sh
  % pipenv install
  % pipenv shell
  % pipenv install {proj dependencies}
  ```

- **Set Project-Based Virtualenv**: `export PIPENV_VENV_IN_PROJECT=1`
  - This creates the virtualenv in the `.venv` directory next to the `Pipfile`. Use `unset PIPENV_VENV_IN_PROJECT` to remove the option again. [Stack Overflow on project-based virtualenv](https://stackoverflow.com/a/52540270/14436643)
##### Pipenv Environment Variables
[Advanced Usage](https://docs.pipenv.org/advanced/#configuration-with-environment-variables)
[Environment Variables](https://pipenv.pypa.io/en/latest/configuration.html#configuration-with-environment-variables)
- **Default Python Version**: `PIPENV_DEFAULT_PYTHON_VERSION`
	- Use this version of Python when creating new virtual environments, by default.
	- Default is to use whatever Python Pipenv is installed under (i.e. `sys.executable`).
- **Ignore Virtualenv**: `PIPENV_IGNORE_VIRTUALENVS`
	- Set to disable automatically using an activated virtualenv over the current project’s own virtual environment.
- **Virtualenv Location**: `export WORKON_HOME=~/.venvs`
	- so you can tell pipenv to store your virtual environments wherever you want
- **Python site.USER_BASE**: `export PYTHONUSERBASE=/myappenv` [ --user](https://pip.pypa.io/en/stable/user_guide/#user-installs)
- **Python Virtual Wrapper**: `export VIRTUALENVWRAPPER_PYTHON=/usr/local/bin/python3`
- **Pipenv custom venv name:** `export PIPENV_CUSTOM_VENV_NAME` 
	- Tells Pipenv whether to name the venv something other than the default dir name.

###### [Pipenv virtual mapping caveat](https://pipenv.pypa.io/en/latest/installation.html#virtualenv-mapping-caveat)
- Pipenv integrates Pipfile, pip, and virtualenv into a single command-line tool, simplifying the management of Python project environments. By default, Pipenv stores virtual environments globally in a directory such as `~/.local/share/virtualenvs/`, with the name combining the project's root directory and a hash of its path (e.g., `my_project-a3de50`). This differs from traditional `virtualenv`, which creates virtual environments in the current working directory.
- Should you change your project’s path, you break such a default mapping and pipenv will no longer be able to find and to use the project’s virtualenv.
- If you must move or rename a directory managed by pipenv, run ‘pipenv –rm’ before renaming or moving your project directory. Then, after renaming or moving the directory run ‘pipenv install’ to recreate the virtualenv.
- Customize this behavior with `PIPENV_CUSTOM_VENV_NAME` environment variable.
- For a more project-specific setup, you can set the environment variable `PIPENV_VENV_IN_PROJECT=1` in your shell configuration file (e.g., `.env`, `.bashrc`, `.zshrc`). This option creates the virtual environment directly inside your project’s directory. 

---
#### example user install of pipenv - (Review)
[Managing python virtualenvs with pipenv](https://automationhacks.io/2020/07/12/how-to-manage-your-python-virtualenvs-with-pipenv/)
[Managing python virtualenvs with pipenv II](https://medium.com/test-automation-university/how-to-manage-your-python-virtualenvs-with-pipenv-f1220ded702e)
```sh
pip3 install --user pipenv
export WORKON_HOME=~/virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/local/bin/python3
cd test_project
pipenv --three
```

If you work with multiple python projects on your machine, it is a good practice to store the virtualenvs in a central directory which you can easily refer to later on.

To enable that, we need to add an environment variable `WORKON_HOME` in a shell configuration file of your choice (`.zshrc`, or `.bash_profile`)

> _ℹ️ In the absence of WORKON_HOME variable being set, pipenv would simply create the virtualenv in the current terminal dir_

```sh
export WORKON_HOME=~/virtualenvs  
export VIRTUALENVWRAPPER_PYTHON=/usr/local/bin/python3
```
Go to the project for which you want to create a virtualenv for, and then execute below command:
```sh
cd test_project   
pipenv --three
```

Executing the above command would give an output like below, notice it **creates a virtualenv** in `**WORKON_HOME**` directory with the name of the project followed by a string. ( `test_project-UdkZ3s0t`)

Open a shell with the already created virtualenv

```sh
% pipenv shell
```

```sh
Launching subshell in virtual environment…  
 . /Users/gaurav/virtualenvs/test_project-UdkZ3s0t/bin/activate  
➜  test_project  . /Users/gaurav/virtualenvs/test_project-UdkZ3s0t/bin/activate  
(test_project) ➜  test_project
```

Notice that pipenv is invoking `**/bin/activate**` inside our created virtualenv just as you manually would if you were not using a higher-level tool like `virtualenvwrapper`

---

## Using `pipenv` and `pip`

### Running `pipenv` with a Specific Python Interpreter

To run `pipenv` with a specific Python interpreter, use the `-m` flag. This ensures that `pipenv` is executed using the specified Python version:

```bash
python3 -m pipenv
```

- **Why This Works**: The `-m` flag runs a library module as a script within the context of the given Python interpreter, making sure that the correct version is used.
  - [More Information](https://stackoverflow.com/questions/46391721/pipenv-command-not-found/64381102#64381102)
  - [Explanation of `-m` Option](https://stackoverflow.com/a/50620884)

#### Aliasing `pipenv` for Convenience

To simplify running `pipenv` with a specific Python version, create an alias:

```bash
alias pipenv='python3 -m pipenv'
```

- **Reference**: [Stack Overflow](https://stackoverflow.com/a/64381102)

### `python3 -m pip install` vs. `pip3 install`

Both commands are used to install Python packages but differ in their execution:

- **`python3 -m pip install`**: Executes the `pip` module as a script using the Python interpreter specified (`python3`). This method ensures that packages are installed for the Python version you are explicitly working with.
  - [Difference Explained](https://stackoverflow.com/questions/25749621/whats-the-difference-between-pip-install-and-python-m-pip-install?noredirect=1&lq=1)
  - [Detailed Explanation](https://stackoverflow.com/a/77153759/14436643)

- **`pip3 install`**: Directly invokes the `pip` executable. This can be convenient but may not always align with the Python version used by `python3`, particularly if multiple versions or environments exist. The executable `pip3` might not match the intended Python interpreter if the `PATH` is misconfigured.

**Why `python3 -m pip` is Preferred**:
- **Precision**: Ensures that `pip` is used with the exact Python interpreter intended, which is especially useful in virtual environments.
- **Avoiding Confusion**: Directly running `pip` can lead to discrepancies if multiple Python versions are installed, as each Python installation may have its own version of `pip`.

  - **Path Checking**: Use these commands to verify the paths of `python` and `pip`:
    ```bash
    % which python
    % which pip
    ```
  - **Virtual Environments**: When activated, a virtual environment will set up paths for `pip` accordingly.

**Key Takeaways**:
- Using `python3 -m pip` is a more explicit and reliable method to ensure that the correct `pip` version is used with the corresponding Python interpreter.
- Direct use of `pip3` may not always match the expected Python version due to `PATH` issues or multiple installations.

---
### Python with SSL and Homebrew

- **Install OpenSSL**: [Stack Overflow on SSL](https://stackoverflow.com/a/57068115)
- Apple is using their own TLS Library, and OpenSSL is deprecated, to get a more up to date version to increase security: [gist](https://gist.github.com/Kuret/d21ddcd94c23df07571924f77d080569?permalink_comment_id=3100475)
- [PKG_CONFIG_PATH](https://stackoverflow.com/a/52997074)
  ```bash
  brew install openssl
  export PATH="/usr/local/opt/openssl@3/bin:${PATH}"
  #suggested by brew
  export LDFLAGS="-L/usr/local/opt/openssl@3/lib"
  export CPPFLAGS="-I/usr/local/opt/openssl@3/include"
  export PKG_CONFIG_PATH="/usr/local/opt/openssl@3/lib/pkgconfig"
  ```
- **pyenv and OpenSSL**: [pyenv OpenSSL Issue](https://github.com/pyenv/pyenv/issues/1624)

---
### Python Environment Libraries and Package Managers

#### Articles and Guides

**1. [Why Pipenv Over venv for Python Projects](https://medium.com/analytics-vidhya/why-pipenv-over-venv-for-python-projects-a51fb6e4f31e)**
   - **Description**: This Medium article explains the benefits of using Pipenv compared to the built-in `venv` module. It highlights how Pipenv simplifies dependency management by automatically creating and updating `Pipfile` and `Pipfile.lock`, ensuring a consistent environment across setups. The article covers Pipenv's features such as deterministic builds and improved package management.

**2. [Difference Between venv, pyvenv, pyenv, virtualenv, and virtualenvwrapper](https://stackoverflow.com/questions/41573587/what-is-the-difference-between-venv-pyvenv-pyenv-virtualenv-virtualenvwrappe)**
   - **Description**: This Stack Overflow thread provides a comparison of tools for creating and managing Python virtual environments. It clarifies the distinctions between `venv`, `pyvenv`, `pyenv`, `virtualenv`, and `virtualenvwrapper`, helping users choose the best tool for their needs.

**3. [The Right Way to Set Up Python on macOS](https://devsintheshed.com/the-right-way-to-setup-python-on-macos/)**
   - **Description**: A comprehensive guide to setting up Python on macOS, including instructions for installing Python with Homebrew, managing multiple Python versions, and configuring virtual environments. This guide aims to help developers create a robust Python development environment on macOS.
   - `pyenv`: use multiple versions of Python locally, do not override the default/global version of Python

**4. [Apple Stack Exchange - Python Installation and PATH Configuration](https://apple.stackexchange.com/a/381658)**
   - **Description**: Discusses installing Python on macOS and configuring the PATH environment variable. It includes instructions on managing multiple Python versions and setting up symbolic links to ensure the correct Python interpreter is used.
   - `pyenv`: You can set a specific Python version to be "global" (i.e. default everywhere) and/or "local" (i.e. using that version in a specific directory only).

**5. [pyenv Homebrew Setup](https://github.com/pyenv/pyenv#homebrew-on-macos)**
   - **Description**: The official `pyenv` GitHub page provides instructions for installing and using `pyenv` with Homebrew on macOS. It includes steps for managing different Python versions and configuring `pyenv` for a smooth development experience.

---

#### Tools and Recommendations

**1. [Tool Recommendations for Packaging Python Projects](https://packaging.python.org/en/latest/guides/tool-recommendations/)**
   - **Virtual Environments**: 
     - **`virtualenv`**: A PyPA project tool for creating virtual environments.
     - **`venv`**: A module in the Python standard library, with fewer features than `virtualenv`.

   - **Package Installers**:
     - **`pip`**: The standard tool for installing packages from PyPI, with recommendations for [secure installs](https://pip.pypa.io/en/latest/topics/secure-installs/). Included by default in most Python installations through `ensurepip`.
     - **`pipx`**: Installs Python applications into dedicated virtual environments to avoid conflicts between dependencies. See [[#Pip vs. Pipx]]

   - **Workflow Tools**:
     - [flit](https://packaging.python.org/en/latest/key_projects/#flit)
     - [hatch](https://packaging.python.org/en/latest/key_projects/#hatch)
     - [nox](https://nox.thea.codes/en/latest/index.html)
     - [pdm](https://packaging.python.org/en/latest/key_projects/#pdm)
     - [Pipenv](https://packaging.python.org/en/latest/key_projects/#pipenv)
     - [poetry](https://packaging.python.org/en/latest/key_projects/#poetry)
     - [tox](https://tox.wiki/en/latest/index.html)

   - **Lock Files**:
     - **`pip-tools`** and **`Pipenv`**: Tools for creating lock files that ensure reproducibility by specifying exact package versions.
     - [Stack Overflow Discussion on `Pipfile` and `Pipfile.lock`](https://stackoverflow.com/questions/46330327/how-are-pipfile-and-pipfile-lock-used) explains how Pipenv uses these files.

   - **Packaging Python Applications**:
     - [Python Packaging Tutorial](https://packaging.python.org/en/latest/tutorials/packaging-projects/): Replaces `setup.py` and `setup.cfg` with `pyproject.toml`.
     - [Stack Overflow Discussion on `pyproject.toml`](https://stackoverflow.com/a/72473307/14436643) supported by various PEPs like [660](https://peps.python.org/pep-0660/) and [621](https://peps.python.org/pep-0621/), which is slowly gaining support by `setuptools`, `flit`, `hatch`

**2. [Pipx Comparison to Other Tools](https://pipx.pypa.io/latest/comparisons/)**
   - **Description**: A comparison of `pipx` with other tools for managing Python applications and environments.

**3. [direnv](https://direnv.net/)**
   - **Description**: Automatically activates environments when `cd` into a directory containing a `.env` file.
   - **Installation on macOS**: `$ brew install direnv`

**4. [pyenv](https://github.com/pyenv/pyenv)**
   - **Description**: `pyenv` allows for managing multiple Python versions locally without overriding the global version.


---

#### Pip vs. Pipx

**When to Use `pipx` vs `pip` Inside Virtual Environments**

- **`pipx`**:
  - **Use Case**: Recommended for installing command-line tools.
  - **Advantages**: Simplifies the installation and management of command-line tools. Automatically creates a virtual environment for each tool, isolating it from the rest of the system.
  - **Example Installation**:
    ```sh
    % pipx install my-cli-tool
    ```

- **`pip`**:
  - **Use Case**: Ideal for installing libraries or dependencies.
  - **Advantages**: Provides fine-grained control over package installations and dependencies. Used within a manually created virtual environment for isolation.
  - **Example Installation**:
    ```sh
    % python -m venv my-env
    % source my-env/bin/activate
    % pip install my-package
    ```

**Summary**:
- `pipx` is tailored for command-line tools and automates virtual environment management for each tool.
- `pip` is suited for libraries and dependencies, offering more control within manually created virtual environments.
- Both tools provide isolation but are optimized for different use cases.