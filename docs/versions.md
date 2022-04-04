# Version numbering

## Single-sourcing the version number

Having a single, definitive version number for your project is obviously highly desirable - why bother with version numbers at all if they are ambiguous? So it's very handy that the `pyproject.toml` file contains a version attribute. Poetry also provides the `poetry version` command to access or increment the version number. **If you're looking for maximum simplicity, you can stop there**, but as we shall see below there are still some traps lurking for the unwary.

## Accessing the version number

Once you have a version number, you are likely to want to use it. You might want to log the version number on startup or print it to your user interface. You may also want to provide a `__version__` module attribute, a common convention to allow others to easily check the version of your package they are using.

Using the above method you get three out-of-the-box ways to access the version number, but none of them is fool-proof

1. Use `poetry version` to return it
2. Read it out of the pyproject.toml file
3. Get it from importlib.metadata

Option (1) is fine during development or packaging but no use once you've distributed the package as it requires you to have poetry installed and to have installed your project using `poetry install`.

Option (2) you could just about get away with during development, but in deployment it would be a mess. The `pyproject.toml` file is meant for use when packaging or installing, not during runtime. For a start you'd have to find the file buried in your site-packages directory, then you'd need the non-standard toml package installed to pass it (lets skip over the fact Python is trying to standardise on a file in a format it doesn't actually support in the standard library)

Option (3) is the 'correct' one and avoids all the issues above. All you need to do is:


``` python linenums="1"
from importlib.metadata import version
__version__ = version("my_package_name")
```

Frustratingingly, this has two drawbacks:

- It requires Python 3.8 which makes it a non-starter for the Debian 10 servers I often deploy to, which use 3.7.
- It only works *after* the code has been packaged and installed. If you try to invoke it when running your code from the dev environment it won't work, which is annoying.

## What I actually do

To overcome the various limitations above, I do the following

- To increment the version number, I use a script that wraps poetry version *and* writes a `VERSION.TXT` file into each package within my project
- To access the version number, I read the number in from `VERSION.TXT` in `__init__.py` and assign the name `__version__` to it.

My full script also includes an experimental implementation of C# style version numbers, but a stripped down version looks like this.

``` python linenums="1"
"""Script to apply versions in projects using Poetry

This simple version is actually less functional than calling `poetry version`
but shows how I create `version.txt` in each package
"""
import sys
import subprocess
import re
import os

short_version_pattern = re.compile(r"^\d+\.\d+$")
long_version_pattern = re.compile(r"^\d+\.\d+\.\d+(\.\d+)?$")


def version_to_str(version):
    return ".".join(str(x) for x in version)


def write_version_to_pyproject(version):
    subprocess.run(["poetry", "version", version_to_str(version)])


def write_version_to_packages(version):
    dir_list = next(os.walk('.'))[1]
    for d in dir_list:
        if d[0] not in (".", "_") and os.path.exists(os.path.join(d, "__init__.py")):
            target_path = os.path.join(d, "version.txt")
            with open(target_path, 'w') as f:
                f.write(version_to_str(version))


if len(sys.argv) > 2:
    print("Too many arguments")

elif long_version_pattern.match(sys.argv[1]):
    v = sys.argv[1].split(".")
    version = tuple(v)
else:
    print(f"Invalid argument '{sys.argv[1]}'")
    exit(1)
write_version_to_pyproject(version)
write_version_to_packages(version)
```
