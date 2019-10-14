# $PATH
- $PATH is defined in three places:
 1. All folders in `/etc/paths.d/` and `/etc/manpaths.d/`
 2. The `/etc/paths file`
 3. In the `.bash_profile` file

- The `.bash_profile` is unique to each user  

Packages may install in:
- `.bashrc`
- `.zshrc`
  
- Make path look pretty using `tr ":" "\n" <<< $PATH`
