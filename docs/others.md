# Things that I tried but didn't quite work out

## [pipenv](https://pipenv.pypa.io/en/latest/)- where I started

My earliest version of this workflow involved providing a pipfile and one batch/shell script to install (`pipenv install`) 
and another to run the code inside the virtual environment created by pipenv. 
This meant the target system needed to have pipenv installed, which was a minor annoyance. 
It also didn't provide an executable or any help with versioning etc. 
In any case, pipenv seems to be in decline, although it has had more regular releases again recently.

## [pipx](https://github.com/pypa/pipx) - applications with their own environments

During my research I discovered pipx, and I love it. 
The approach I describe here can also work with pipx, you simply take the wheel or sdist built with poetry and install it with pipx. 
The reason I don't use pipx everywhere is that it creates its virtual environments in an obscure user folder 
which means they are only on the path for the user who installed them and could be accidentally deleted if that user's profile is removed (i.e. on a server). 
I suspect on Linux this wouldn't be an issue as long as you always `sudo pipx install` but on Windows it's a show-stopper for server deployment.

## [pex](https://pex.readthedocs.io) - portable Python executables

These sounded promising - basically a script that brings its own virtual environment without requiring any installation. 
But the learning curve was a little too steep and the adoption of pex seems low.
I do plan on revisiting pex as a way to provide completely self-contained deployments.

## [pyinstaller](https://pyinstaller.readthedocs.io) - executables with Python included

Pyinstaller is great. If you need to package up you application as a single portable executable file it is extremely useful. 
Resolving paths can be a little tricky (as it is executed from a temporary directory) but tractable. T
he main limitation of Pyinstaller is that it is more like compiled languages - you need to build it on the target platform or you may find your exe doesn't run. 
This means having multiple builds and multiple build environments which is all extra management effort.