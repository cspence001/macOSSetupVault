### TLDR Pages
https://tldr.inbrowser.app/pages/osx
text-based search interface of system shell commands and service descriptions in macOS.
### Internals

#### [apple-internals site](https://mroi.github.io/apple-internals/)
Information on apple system processes, text-based search interface 
#### [apple-internals repo](https://github.com/mroi/apple-internals)
This repository provides tools and information to help understand and analyze the internals of Apple’s operating system platforms. Information is collected in a text file and [presented on a website](https://mroi.github.io/apple-internals). A [Nix flake](https://nixos.wiki/wiki/Flakes) allows to build the following externally hosted tools:

[**acextract**](https://github.com/bartoszj/acextract)  
Unpacks asset catalogs to individual files.
[**dyld-shared-cache-util**](https://github.com/antons/dyld-shared-cache-big-sur)  
Extracts dynamic libraries from the dyld linker cache.
[**snapUtil**](https://github.com/ahl/apfs)  
Manages APFS snapshots.

The Makefile aggregates various kinds of information from the system in a SQLite database and checks the internals text file against this information. Collected details include:

- all file names of the installed macOS and the iOS, tvOS, and watchOS simulators
- linkages of binaries to libraries
- entitlements for all executables
- plain-text strings embedded in binaries
- launchd service descriptions and bundle Info.plist content
- lists of assets inside asset catalogs


### Parsing + Formats
#### [dtformats](https://github.com/libyal/dtformats/tree/main/documentation) 
collection of macOS file formats

#### [Sysinternals surrogates for macOS](https://tinyapps.org/blog/201906050700_sysinternals_surrogates_macos.html)
Jonathan Levin (the author of a [trilogy on macOS and iOS internals](http://newosxbook.com/)) offers a [plethora of tools](http://newosxbook.com/index.php?page=downloads) (many of them with source code) for digging deeper into your system, including:

- [filemon 2.01](http://newosxbook.com/tools/filemon.html) [22k] {S} [FSEvents](https://en.wikipedia.org/wiki/FSEvents) client for monitoring file system activity. [📺](https://tinyapps.org/screenshots/filemon-macos.png)
- [procexp 1.0](http://newosxbook.com/index.php?page=downloads) [729k] Process Explorer for macOS and iOS. Output can be displayed via curses or piped to other commands. Includes dynamically-updated WiFi signal strength indicator. [📺](https://tinyapps.org/screenshots/procexp.png)
- [SUpraudit](http://newosxbook.com/tools/supraudit.html) [23k] [praudit(8)](https://docs.oracle.com/cd/E88353_01/html/E72487/praudit-8.html) clone on steroids. Track all activity on a macOS system via the built-in BSM audit facility. [📺](https://tinyapps.org/screenshots/supraudit.png)

See also [fseventer - FileMon for OS X](https://tinyapps.org/blog/200807120700_filemon_for_os_x.html).