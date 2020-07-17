# Remove the last thing typed from history

If you accidentally type your password into a plain bash prompt, it can be useful to prune your history. I've been using a variation of the following for a few years.

```bash
  # ~/.profile (or whatever you use to set up your sell session)
  alias whoops="history -d \`history 2 | head -n 1 | awk '{ print $1 }'\`; clear"
```

This sets up an alias called `whoops` which will instantly remove the last item from your history.

``` bash
  ✔ ~
  14:02 $ mypassword
  -bash: mypassword: command not found
  ✘-127 ~
  14:02 $ whoops
  # Display is cleared
  14:02 $ history
  ...
  501  git commit
  502  git push somehing
  503  history
```

`history -d n` will remove history item n from the history. We pass in the result of another command using backtick.

`history 2` returns the last two history items, and `head -n 1` picks the first of those. This is the command typed immediately before whoops, as the last item will be the whoops itself.
We use awk to extract the first item in the line, in this case the item number, and this gets used by the `history -d` command.

Finally we use `clear` to wipe the termial, to hide your embarassent, and password, from anyone standing behind you.
 
