# Lower level: virtualenv[](https://docs.python-guide.org/dev/virtualenvs/#lower-level-virtualenv "Permalink to this headline")

[virtualenv](http://pypi.org/project/virtualenv) is a tool to create isolated Python environments. virtualenv creates a folder which contains all the necessary executables to use the packages that a Python project would need.

It can be used standalone, in place of Pipenv.
Install virtualenv via pip:
`$ pip install virtualenv`
Test your installation:
`$ virtualenv --version`
### Basic Usage[](https://docs.python-guide.org/dev/virtualenvs/#basic-usage "Permalink to this headline")

1. Create a virtual environment for a project:
`$ cd project_folder`
`$ virtualenv venv`

`virtualenv venv` will create a folder in the current directory which will contain the Python executable files, and a copy of the `pip` library which you can use to install other packages. The name of the virtual environment (in this case, it was `venv`) can be anything; omitting the name will place the files in the current directory instead.

Note
	‘venv’ is the general convention used globally. As it is readily available in ignore files (eg: .gitignore’)

This creates a copy of Python in whichever directory you ran the command in, placing it in a folder named `venv`.

You can also use the Python interpreter of your choice (like `python2.7`).
`$ virtualenv -p /usr/bin/python2.7 venv`
or change the interpreter globally with an env variable in `~/.bashrc`:
`$ export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python2.7`
2. To begin using the virtual environment, it needs to be activated:
`$ source venv/bin/activate`

The name of the current virtual environment will now appear on the left of the prompt (e.g. `(venv)Your-Computer:project_folder UserName$`) to let you know that it’s active. From now on, any package that you install using pip will be placed in the `venv` folder, isolated from the global Python installation.

Install packages using the `pip` command:
`$ pip install requests`
3. If you are done working in the virtual environment for the moment, you can deactivate it:
`$ deactivate`
This puts you back to the system’s default Python interpreter with all its installed libraries.