### Installation
To install the bash scripts:

    1. Put the script anywhere in your PATH. For instance, `/usr/local/bin`.
    2. Make sure it's executable.
    3. Done.

----------------------------------------------------------------------------------------

### barchiver Python script
Must have at least Python 3.2

- Supports archive formats gzip, bzip2, tar and zip.
- Supports pushing archive to a remote server.
- Supports pointing script at a config file for the remote server params.

Example usage:

    barchiver --name my_archive_20131225 --src . --dest /usr/local/www/dotfiles
    barchiver --name motley --config ~/.barchiver/benjamintoll.com

For example, the file ~/.barchiver/benjamintoll.com in the example above could look like this:

    {
        "hostname": "benjamintoll.com",
        "username": "benjamin",
        "port": 2009,
        "format": "bztar"
    }

Usage:

    Optional flags:
    -h, --help    Help.
    -n, --name    An optional archive name. The default is YYYYMMDDHHMMSS.
    -s, --src     The location of the assets to archive. Defaults to cwd.
    -d, --dest    The location of where the assets should be archived. Defaults to cwd.
    -r, --root    The directory that will be the root directory of the archive. For example, we typically chdir into root_dir before creating the archive. Defaults to cwd.
    -c, --config  A config file that the script will read to get remote system information. Session will be non-interactive. Useful for automation.

### csv bash script
I had several csv files that I needed to slice by column(s). In addition, some lines had commas within double-quotes, which is no good when the field delimiter is the comma. So, I decided that this script will replace all commas not within double quotes to an underscore and then specify the underscore as the field delimiter in my import tool.

The command-line arguments are the csv to operate on, followed by an optional filename for the newly-created file (.csv will be appended) and the optional columns to slice.

If using a tool like mysqlimport, name the new file the same name as the database table that the records should be inserted into.

Example usage:

    bash csv.bash psa.csv cards 2-5


### bfind bash function

    bfind() {
        vim -p $(find "$1" -type f -name "$2")
    }

Example usage:

    bfind . Component.js
    bfind dir_name file_name

This does the following:

- finds every file that matches file_name in dir_name
- vim then opens each file in a vertically split pane

### bgrep bash function

    bgrep() {
        vim -p "+/$1" $(grep -riIl "$1" "$2" | uniq)
    }

Example usage:

    bgrep DEBUG[_]*SCRIPT .
    bgrep regex dir_name

This does the following:

- searches every file in dir_name
- pipes the results to cut which selects the first column of each line (delimited by a colon)
- pipes the result to uniq which filters out duplicates
- vim then opens each file in a vertically split pane
- the first file will be opened at the line where the regex is first found in the file

### make_ticket bash script (for ExtJS)
Example usage:
    `make_ticket 5671 3.4.0`

Example usage:
    `make_ticket -t EXTJSIV-11987 -v 4`

    make_ticket ticket_dir_name ext_version

Run the script wherever your ticket directories are located.

It will ask for the location of your SDK or build directory if the proper environment variables have not been set (`$SDK4`, `$SDK5` `$SDK6`).

This does the following:

- makes the directory in which the new ticket will live (in this example, `5671`).
- creates an `index.html` document within the new directory which properly references the JavaScript and CSS resources it needs to load.

The script properly handles sencha ExtJS versions 4, 5 and 6.

