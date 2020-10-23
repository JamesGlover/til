# Fixing Insecure Completion Dependent Directories

## Problem

By default, zsh tries to load auto-completion definitions from
subdirectories under: `/usr/local/share/zsh`

Unfortunately this can cause problems if another user on your
system has installed homebrew at the system level, but retaining
their ownership of the directories in `/usr/local/share/zsh`.

If using oh-my-zsh you'll see the following when opening a new
session:

```zsh
Last login: Fri Oct 23 11:40:35 on ttys007
[oh-my-zsh] Insecure completion-dependent directories detected:
drwxrwxr-x  3 20196  admin   96 15 Apr  2020 /usr/local/share/zsh
drwxrwxr-x  5 20196  admin  160 20 Apr  2020 /usr/local/share/zsh/site-functions
lrwxr-xr-x  1 20196  admin   39 15 Apr  2020 /usr/local/share/zsh/site-functions/_brew -> ../../../Homebrew/completions/zsh/_brew
lrwxr-xr-x  1 20196  admin   44 15 Apr  2020 /usr/local/share/zsh/site-functions/_brew_cask -> ../../../Homebrew/completions/zsh/_brew_cask
lrwxr-xr-x  1 20196  admin   88 20 Apr  2020 /usr/local/share/zsh/site-functions/_brew_services -> ../../../Homebrew/Library/Taps/homebrew/homebrew-services/completions/zsh/_brew_services

[oh-my-zsh] For safety, we will not load completions from these directories until
[oh-my-zsh] you fix their permissions and ownership and restart zsh.
[oh-my-zsh] See the above list for directories with group or other writability.

[oh-my-zsh] To fix your permissions you can do so by disabling
[oh-my-zsh] the write permission of "group" and "others" and making sure that the
[oh-my-zsh] owner of these directories is either root or your current user.
[oh-my-zsh] The following command may help:
[oh-my-zsh]     compaudit | xargs chmod g-w,o-w

[oh-my-zsh] If the above didn't help or you want to skip the verification of
[oh-my-zsh] insecure directories you can set the variable ZSH_DISABLE_COMPFIX to
[oh-my-zsh] "true" before oh-my-zsh is sourced in your zshrc file.
```

And in general:
```zsh
~ compaudit
There are insecure directories and files:
/usr/local/share/zsh/site-functions
/usr/local/share/zsh
/usr/local/share/zsh/site-functions/_brew
/usr/local/share/zsh/site-functions/_brew_cask
/usr/local/share/zsh/site-functions/_brew_services
```

The suggested solution assumes that you own the directories, and have just set
the permissions incorrectly. But if you don't have sudo privileges you can't
claim ownership of the files.

Meanwhile, the other solution is disabling the check completely. But really, I
want to not load the folders added by 'admin' at all.

## Solution

Zsh loads its completions and functions using the environment variable fpath.

```zsh
echo $fpath
/Users/your_username/.oh-my-zsh/plugins/git /Users/your_username/.oh-my-zsh/functions /Users/your_username/.oh-my-zsh/completions /usr/local/share/zsh/site-functions /usr/share/zsh/site-functions /usr/share/zsh/5.7.1/functions
```

So just remove the problem directories from this list, and add it to the top of
your `.zshrc` file:

```zsh
fpath=(
  /Users/your_username/.oh-my-zsh/plugins/git
  /Users/your_username/.oh-my-zsh/functions
  /Users/your_username/.oh-my-zsh/completions
  /usr/share/zsh/site-functions
  /usr/share/zsh/5.7.1/functions
)
```
