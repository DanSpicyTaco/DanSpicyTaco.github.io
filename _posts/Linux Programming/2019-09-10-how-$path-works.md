---
categories: [Linux Programming]
---

Today I learnt how `$PATH` works.
`$PATH` is defined in a couple of places:

1.  All folders in `/etc/paths.d/` and `/etc/manpaths.d/`
2.  The `/etc/paths file`
3.  In the `.bash_profile` file

Packages can add their `$PATH` in:

- `.bashrc`
- `.zshrc`

Sometimes, a package will do all bash configuration files because they don't know what config files you have.

## Tips and Tricks

- Make path look pretty using `tr ":" "\n" <<< $PATH`
