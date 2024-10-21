### SUBLIME-TEXT

#### Install with Brew
`% brew install --cask sublime-text`

##### Install Sublime Text settings from Backup
`% cp -r init/Preferences.sublime-settings ~/Library/Application\ Support/Sublime\ Text*/Packages/User/Preferences.sublime-settings 2> /dev/null`

#### Settings
```
{
        "remember_open_files":false,
        "hot_exit":"disabled",
        "create_window_at_startup": false,
        "update_system_recent_files": false,
        "auto_find_in_selection": true,
        "close_find_after_find_all": false,
        "close_find_after_replace_all": false,
        "highlight_modified_tabs": true,
        "font_face":"Menlo Regular",
        "font-size":10,
        "color_scheme": "Monokai.sublime-color-scheme",
        "theme": "Default Dark.sublime-theme",
        "dark_color_scheme": "Mariana.sublime-color-scheme",
        "light_color_scheme": "Celeste.sublime-color-scheme",
        "default_encoding": "UTF-8",
        "file_exclude_patterns":
        [
                ".DS_Store",
                "Desktop.ini",
                "*.pyc",
                "._*",
                "Thumbs.db",
                ".Spotlight-V100",
                ".Trashes"
        ],
        "folder_exclude_patterns":
        [
                ".git",
                "node_modules"
        ],
        "ignored_packages":
        [
                "Vintage",
        ],
        "index_files": true,
        "console_max_history_lines": 8,
        "show_encoding": true,
        "show_line_endings": true,
        "translate_tabs_to_spaces": false,
        "trim_trailing_white_space_on_save": true,
        "word_wrap": false
}
```

#### Packages
To manage packages, use the Command Palette (CMD+SHIFT+P) and search for the following packages:

- [Theme - Rainbow](https://packagecontrol.io/packages/Theme%20-%20Rainbow)
- [Rainbow CSV](https://packagecontrol.io/packages/rainbow_csv)
- Pretty JSON
- Transcrypt
- Filter Lines



##### Set git to use Sublime as default editor
`% git config --global core.editor "subl -n -w"`

##### Preference (plist) for Sublime Text
```
defaults read -app Sublime\ Text
defaults write ~/Library/Preferences/com.sublimetext.4.plist ApplePersistence -bool no
defaults write ~/Library/Preferences/com.sublimetext.4.plist NSQuitAlwaysKeepsWindows -bool false
defaults write ~/Library/Preferences/com.sublimetext.4.plist AutosavingDelay -int 0
```

##### Removing Group Write permissions
```
sudo chmod -R g-w /Applications/Sublime\ Text.app/
```

##### Package Control not working - https://stackoverflow.com/a/77836847
1. Close Sublime Text
2. Run:
`% ln -sf /usr/local/Cellar/openssl@1.1/1.1.1o/lib/libcrypto.dylib /usr/local/lib/`
3. Open Sublime Text, Wait for Pop-up, Restart Sublime Text

________________________

##### Shortcuts, Commands 
[Editing Multiple Lines](https://superuser.com/questions/1146905/sublime-text-3-multiline-select-how-to-go-to-a-specified-characted-on-all-lin)
1. CMD + A (select all)
2. CMD + SHIFT + L (multi line select mode)
3. CMD + LEFT ARROW (go to left of selection)
4. SHIFT + ALT + RIGHT ARROW (select first column)