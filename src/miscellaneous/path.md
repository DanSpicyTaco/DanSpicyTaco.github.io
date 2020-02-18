# \$PATH

## Location

_\$PATH_ is defined in three places:

1.  All folders in `/etc/paths.d/` and `/etc/manpaths.d/`
2.  The `/etc/paths file`
3.  In the `.bash_profile` file

Packages may install in:

- `.bashrc`
- `.zshrc`

## Tips and Tricks

- Make path look pretty using `tr ":" "\n" <<< $PATH`

Brew:

```
git add .
git fetch
git reset --hard origin/master
```
