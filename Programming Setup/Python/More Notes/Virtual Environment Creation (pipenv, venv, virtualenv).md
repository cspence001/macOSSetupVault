Processes for creating and managing virtual environments using `virtualenv`, `venv`, and `pipenv`

---

### Virtual Environment Creation and Management

#### `virtualenv`

1. **Installation:**
   - Install `virtualenv` using `pip`:
     ```bash
     $ pip install virtualenv
     ```
   - Test the installation:
     ```bash
     $ virtualenv --version
     ```

2. **Creating a Virtual Environment:**
   - Navigate to your project directory:
     ```bash
     $ cd project_folder
     ```
   - Create a virtual environment:
     ```bash
     $ virtualenv venv
     ```
   - The `virtualenv` command creates a directory named `venv` (or any name you choose) containing the Python executable files and a copy of `pip` for managing packages.

   - **Specifying Python Version:**
     - To use a specific Python interpreter:
       ```bash
       $ virtualenv -p /usr/bin/python2.7 venv
       ```
     - Or set it globally in `~/.bashrc`:
       ```bash
       $ export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python2.7
       ```

3. **Activating the Virtual Environment:**
   - Activate the environment:
     ```bash
     $ source venv/bin/activate
     ```
   - The prompt changes to show the name of the active environment (e.g., `(venv)`).

4. **Managing Packages:**
   - Install packages within the environment:
     ```bash
     $ pip install requests
     ```

5. **Deactivating the Virtual Environment:**
   - Deactivate the environment and return to the global Python installation:
     ```bash
     $ deactivate
     ```

#### Python’s Built-In `venv`

1. **Creating a Virtual Environment:**
   - Use the built-in `venv` module (available in Python 3.3+):
     ```bash
     $ python -m venv .venv
     ```
   - This creates a `.venv` directory containing the Python executable and `pip`.

2. **Activating the Virtual Environment:**
   - On Unix or macOS:
     ```bash
     $ source .venv/bin/activate
     ```

3. **Managing Packages:**
   - Install packages using `pip`:
     ```bash
     $ pip install package_name
     ```

4. **Deactivating the Virtual Environment:**
   - Deactivate the environment:
     ```bash
     $ deactivate
     ```

#### `pipenv`

1. **Setting Up `pipenv`:**
   - Ensure the environment variable is set to create the virtual environment in the project directory:
     ```bash
     % export PIPENV_VENV_IN_PROJECT=1
     ```

2. **Creating and Activating a `pipenv` Environment:**
   - Install dependencies and create a `Pipfile`:
     ```bash
     % pipenv install
     ```
   - Activate the `pipenv` shell:
     ```bash
     % pipenv shell
     ```

3. **Managing Dependencies:**
   - Install a specific package:
     ```bash
     % pipenv install boto3
     ```
   - Install development packages:
     ```bash
     % pipenv install --dev
     ```
   - Update a specific package:
     ```bash
     % pipenv update werkzeug
     ```
   - Install from a `requirements.txt` file:
     ```bash
     % pipenv install -r requirements.txt
     ```

4. **Manual Setup with `pipenv`:**
   - Create an empty `Pipfile`:
     ```bash
     % mkdir .venv
     % touch Pipfile
     ```
   - Set up the environment:
     ```bash
     % pipenv shell
     ```

5. **Replacing `.venv` with `pipenv`:**
   - Remove existing virtual environments and dependency files:
     ```bash
     % rm -rf .venv Pipfile requirements.txt
     ```
   - Set the environment variable if necessary:
     ```bash
     % export PIPENV_VENV_IN_PROJECT=1
     ```
   - Install dependencies with `pipenv`:
     ```bash
     % pipenv install
     % pipenv shell
     % pipenv install {proj dependencies}
     ```

### Key Differences

- **`virtualenv`**:
  - A tool to create isolated Python environments, allowing you to specify Python versions and manage environments manually.
  - Requires manual activation and deactivation.

- **`venv`**:
  - A built-in module in Python 3.3+ for creating virtual environments, offering similar functionality to `virtualenv` but integrated into Python.
  - Standard for Python projects needing virtual environments.

- **`pipenv`**:
  - Combines package management and virtual environment creation in one tool, using `Pipfile` and `Pipfile.lock` for dependency management.
  - Provides additional features such as dependency updates and environment setup in a project-specific way.

