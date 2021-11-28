# Installing on the target system

## Overview
When distributing my application to the target system, I provide a batch (Windows) and bash (Linux) scripts along with the wheel created by Poetry. I'll also provide a readme and any configuration or resource files required. The script does the following:

- Creates a directory in a location of the user's choosing
- Creates a virtual environment in an `env` subdirectory
- Activates the virtual environment and pip installs my project from the wheel
- Copies readme and resources into the target directory
- Makes a link to the script we declared in the `pyproject.toml` files (which by defaul is created several folders deep inside the virtual enviroment) at the top level of the new directory

This leaves you with something like this, which looks remarkably like a 'normal' Windows application. You can double click the executable to run the app, or make it the target of a service and it will just work. Importantly for me, it works identically on Linux.

```
my-app/
├─ env/
├─ resources/
├─ my-app.exe
├─ readme.md
├─ setup.yaml
```
>I've been considering creating a .deb package, or as a minimum installing into  
> `/usr/local/bin` on Debian to fit in with the norms of Linux, but I haven't got that far


## How to create an executable
You can tell pip (or equivalent) to create executables by declaring scripts in `pyproject.toml`:

```ini
[tool.poetry.scripts]
my-executable = "my_package.my_module:my_function"
```

## The installation script

This is the Linux version, both versions are on my github.

``` bash linenums="1"
#!/usr/bin/env bash
set -e # (1)
echo "*************************"
echo "Installing My Great Project"
echo "*************************"
filePath=${1:-"/opt"}/my-great-project # (2)
echo "Installing to $filePath"
echo "Building Python virtual environment"
python3 -m venv $filePath/env # (3)
target=(my_great_project*.whl) # (4)
source $filePath/env/bin/activate # (5)
pip install "${target[0]}" # (6)
ln -s $filePath/env/bin/my-great-project $filePath/my-great-project # (7)
cp -R assets $filePath/assets # (8)
cp setup.yaml $filePath/setup.yaml

```

1. stop if there's an error
2. allow user to specify install location or default to /opt
3. create virtual environment inside the installation folder
4. use wildcard to allow for version number
5. activate the venv we just created
6. install your project from the wheel file
7. create a symlink to the script for convenience
8. copy any other files that you need to distribute with the app