
- [[#File Management Commands]]
- [[#Image Processing]]
- [[#File Conversions]]
- [[#ExifTool Commands]]

---

## File Management Commands

- **Shows the encoding of the specified file.**  
  ```bash
  alias enc="file -I \$1"
  ```

- **Converts a timestamp to a human-readable date.**  
  ```bash
  alias ts="date -r \$1"
  ```

- **Converts a binary plist file to XML format. Usage: `binxml outputfile.xml binaryinputfile`**  
  ```bash
  alias binxml="sudo plutil -convert xml1 -o"
  ```

- **Converts an XML plist file to binary format. Usage: `xmlbin binaryoutputfile inputfile.xml`**  
  ```bash
  alias xmlbin="sudo plutil -convert binary1 -o"
  ```

- **Runs a property list (plist) validation check, ensuring that the plist file is correctly formatted.**  
  ```bash
  alias lint="plutil -lint \$1"
  ```

- **Computes the SHA-256 checksum of the specified file.**  
  ```bash
  alias shacheck="shasum -a 256 \$1"
  ```

---

### File Analysis Commands

- **Determines the type and encoding of a file.**  
  ```bash
  file [filename]
  ```

- **Shows the encoding of the specified file.**  
  ```bash
  file -I [filename]
  ```
  
- **Displays a hexadecimal and ASCII dump of a file.**  
  ```bash
  hexdump -C [filename]
  ```

- **Converts a binary file into a hexadecimal dump.**  
  ```bash
  xxd [filename]
  ```

e.g.
```sh
xxd -b /private/var/folders/vy/qdp8xcfj4s3dd5c6d1xp_y3w0000gn/C/com.apple.Safari.SafeBrowsing/Google/social_engineering\,osx\,url_expression > socialeng.txt
```

You can use `xxd` or other tools to convert the binary patterns into ASCII text:
**Example using `xxd` (as you did earlier):**
```sh
xxd -r -b googsocialeng.txt | hexdump -C
```
- `xxd -r -b` reverses the binary back to its byte form, and `hexdump -C` will then format it into a human-readable format, showing both the hex and ASCII equivalent.
If any text is encoded in ASCII, it should appear when you use this method.

- **Extracts printable strings from a binary file.**  
  ```bash
  strings [filename]
  strings -a [filename]
  ```

---
## File Conversions

- **Converts a DOCX file to Markdown format using `pandoc`.**  
  ```bash
  brew install pandoc
  pandoc -f docx -t gfm foo.docx -o foo.markdown
  ```

- [**pandoc**](https://github.com/jgm/pandoc) Universal markup converter
- [Releases](https://github.com/jgm/pandoc/releases/tag/3.3)
- [Uninstall Script](https://raw.githubusercontent.com/jgm/pandoc/main/macos/uninstall-pandoc.pl) 

---
## ExifTool Commands
[exiftool](https://github.com/exiftool/exiftool)
[unix install](https://exiftool.org/install.html)
[features](https://exiftool.org/index.html#features)

- **Executes the Image-ExifTool to read or manipulate metadata in image files, specifically located in the specified directory.**  
  ```bash
  alias exif="~/folder/macsetup/_packages/Image-ExifTool-12.84/exiftool"
  ```

- **Uses the Image-ExifTool to extract or modify geolocation metadata from image files, allowing for geographic data to be accessed or edited.**  
  ```bash
  alias exifgeo="~/folder/macsetup/_packages/Image-ExifTool-12.84/exiftool -api geolocation '-geolocation*'"
  ```

---
## Image Processing

- **Concatenates multiple PNG images into a grid and saves the result as `out.png`.**  
  ```sh
  montage *.PNG -mode concatenate -tile 2x3 out.png
  ```

- **ImageMagick**:
  - Create montages or grids of images with [ImageMagick](https://github.com/ImageMagick/ImageMagick).
  - For more details, see [this Stack Overflow question](https://stackoverflow.com/q/2853334).
