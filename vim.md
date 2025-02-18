# Vim

## Disable warning about changing a read-only file
```vim
set noro
```
But for some reason it does not work if put in vimrc. It only works if done via command line.
What works is doingt this in .vimrc
```vim
au BufEnter * set noro
```
