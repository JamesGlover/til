# Solving too many opened files

This one is not exclusively a ruby issue, but I most frequently
run into it with capybara tests.

## The error

If you see something like this:

```
Errno::EMFILE:
       Too many open files - socket(2) for "127.0.0.1" port 9517
```

Then your system is running out of available file descriptors.
You can increase this with:

```
ulimit -n 24576
```

(You may need to set a lower number if MacOS complains)

I recommend adding the above to your `.bashrc` or `.zshrc` to ensure its always
available.
