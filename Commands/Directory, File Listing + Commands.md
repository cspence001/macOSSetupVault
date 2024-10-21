
- [[#Directory / File Listings]] 
- [[#Directory Size]] 
- [[#Difference Between 2 Files]] 
- [[#Difference Between 2 Directories]] 
- [[#File Searching]] `find`, `mdfind`, `mfind`, `grep`, `ack`, `ripgrep`
- [[#File Copying]] Copying Files and Directories using `ditto`
- [[#File Downloading]]

---

## Directory / File Listings

* Full listing of directories and subdirectories:
```sh
find ~/Library/Application\ Support/SyncServices/Local -print
```
* Include File Sizes and other info
```sh
ls -lhR ~/Library/Application\ Support/SyncServices/Local
```
- `-l`: Long listing format.
- `-h`: Human-readable sizes.
- `-R`: Recursive listing through subdirectories.
- **List directory files recursively with path header:**
  ```sh
ls -R -A data/ > data/alt_data/directorylistings/local_backup_head.txt
ls -R -A mapdata_fips_aggregation/
  ```
  
* Using `tree`:
```sh
brew install tree
tree -h ~/Library/Application\ Support/SyncServices/Local
```

* **List all files and their sizes:**
```sh
find ~/Library/Application\ Support/SyncServices/Local -type f -exec du -h {} +
```

- **List all objects in a directory ignoring dotfiles (with timestamp and size):**
  ```sh
find . \( ! -regex '.*/\..*' \) -type d -exec stat -f "%t%Sm %N" {} \;
  ```

- **List directory files recursively with full paths:**
  ```sh
find . -type f > data/alt_data/directorylistings/local_backup_dirpath.txt
  ```

- **List all files in a directory ignoring dotfiles (with timestamp):**
  ```sh
find . \( ! -regex '.*/\..*' \) -type f -exec stat -f "%t%Sm %N" {} \;
  ```

- **List all files in a directory ignoring dotfiles (with timestamp and size):**
  ```sh
find . \( ! -regex '.*/\..*' \) -type f -exec stat -f '%t%Sm %z %t%N' '{}' +;
find . \( ! -regex '.*/\..*' \) -type f -exec stat -f '%t%Sm %z %t%N' '{}' \;
  ```

- **List all files in a directory ignoring dotfiles (with timestamp and size), sort date:**
```sh
find . \( ! -regex '.*/\..*' \) -type f -exec stat -f '%m %z %N' '{}' + | sort -n
# + format date
find . \( ! -regex '.*/\..*' \) -type f -exec stat -f '%m %z %N' '{}' + | sort -n | awk '{ cmd = "date -r " $1; cmd | getline d; close(cmd); printf "%s %s %s\n", d, $2, $3 }'
```

- **List directory files recursively with full paths, ignore dotfiles, and sort by the second key:**
  ```sh
find . \( ! -regex '.*/\..*' \) -type f | sort -n -k2 | cut -f 1 > backup_dir.txt
  ```
  
  - **List directory files recursively with full paths, ignore dotfiles & image files:**
  ```sh
find /path/to/directory -type f ! -name "*.png" ! -name "*.img" ! -name ".*"
  ```
  - **List directory files recursively with full paths, ignore dotfiles & dot directories:
```sh
find ReactCodeVault -type f ! -name ".*" ! -path "*/.*" -print
```


### Mapping and Aliases

- **Intuitive map function example (list directories containing a certain file):**
  ```sh
  find . -name .gitattributes | map dirname
  ```

- **Alias for mapping:**
  ```sh
  alias map="xargs -n1"
  ```


---

## Directory Size

- **Basic Size of a Directory:**
  ```bash
  du -hs /path/to/directory
  ```
  - `-s` (summary) gives you only the total size of the directory.
  - `-h` (human-readable) formats the size in a human-readable way (e.g., KB, MB, GB).
```sh
  #size of each Brave Profile directory
  du -hs ~/Library/Application\ Support/BraveSoftware/Brave-Browser/Profile\ *
```
- **Size of Each Subdirectory:**
  ```bash
  du -h /path/to/directory
  ```
  - This will list the size of the directory and each subdirectory within it.

- **Total Size of All Subdirectories:**
  ```bash
  du -sh /path/to/directory/*
  ```
  - This will give you the size of each item (file or directory) inside `/path/to/directory`.

- **Size of a Directory with a Depth Limit:**
  ```bash
  du -h --max-depth=1 /path/to/directory
  ```
  - This shows the sizes of directories at the first level of depth (i.e., immediate subdirectories).

- **Displays the top 10 largest files and directories in current directory, sorted by size:**
  ```bash
  du -hsx * | sort -rh | head -10 
  ```

---
### File Count

**Using `find` to Count Files**
To count all files in a directory and its subdirectories, use:
`find /path/to/directory -type f | wc -l`

**Count Non-Empty Files (Non-Zero Bytes)**
To count all non-empty files (files greater than 0 bytes) in a directory and its subdirectories, use:
`find /path/to/directory -type f -size +0c | wc -l`

**Count Empty Files (Zero Bytes)**
To count all empty files (files that are exactly 0 bytes) in a directory and its subdirectories, use:
`find /path/to/directory -type f -size 0c | wc -l`

---

## Difference Between 2 Files

- **`comm` - Compare two sorted files line by line:**
  ```sh
  comm [OPTION]... FILE1 FILE2
  ```
  **Options:**
  - `-1` Suppress lines unique to FILE1
  - `-2` Suppress lines unique to FILE2
  - `-3` Suppress lines that appear in both files

  **Example:**
  ```sh
  comm -2 -3 file1 file2 > file3
  ```

- **`diff` - Compare files and suppress common lines:**
  ```sh
  diff -a --suppress-common-lines -y a.txt b.txt > c.txt
  ```

---

## Difference Between 2 Directories

- **Brief output (just note differences):**
  ```sh
  diff -rq -x .DS_Store $1 $2
  ```

- **Compare files within directories:**
  ```sh
  diff -r -x .DS_Store $1 $2
  ```

- **Additional `diff` options:**
  - `-a` Treat all files as text and compare line-by-line.
  - `-b` Ignore changes in amount of white space.
  - `-B` Ignore changes that just insert or delete blank lines.
  - `-w` Ignore white space when comparing lines.
  - `-c` Use context output format.
  - `-C lines` Use context output format with specified number of context lines.
  - `-u, --unified` Display differences in unified format with context lines.

  For more options, refer to [SS64 diff documentation](https://ss64.com/mac/diff.html).

- **Adding color to diff output (in terminal only):**
  `colordiff.bash`  
  [colordiff.bash](https://github.com/ildar-shaimordanov/colordiff/blob/master/colordiff.bash)  
  ```bash
  # /etc/bashrc
  # Check and use colordiff if available
  if [ -x ~/bin/colordiff.bash ]; then
      alias diff='~/bin/colordiff.bash'
  fi
  ```

---
## File Searching

#### `find` Commands

  - **Find directories named 'Caches':**
    ```sh
    sudo find ~/Library/ -type d -name 'Caches/*' -ls -delete 2> /dev/null
    ```

  - **Alias for quick file search:**
    ```sh
    alias qfind="find . -name "
    ```

  - **Functions for finding files:**
    ```sh
    ff () { /usr/bin/find . -name "$@" ; }      # Find file under the current directory
    ffs () { /usr/bin/find . -name "$@"'*' ; }  # Find file whose name starts with a given string
    ffe () { /usr/bin/find . -name '*'"$@" ; }  # Find file whose name ends with a given string
    ```

  - **Example searches:**
    ```sh
    find . -name '*.csv'
    find . -name '*.db'
    ```
**Scope**: This command searches within the current directory (`.`) and all its subdirectories.
**Pattern**: It looks for files with names ending in `.db` (a common extension for database files).

```sh
	find / -name "*4349612081192991917*"
```
**Scope**: This command searches the entire filesystem, starting from the root directory (`/`).
**Pattern**: It looks for files or directories whose names contain the string

```sh
	find . | grep "text goes here"
```
**Scope**: Searches within the names or paths, not the contents.
**Purpose**: To filter the list of all files and directories by their names (or paths) for those that contain `"text goes here"`.

Find and delete all .mp4 and .gif files from directory, recursively:
```sh
# view files to be deleted
$ find . -type f \( -iname "*.mp4" -o -iname "*.gif" \)
# delete
$ find . -type f \( -iname "*.mp4" -o -iname "*.gif" \) -delete
```
#### `mdfind` and `mfind`

  - **Search for files containing the word 'password':**
    ```sh
    mdfind password
    mfind -name password
    ```

#### `grep` Commands

* For searching text within files
	* `-r`: Recursively search through directories and subdirectories.
	- `-R`: Recursively search through directories and subdirectories, include symbolic links.
	- `-n`: Show the line number where the text pattern is found.
	- `-s`: Suppress error messages about nonexistent or unreadable files.
	- `-l`: List the names of files containing the text pattern, rather than show the matching lines.
	- `-I`: Ignores binary files.
	- `-i`: Ignores case sensitivity

  - **Search for text pattern in files with line numbers:**
```sh
    grep -nr "text pattern" [PATH OF DIRECTORY] -s
```

  - **Example:**
    ```sh
    grep -nr "smartcard" ./ -s #current directory 
    grep -RnslI "text goes here" 
    ```

* **Example using exclude files using `grep`**:
```sh
	grep -nr --exclude='README.*' "gitflic.ru" ./ -s
```

* **`find` + `grep`**
```sh
	find . -type f -exec grep -H "text goes here" {} +
```
Explicitly searches for text within files found by `find`, and executes `grep` on multiple files at once, showing filenames and matching lines. You can customize `find` to include or exclude certain types of files or directories.

* **Example to exclude files using `find` + `grep`**:
```sh
	find . -type f ! -name 'README.*' -exec grep -nH "gitflic.ru" {} +
```

* **`grep` + `regex` patterns**
	* `-E`: This enables extended regex syntax, allowing the use of `{}` for repetition.

* **Example to look for recovery key within files in a directory:**
```sh
grep -rE '\b([0-9A-Z]{4}-){6}[0-9A-Z]{4}\b' /path/to/directory
```
Example Matches using the above regex:
- `658J-WYRH-W84P-PPBH-PADV-6QDY-CF8Z`
- `1234-ABCD-EFGH-IJKL-MNOP-QRST-UVWX`



#### `ack` Commands

  **[ack3](https://github.com/beyondgrep/ack3)**
  [ack CL](https://beyondgrep.com/documentation/)
  
* **Install `ack` executable (single-file version)**
```sh
sudo curl -L https://beyondgrep.com/ack-v3.7.0 -o /usr/local/bin/ack
```
Official documentation for `ack` suggests installing the executable in the `~/bin` directory, which isn't a formal directory on macOS, the equivalent is `/usr/local/bin`.

* **Set the executable permissions for the `ack` script:**
```sh
sudo chmod 0755 /usr/local/bin/ack
```

* **Verify installation and accessibility:**
```sh
ack --version
```

  - Alternatively, **Install `ack` via brew:**
    ```sh
    brew install ack
    ```
 
  - **Search for patterns:**
    ```sh
    ack 'url'
    ack 'email'
    ack 'RedirectUrl'
    ack 'clone'
    ```

  - **Find all JavaScript files using the module 'pancakes':**
    ```sh
    ack --js pancakes
    ```

  - **Find files not containing the word 'brew':**
    ```sh
    ack -L brew
    ```

#### ripgrep `rg`

[ripgrep](https://github.com/BurntSushi/ripgrep/tree/master) is a high-performance line-oriented search tool that recursively searches the current directory for a regex pattern. By default, ripgrep will respect gitignore rules and automatically skip hidden files/directories and binary files. (To disable all automatic filtering by default, use `rg -uuu`.)
[rg config file](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file
##### Steps to Download and Install `ripgrep` Binary

* **Download the `ripgrep` Binary for macOS**:
   You can download the pre-built binary from the [GitHub releases page for ripgrep](https://github.com/BurntSushi/ripgrep/releases). Look for the `.tar.gz` file suitable for macOS. For example, `ripgrep-<version>-x86_64-apple-darwin.tar.gz`.

* Use `curl` to download the file. Replace `<version>` with the actual version number:
   ```sh
   curl -L https://github.com/BurntSushi/ripgrep/releases/download/<version>/ripgrep-<version>-x86_64-apple-darwin.tar.gz -o ripgrep.tar.gz
   ```

* After downloading, extract the tarball:
   ```sh
   tar -xzf ripgrep.tar.gz
   ```
	**Note**: This will create a directory with the extracted files. You should see the `rg` binary inside this directory.
	
* Move the `rg` binary to a directory that's in your `PATH`, such as `/usr/local/bin`. If `/usr/local/bin` is not writable by your user, you might need to use `sudo`:
   ```sh
   sudo mv ripgrep-<version>-x86_64-apple-darwin/rg /usr/local/bin/
   ```
	**Note**: Replace `<version>` with the actual version number of the directory you extracted.
   
* Ensure the `rg` binary is executable:
   ```sh
   sudo chmod 755 /usr/local/bin/rg
   ```
* Check if `ripgrep` is correctly installed and accessible:
   ```sh
   rg --version
   ```
* Delete extracted files to clean-up space:
```sh
rm -rf ripgrep-<version>-x86_64-apple-darwin/
```

* Alternatively, install via brew:
```sh
   brew install ripgrep
```

---
## File Copying
#### `ditto`

> Copy files and directories. More information: [https://keith.github.io/xcode-man-pages/ditto.1.html](https://keith.github.io/xcode-man-pages/ditto.1.html).

- Overwrite contents of destination directory with contents of source directory:

`ditto path/to/source_directory path/to/destination_directory`

- Print a line to the Terminal window for every file that's being copied:

`ditto -V path/to/source_directory path/to/destination_directory`

- Copy a given file or directory, while retaining the original file permissions:

`ditto -rsrc path/to/source_directory path/to/destination_directory`

---
## File Downloading

- **Downloads a file from the URL and saves it to `~/Downloads` with the filename extracted from the URL.**  
  ```bash
  alias curldl="curl --output-dir \"~/Downloads\" \$1 -O -L"
  ```

- **Downloads a file from the URL and saves it to `~/Downloads` with the filename determined by the `Content-Disposition` header sent by the server.**  
  ```bash
  alias curldl2="curl --output-dir \"~/Downloads\" \$1 -O -J -L"
  ```
