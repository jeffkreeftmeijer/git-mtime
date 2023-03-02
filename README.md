# git-mtime

Returns modified times for deliberate changes to files.
Git repositories provide records of deliberate updates through commit logs.
By retrieving the last commit date for a specific file, we can find the last date a file was purposefully updated.

## Installation

    git clone git@github.com:jeffkreeftmeijer/git-mtime.git
    cd git-mtime
    ln -s $(realpath git-mtime) /usr/local/bin/git-mtime

## Usage

Given a repository with two commits:

    commit 7c968f930a7782a339aaa07488018522c0548790
    Author: Jeff Kreeftmeijer
    Date:   Thu Mar 2 11:26:56 2023 +0100

	Update file.txt

	[date skip]

    commit fbae95bc56608eed6096965bfb76c8c1f2000af2
    Author: Jeff Kreeftmeijer 
    Date:   Thu Mar 2 11:09:56 2023 +0100

	Add file.txt

Get the modified date for `file.txt`:

    git mtime file2.txt
    Thu Mar 2 11:09:56 2023 +0100

Format the date with the `--date` flag:

    git mtime --date=format:"%F %R" file.txt
    2023-03-02 11:09

