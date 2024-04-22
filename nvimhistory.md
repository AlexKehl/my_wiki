# Neovim command history

Tags: #neovim #history #vim

Deleting a command from history is possible with 

```
:call histdel(":", "expr")
```

But is somehow deletes matching by regex. E.g. it also deletes similar
commands.
Need to explore more here.

Links:

202404181807
