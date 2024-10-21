

**VSCode unused workspaceStorage cleanup:**
```sh
#!/bin/bash
#VSCode unused workspaceStorage cleanup for macOS
CONFIG_PATH=~/Library/Application\ Support/VSCodium

echo "Configuration Path: $CONFIG_PATH"

for i in "$CONFIG_PATH"/User/workspaceStorage/*; do
    echo "Checking: $i"
    if [ -f "$i/workspace.json" ]; then
        echo "Found workspace.json in $i"
        folder=$(python3 -c "import sys, json; print(json.load(open(sys.argv[1]))['folder'])" "$i/workspace.json" 2>/dev/null | sed 's#^file://##;s/+/ /g;s/%\(..\)/\\x\1/g;')
        echo "Folder extracted: $folder"
        
        if [ -n "$folder" ]; then
            if [ ! -d "$folder" ]; then
                echo "Removing workspace $(basename "$i") for deleted folder $folder of size $(du -sh "$i" | cut -f1)"
                rm -rfv "$i"
            else
                echo "Folder exists: $folder"
            fi
        else
            echo "No folder found in workspace.json"
        fi
    else
        echo "No workspace.json in $i"
    fi
done

```


[**VSCode Tips**](https://www.freedium.cfd/https://medium.com/coding-beauty/vscode-tips-tricks-98c6e2258626)

- Enable local source control with **Timeline view**; available in Explorer pane by default.
- Autosave files with **`File > Autosave`**.
- Run commands in Command Palette with **`Ctrl + Shift + P`** (Windows) or **`Shift + Command + P`** (macOS).
- Go to a file with **`Ctrl + P`**, navigate between open files with **`Alt + Left/Right`** or **`Ctrl + Tab`**.
- Go to a line with **`Ctrl + G`**.
- Delete a line with **`Ctrl + Shift + K` .**
- Enable smooth typing with **`Editor: Cursor Smooth Caret Animation`** setting.
- Format code with Format Document command, use **[Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)**, change shortcut to **`Ctrl + D, Ctrl + D` .**
- Use multiple cursors at once with **`Alt + Click`. `Ctrl + Alt + Up/Down`** adds a cursor above/below.
 ![None](https://miro.medium.com/v2/resize:fit:700/0*nINwKqOF3Lgn8qbk.gif)
- Move a line up or down with **Alt/Option + Up/Down** in Windows/Mac.
- Create a new file by double-clicking the Explorer pane or **set a custom keyboard shortcut**. Create a new file in a new folder with "**folder/file.ext**".