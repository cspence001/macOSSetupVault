
* [[#.gitconfig example]]
* [[#.ssh/config example]]
* [[#.gitignore example]]
* [[#github profile README setup]]

#### .gitconfig example

```sh
[user]
	name = cspence001
	email = 69818722+cspence001@users.noreply.github.com
	signingkey = [key here]

[core]
	excludesfile = /Users/meuthu/.gitignore

[commit]
	gpgsign = true

# Git URL Rewrite to use SSH over port 443
[url "ssh://git@github.com:443/"]
  insteadOf = git@github.com:
[url "ssh://git@github.com:443/"]
  insteadOf = https://github.com/

[color]
	# Use colors in Git commands that are capable of colored output when
	# outputting to the terminal. (This is the default setting in Git ≥ 1.8.4.)
	ui = auto

[color "branch"]
	current = yellow reverse
	local = yellow
	remote = green

[color "diff"]
	meta = yellow bold
	frag = magenta bold # line info
	old = red # deletions
	new = green # additions

[color "status"]
	added = yellow
	changed = green
	untracked = cyan
```

#### .ssh/config example

```sh
Host github.com
  Hostname ssh.github.com
  Port 443
  User git
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
  IdentitiesOnly yes
  UserKnownHostsFile ~/.ssh/known_hosts
```

#### .gitignore example
```sh
# [1]https://gist.github.com/octocat/9257657
# [2]https://raw.githubusercontent.com/github/gitignore/main/Python.gitignore

# [1]##############
# Compiled source #
###################
*.com
*.class
*.dll
*.exe
*.o
*.so

# Packages #
############
# it's better to unpack these files and commit the raw source
# git has its own built in compression methods
*.7z
*.dmg
*.gz
*.iso
*.jar
*.rar
*.tar
*.zip

# Logs and databases #
######################
*.log
*.sql
*.sqlite

# OS generated files #
######################
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

# VS
.vscode

# [2] ##############
# Byte-compiled / optimized / DLL files
__pycache__/
*.py[cod]
*$py.class

# Compiled Python files
*.pyc

# C extensions
*.so

# Distribution / packaging
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
share/python-wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST

# PyInstaller
#  Usually these files are written by a python script from a template
#  before PyInstaller builds the exe, so as to inject date/other infos into it.
*.manifest
*.spec

# Installer logs
pip-log.txt
pip-delete-this-directory.txt

# Unit test / coverage reports
htmlcov/
.tox/
.nox/
.coverage
.coverage.*
.cache
nosetests.xml
coverage.xml
*.cover
*.py,cover
.hypothesis/
.pytest_cache/
cover/

# Translations
*.mo
*.pot

# Django stuff:
*.log
local_settings.py
db.sqlite3
db.sqlite3-journal

# Flask stuff:
instance/
.webassets-cache

# Scrapy stuff:
.scrapy

# Sphinx documentation
docs/_build/

# PyBuilder
.pybuilder/
target/

# Jupyter Notebook
.ipynb_checkpoints

# IPython
profile_default/
ipython_config.py

# pyenv
#   For a library or package, you might want to ignore these files since the code is
#   intended to run in multiple environments; otherwise, check them in:
# .python-version

# pipenv
#   According to pypa/pipenv#598, it is recommended to include Pipfile.lock in version control.
#   However, in case of collaboration, if having platform-specific dependencies or dependencies
#   having no cross-platform support, pipenv may install dependencies that don't work, or not
#   install all needed dependencies.
#Pipfile.lock

# poetry
#   Similar to Pipfile.lock, it is generally recommended to include poetry.lock in version control.
#   This is especially recommended for binary packages to ensure reproducibility, and is more
#   commonly ignored for libraries.
#   https://python-poetry.org/docs/basic-usage/#commit-your-poetrylock-file-to-version-control
#poetry.lock

# pdm
#   Similar to Pipfile.lock, it is generally recommended to include pdm.lock in version control.
#pdm.lock
#   pdm stores project-wide configurations in .pdm.toml, but it is recommended to not include it
#   in version control.
#   https://pdm.fming.dev/#use-with-ide
.pdm.toml

# PEP 582; used by e.g. github.com/David-OConnor/pyflow and github.com/pdm-project/pdm
__pypackages__/

# Celery stuff
celerybeat-schedule
celerybeat.pid

# SageMath parsed files
*.sage.py

# Environments
.env
.venv
env/
venv/
ENV/
env.bak/
venv.bak/

# Spyder project settings
.spyderproject
.spyproject

# Rope project settings
.ropeproject

# mkdocs documentation
/site

# mypy
.mypy_cache/
.dmypy.json
dmypy.json

# Pyre type checker
.pyre/

# pytype static type analyzer
.pytype/

# Cython debug symbols
cython_debug/

# PyCharm
#  JetBrains specific template is maintained in a separate JetBrains.gitignore that can
#  be found at https://github.com/github/gitignore/blob/main/Global/JetBrains.gitignore
#  and can be added to the global gitignore or merged into this file.  For a more nuclear
#  option (not recommended) you can uncomment the following to ignore the entire idea folder.
#.idea/

```


##### More .gitignore templates
**vscode**
https://github.com/github/gitignore/blob/main/Global/VisualStudioCode.gitignore
https://github.com/github/gitignore/blob/main/VisualStudio.gitignore
**macOS**
https://github.com/github/gitignore/blob/main/Global/macOS.gitignore
**node**
https://github.com/github/gitignore/blob/main/Node.gitignore


---

#### github profile README setup

https://www.sitepoint.com/github-profile-readme/
https://github.com/anuraghazra/github-readme-stats
https://dev.to/anuraghazra/dynamically-generated-github-stats-for-your-profile-readme-o4g
creating vercel fork of github-readme-stats
https://www.youtube.com/watch?v=n6d4KHSKqGk&t=107s

###### stats

https://github-readme-streak-stats.herokuapp.com/?user=your-github-username
customization
https://github-readme-streak-stats.herokuapp.com/demo/

dark-smoky
[![GitHub Streak](http://github-readme-streak-stats.herokuapp.com?user=cspence001&theme=dark-smoky&background=000000)](https://git.io/streak-stats)
```sh
### :fire: My Stats :
[![GitHub Streak](http://github-readme-streak-stats.herokuapp.com?user=cspence001&theme=dark-smoky&background=000000)](https://git.io/streak-stats)
```

black-ice, exclude Sat,Sun
[![GitHub Streak](https://github-readme-streak-stats.herokuapp.com?user=cspence001&theme=black-ice&background=000000&hide_border=true&exclude_days=Sun%2CSat)](https://git.io/streak-stats)

```sh
[![GitHub Streak](https://github-readme-streak-stats.herokuapp.com?user=cspence001&theme=black-ice&background=000000&hide_border=true&exclude_days=Sun%2CSat)](https://git.io/streak-stats)
```

###### top languages

[![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=cspence001)](https://github.com/anuraghazra/github-readme-stats)
custom
[![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=cspence001&layout=compact&theme=vision-friendly-dark)](https://github.com/anuraghazra/github-readme-stats)
custom w vercel
[![Top Langs](https://github-readme-stats-cspence001s-projects.vercel.app/api/top-langs/?username=cspence001&layout=compact&theme=vision-friendly-dark&count_private=true)
exclude jupyter
[![Top Langs](https://github-readme-stats-cspence001s-projects.vercel.app/api/top-langs/?username=cspence001&layout=compact&theme=vision-friendly-dark&count_private=true&hide=jupyter%20notebook)](https://github.com/anuraghazra/github-readme-stats)

```sh
[![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=cspence001)](https://github.com/anuraghazra/github-readme-stats)
custom
[![Top Langs](https://github-readme-stats-cspence001s-projects.vercel.app/api/top-langs/?username=cspence001&layout=compact&theme=vision-friendly-dark&count_private=true)](https://github.com/anuraghazra/github-readme-stats)
[![Top Langs](https://github-readme-stats-cspence001s-projects.vercel.app/api/top-langs/?username=cspence001&layout=compact&theme=vision-friendly-dark&count_private=true&hide=jupyter%20notebook)](https://github.com/anuraghazra/github-readme-stats)
```


###### other stats

<a href="https://github.com/anuraghazra/github-readme-stats">
    <img src="https://github-readme-stats-cspence001s-projects.vercel.app/api/?username=cspence001&layout=donut&show_icons=true&theme=material-palenight&hide_border=true&icon_color=58A6FF&bg_color=30,e96443,904e95&title_color=fff&text_color=fff&count_private=true" width=39.5% />
</a>
```sh
<a href="https://github.com/anuraghazra/github-readme-stats">
    <img src="https://github-readme-stats-cspence001s-projects.vercel.app/api/?username=cspence001&layout=donut&show_icons=true&theme=material-palenight&hide_border=true&icon_color=58A6FF&bg_color=30,e96443,904e95&title_color=fff&text_color=fff&count_private=true" width=39.5% />
</a>

```

