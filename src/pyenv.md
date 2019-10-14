# pyenv - Python Manager
## Installing and Upgrading
- pyenv was installed by Homebrew - manage it there

### Adding and using a new version
1. Confirm what verions you have using `pyenv versions`
2. See which versions are availible `pyenv install --list`
3. Run `pyenv install <version>`
4. USe the new version globally using `pyenv global <version`

## Virtualenv
- pyenv-virtualenv should be used to manage virtual environments

### Creating and managing virtual environments
1. `pyenv virtual <python_version <environment_name>`  
2. Activate using `pyenv local <environment_name>`
3. Deactivate using `pyenv local <old_environment_name>`
