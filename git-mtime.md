git-mtime is a Git extension that finds a [last modified times through Git commits](https://jeffkreeftmeijer.com/git-file-modified-time/):

Git repositories provide records of deliberate updates through commit logs. By retrieving the last commit date for a specific file, we can find the last date a file was purposefully updated.

```shell
#!/bin/sh
path="${@: -1}"
dir=$(dirname "$path")

hash=$(git -C "$dir" log \
           --format="%ad %H" \
           --date=format:"%s" \
           --grep="\[date skip\]" \
           --invert-grep \
           -- $(basename "$path") |\
           sort --reverse |\
           awk '{ print $2 }' |\
           head -n1)

git -C "$dir" show --format="%ad" --quiet "${@:1:$#-1}" "${hash}"
```


# Installation

Download git-mtime and symlink it to `/usr/local/bin/git-mtime`:

```shell
curl --remote-name --silent https://raw.githubusercontent.com/jeffkreeftmeijer/git-mtime/main/git-mtime
ln -s $(realpath git-mtime) /usr/local/bin/git-mtime
```


# Usage

Execute `git mtime` with a path to a file in a Git repository:

```shell
git mtime file.txt
```

    Mon Mar 6 18:24:11 2023 +0100

Even outside of the repository, `git-mtime` can find the commit history and find a file's last update date:

```shell
git mtime repository/file.txt
```

    Mon Mar 6 18:24:11 2023 +0100

By relying on `git-log` internally, `git-mtime` supports Git's `--date` flag:

```shell
git mtime --date=format:"%F %R" repository/file.txt
```

    2023-03-06 18:24

If a commit touches a file, but you don't want to modify the file's modified time, mark the commit with `[date skip]` in its commit message to make `git-mtime` ignore it:

```shell
echo "file.txt" >> repository/file.txt
git -C repository add . 
git -C repository commit --quiet --message 'Fix file.txt' --message "[date skip]" 
./git-mtime repository/file.txt
```

    Mon Mar 6 18:24:11 2023 +0100
