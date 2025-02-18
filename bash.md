# Bash

## Dates
```bash
date "+%Y-%m-%d:%H-%M-%S"
```

## Stdout and Stderr redirection
1>> and 2>> are redirections for specific file-descriptors, in this case the standard output (file descriptor 1) and standard error (file descriptor 2).

```bash
command 1>> stdout 2>> stderr
```
