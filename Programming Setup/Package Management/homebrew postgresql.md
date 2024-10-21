
---
# PostgreSQL Installation and Management with Homebrew

## Summary
- **PostgreSQL Version:** 15.4
- **Files:** 3,698 files, 60.6MB

## Cleanup
To disable automatic cleanup, set:
```sh
HOMEBREW_NO_INSTALL_CLEANUP=1
```
To hide environment hints, set:
```sh
HOMEBREW_NO_ENV_HINTS=1
```
For more information, see `man brew`.

## Caveats
- **Default Database Cluster Initialization:**
  ```sh
  initdb --locale=C -E UTF-8 /usr/local/var/postgresql@15
  ```
  For details, visit: [PostgreSQL InitDB Documentation](https://www.postgresql.org/docs/15/app-initdb.html)

- **Keg-Only:**
  `postgresql@15` is keg-only, meaning it was not symlinked into `/usr/local` due to it being an alternate version.

- **Updating PATH:**
  To prioritize `postgresql@15` in your PATH, add:
  ```sh
  echo 'export PATH="/usr/local/opt/postgresql@15/bin:$PATH"' >> ~/.zshrc
  ```

- **Compiler Flags:**
  Set the following for compilers:
  ```sh
  export LDFLAGS="-L/usr/local/opt/postgresql@15/lib"
  export CPPFLAGS="-I/usr/local/opt/postgresql@15/include"
  ```

- **Pkg-Config Path:**
  Set the following for `pkg-config`:
  ```sh
  export PKG_CONFIG_PATH="/usr/local/opt/postgresql@15/lib/pkgconfig"
  ```

- **Starting PostgreSQL:**
  To start PostgreSQL now and ensure it restarts at login:
  ```sh
  brew services start postgresql@15
  ```
  Alternatively, start PostgreSQL manually:
  ```sh
  LC_ALL="C" /usr/local/opt/postgresql@15/bin/postgres -D /usr/local/var/postgresql@15
  ```

## Installation Steps

1. **Install Homebrew and PostgreSQL:**
   ```sh
   brew install postgresql@15
   ```

2. **Initialize Database Cluster:**
   ```sh
   initdb /usr/local/var/postgresql@15
   ```
   - Ownership: User "meuthu" must own the server process.
   - Locale: "en_US.UTF-8"
   - Default Encoding: "UTF8"
   - Text Search Configuration: "english"
   - Data Page Checksums: Disabled

3. **Start Database Server:**
   ```sh
   pg_ctl -D /usr/local/var/postgresql@15 -l logfile start
   ```

4. **Create User:**
   ```sh
   /usr/local/Cellar/postgresql@15/<version>/bin/createuser -s postgres
   ```
   Or use the latest version:
   ```sh
   /usr/local/opt/postgresql/bin/createuser -s postgres
   ```

## Managing PostgreSQL

- **Start PostgreSQL Server Manually:**
  ```sh
  pg_ctl -D /usr/local/var/postgresql@15 start
  ```
  Or start via Homebrew services:
  ```sh
  brew services start postgresql@15
  ```

- **Start PostgreSQL at Startup:**
  ```sh
  mkdir -p ~/Library/LaunchAgents
  ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents
  launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
  ```

## Using PostgreSQL

- **Activate User:**
  ```sh
  psql -U postgres -h localhost
  ```

- **Create Database:**
  ```sh
  create database blog;
  ```

- **List Databases:**
  ```sh
  \l
  ```

- **Switch to Database:**
  ```sh
  \c blog
  ```

- **Show Tables:**
  ```sh
  \dt
  ```

- **Stop PostgreSQL Service:**
  ```sh
  brew services stop postgresql@15
  ```

---
