# evilarc

## Purpose
evilarc lets you create a zip file that contains files with directory traversal characters in their embedded path.  Most commercial zip program (winzip, etc) will prevent extraction of zip files whose embedded files contain paths with directory traversal characters.  However, many software development libraries do not include these same protection mechanisms (ex. Java, PHP, etc).  If a program and/or library does not prevent directory traversal characters then evilarc can be used to generate zip files that, once extracted, will place a file at an arbitrary location on the target system.

## Example Usage

```bash
python3 evilarcVer2.py check.php -f evil_zip_with_php.zip -o unix -d 10 -p var/www/html
````

**Output:**

```
Creating evil_zip_with_php.zip containing ../../../../../../../../../../var/www/html/check.php
```

This creates a zip archive `evil_zip_with_php.zip` that contains a file `check.php`, embedded with a crafted path designed to escape 10 directories upward and then write to `var/www/html`. When extracted by a vulnerable application, the file may be written to `/var/www/html/check.php`.

---

## Options

| Option                       | Description                                                                                                                                                                          |                                                                                                                                                                               |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<input_file>`               | The local file you want to embed inside the archive. Example: `shell.php`.                                                                                                           |                                                                                                                                                                               |
| `-f`, `--output-file <file>` | Name of the output archive. The format is based on file extension. Supported: `.zip`, `.jar`, `.tar`, `.tar.gz`, `.tgz`, `.tar.bz2`. <br>**Default**: `evil.zip`.                    |                                                                                                                                                                               |
| `-o`, \`--os \<win           | unix>\`                                                                                                                                                                              | Target OS that determines the type of directory traversal. <br>• `win`: Uses `..\\` for Windows paths. <br>• `unix`: Uses `../` for Unix/Linux paths. <br>**Default**: `win`. |
| `-d`, `--depth <number>`     | Number of directories to traverse upward using `../` or `..\\`. This is the number of traversal sequences. <br>**Default**: `8`.                                                     |                                                                                                                                                                               |
| `-p`, `--path <path>`        | Path to append **after** traversal. For example: `var/www/html` will result in `../../.../var/www/html/shell.php`. Optional. Leave empty to drop file in upper-level directory only. |                                                                                                                                                                               |


