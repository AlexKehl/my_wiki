# Working with Archives

## Creating a tar.gz archive

```bash
tar -czvf archive.tar.gz /path/to/directory
```

explanation:
- `c` create a new archive
- `z` compress the archive with gzip
- `v` verbosely list files processed
- `f` write archive to specified file

## unpacking a .gz archive

```bash
gzip -d archive.gz
```
