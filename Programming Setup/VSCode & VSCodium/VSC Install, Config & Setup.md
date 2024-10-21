
---

## VSCode Notes

### 1. Extension Installation and Interpreter Setup

- **Python Extension Install Location / Interpreter / VirtualEnv**
  - [Stack Overflow: Python Extension and Virtual Environment](https://stackoverflow.com/q/58775312/14436643)
  - Note: Python extension uses `-m` to ensure tools use the specific version your project relies on.

- **Installed Extensions**
  - [AWS Toolkit for VSCode](https://marketplace.visualstudio.com/items?itemName=amazonwebservices.aws-toolkit-vscode)
  - [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  - [Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance)
  - [Jupyter](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter)
  - [Jupyter Keymap](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter-keymap)
  - [Jupyter Renderers](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter-renderers)
  - [Jupyter Cell Tags](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-jupyter-cell-tags)
  - [Jupyter Slideshow](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-jupyter-slideshow)

### 2. Interpreter Paths

- **Current Interpreter Paths**
  - Python 3.9.6 (64-bit): `/usr/bin/python3`
  - Python 3.7.9 (64-bit): `/usr/local/bin/python3`

- **Pip Version**
  ```bash
    pip3 --version
    # pip 23.2.1 from /usr/local/lib/python3.11/site-packages/pip (python 3.11)
    ```

- **Default Interpreter Path in VSCode Settings**
```json
"python.defaultInterpreterPath": "/Library/Frameworks/Python.framework/Versions/3.7/lib/python3"
    ```

### 3. VSCodium File Permissions and Configuration

- **Permissions Issue**
   ```bash
    codium --verbose
    [56568:0425/211043.079940:FATAL:credentials.cc(127)] Check failed: . : Permission denied (13)
    ```

- **Fix Permissions**
  ```bash
    chmod -R u+rwX ~/.config/VSCodium
    chown -R andrew:andrew ~/.config/VSCodium
    ```

- **VSCodium Installation (Binary Release)**
  - [VSCodium Releases](https://github.com/VSCodium/vscodium/releases) (VSCode without telemetry)

### 4. Snippets and Emmet

- **User-Defined Snippets**
  - [Creating and Using Snippets](https://code.visualstudio.com/docs/editor/userdefinedsnippets)
  - [YouTube: React Snippets and Emmet](https://www.youtube.com/watch?v=j942wKiXFu8)
  - Example Snippets File: `snippets.json.code-snippets` in [[VSC settings json#snippets]]

### 5. GitHub Integration

- **Configuration**
  - Set Global Git Configurations:
    ```bash
    git config --global user.name "cspence001"
    git config --global user.email "69818722+cspence001@users.noreply.github.com"
    ```
  - [VSCode Git Tutorial](https://courses.cs.washington.edu/courses/cse154/20au/resources/assets/vscode-git-tutorial/macosx/index.html)
  - [Stack Overflow: GitHub Integration Issues](https://stackoverflow.com/a/44099011)

### 6. AWS Toolkit for VSCode

- **Setup and Configuration**
  - [AWS Toolkit User Guide](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/setup-toolkit.html)
  - [Setting Up Project Credentials](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/setup-credentials.html)

### 7. VSCode Settings

- **Disable Telemetry**
  - Turn off telemetry and AWS Code Whisperer settings:
    ```json
    "telemetry.enableTelemetry": false
    "aws.telemetry.enableTelemetry": false
    "aws.codeWhisperer.enableCodeWhisperer": false
    ```

### 8. Python Environments and Interpreter

- **Python Environments Documentation**
  - [Configuring Python Environments](https://code.visualstudio.com/docs/python/environments)

- **Integrated Terminal Configuration**
  - [Integrated Terminal Shell Integration](https://code.visualstudio.com/docs/terminal/shell-integration)
  - [Stack Overflow: Terminal Environment Variables](https://stackoverflow.com/a/65079227/14436643)
    ```json
    "terminal.integrated.env.linux": {
      "PYTHONPATH": ""
    }
    ```

- **Profile Templates**
  - [Python Profile Template](https://code.visualstudio.com/docs/editor/profiles#_python-profile-template)

- **Python Settings Reference**
  - [Python Extension Settings](https://code.visualstudio.com/docs/python/settings-reference)

### 9. VSCode Environments and Interpreter

- **Imports Issue**
  - [Stack Overflow: Import Issues in .venv](https://stackoverflow.com/questions/65933570/import-boto3-could-not-be-resolved-python-vs-code)

- **Flask Setup**
  - [VSCode Flask Tutorial](https://code.visualstudio.com/docs/python/tutorial-flask)
  - Install Flask:
    ```bash
    python3 -m pip install flask
    pip3 install flask --upgrade
    python3 -m flask run
    ```

- **Jupyter Setup**
  - [VSCode Jupyter Notebooks Documentation](https://code.visualstudio.com/docs/datascience/jupyter-notebooks)
  - Install Jupyter:
    ```bash
    python3 -m pip install jupyter
    ```

To work with Python in Jupyter Notebooks, you must activate an Anaconda environment in VS Code, or another Python environment in which you've installed the [Jupyter package](https://pypi.org/project/jupyter/). To select an environment, use the **Python: Select Interpreter** command from the Command Palette (⇧⌘P).

---

