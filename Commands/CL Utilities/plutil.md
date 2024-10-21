
---

### `plutil` Commands and Usage

#### Conversion

- **Convert a Property List to XML format:**
  ```sh
  plutil -convert xml1 <file>
  ```

- **Convert a Property List to Binary format:**
  ```sh
  plutil -convert binary1 <file>
  ```

- **Convert a Property List to XML format, direct output to file:**
  ```sh
  plutil -convert xml1 -o <outputfile> <file>
  ```

- **Convert a Property List to Binary format, direct output to file:**
  ```sh
  plutil -convert binary1 -o <outputfile> <file>
  ```

- **Property List validation check to ensure correct format**
  ```sh
  plutil -lint <file>
  ```

#### Reading Property Lists

- **Read the Launch Services preferences:**
  ```sh
  defaults read com.apple.LaunchServices/com.apple.launchservices.secure
  ```

- **Location of the Launch Services preferences file:**
  ```sh
  ~/Library/Preferences/com.apple.LaunchServices/com.apple.launchservices.secure.plist
  ```

#### Creating and Modifying Property Lists

- **Create an empty XML Property List file (optional):**
  ```sh
  plutil -create xml1 Info.plist
  ```

- **Insert an empty array into a Property List file:**
  ```sh
  plutil -insert SBAppTags -array Info.plist
  ```

- **Append a string to an existing array in a Property List file:**
  ```sh
  plutil -insert SBAppTags -string hidden -append Info.plist
  ```

  **Result:**
  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
  <plist version="1.0">
  <dict>
      <key>SBAppTags</key>
      <array>
          <string>hidden</string>
      </array>
  </dict>
  </plist>
  ```

- **Insert an array in XML format into a Property List file:**
  ```sh
  plutil -insert SBAppTags -xml "<array><string> hidden </string></array>" Info.plist
  ```

- **Insert a dictionary in XML format into a Property List file:**
  ```sh
  plutil -insert SBAppTags -xml "<dict><key>myFirstKey</key><string>hidden</string></dict>" Info.plist
  ```
---
