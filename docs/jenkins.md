# Automate packaging with Jenkins

> When I first created snoap, I was working in a business that had a Jenkins server.
> I've since moved on to using GitHub Actions, but kept this section of the site for reference.

Jenkins can automate all the packaging steps I've documented on this site. It's especially useful if you are sharing your applications with a wider team who have access to Jenkins. 

In this scenario, all you need to do is to commit your code and then anyone who wants a copy of the tested, deployable application can grab it from Jenkins. You can have Jenkins build every time you commit, nightly, or just build on-demand.

Getting Jenkins to perform your testing and packaging is pretty straightforward. You need to create a Jenkins job, associate your Git repository with it and then add shell script to perform all the steps documented on this site. It should look something like this.

```bash linenums="1" title="Shell script to be executed by Jenkins"
# Make all required modifications to PATH
export PATH="$HOME/.local/bin:$PATH"
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
# Make all required versions of Python available for test
pyenv local 3.8.12 3.9.7 3.10.0
# Run tests and build
chmod +x build/build.sh
./build/build.sh
# Build the API docs
poetry install
poetry run python -m pdoc example_package -d numpy -o ./pdoc
# Zip up the dist folder for distribution
(cd dist && zip -r ../example-package-"$(cat ../example_project/version.txt)".zip ./*)
(cd pdoc && zip -r ../api-docs-for-developers-"$(cat ../example_project/version.txt)".zip ./*)
```

The script `build_current_version.sh` is separated purely for convenience so I can easily run it in my dev environment.

```bash linenums="1" title="build_current_version.sh"
#!/usr/bin/env bash

tox || { echo "TESTS FAILED - BUILD ABORTED" ; exit 1; }
echo "Test passed - building sdist and wheel"
poetry build || { echo "POETRY BUILD FAILED - BUILD ABORTED" ; exit 1; }        
```    
