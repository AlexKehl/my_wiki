# Git

## How to grep git repository

Tags: #git #grep

This shows the commits which contain the search term.
Afterwards, you can use the commit hash to view the changes in that commit.

```
git log -G "search term"
```

## Fix casing issues after renaming a file

```bash
qq
t mv --force --cached path/to/file path/to/File
git commit -m "Fix casing issues"
```

## Remove tracked files from git

```bash
git rm --cached path/to/file
```
