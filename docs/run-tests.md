# Run tests with tox

## Why bother?

Running the unit tests we wrote previously is as simple as just executing `pytest` in the dev environment, so why do anything else? Turns out there are many good reasons:

1. You are not testing your code in a clean environment - there could be extra files, missing ones, environment variables, etc.
2. You are testing the code as it appears in your repository folders, not as it appears after installation with `pip`
3. You are not testing whether the code _can actually be_ `pip` installed
4. You are only testing the version of Python you have on your dev system

## How does tox help?

Tox is a really cool bit of software, it does so much with almost no effort at all. With a minimal amount of configuration (see below) 
you can have tox create multiple virtual environments with different versions of Python, install your package into them and run all your tests.

``` ini
    [tox]
    envlist = py37, py38, py39, py310
    isolated_build=true

    [testenv]
    deps =
        -rdist/requirements.txt
        pytest
        pytest-cov
        pytest-mock
    commands =
        pytest --cov=my_project --cov-append --cov-branch
```

## Installing tox 

Since tox will be installing your project, it doesn't make much sense to make tox a project dependency. Instead you should make it available in your test environment independently. The simplest way is just to `pip install tox` into your system Python. If you'd rather not clutter your system python with packages

## Managing packages dependencies

Note the line `-rdist/requirements.txt` above. This forces tox to `pip install` all my package's dependencies as declared in `requirements.txt` _before_ installing the package. 
I could skip this step and the dependencies would still get installed as `pip` would determine they are required for the package. 
However, by using a `requirements.txt` file generated during packaging I can specify the exact versions of every dependency (known as _pinning_ dependencies). 
I also include this file with the installer so the production environment looks exactly like the one that was tested.

## Managing test dependencies

When your project is installed by tox, the dev dependencies are not installed. This is as-expected as this is supposed to replicate a production environment. However, this means that if you want pytest in your environment (hint: you do) you need to ensure you include it as a dependency in the tox.ini file as shown above.

In the latest version of [cookiecutter-snoap](https://github.com/sonotley/cookiecutter-snoap) I have combined this step with the one above by including the test dependencies in my Poetry environment 
and then using the following command to create a combined requirements file, pinning both the runtime and test dependencies.

    poetry export -f requirements.txt --with dev --output build/requirements-with-dev.txt


## Managing multiple Python versions with pyenv

One thing that tox _doesn't_ do is install Python for you. You need to have the Python versions with which you want to test installed on your test system. As [XKCD confirms](https://xkcd.com/1987/), multiple Python installs can quickly get out of hand. Luckily [pyenv](https://github.com/pyenv/pyenv) is here to help. If you are running on Windows, there is a Windows-friendly fork [pyenv-win](https://github.com/pyenv-win/pyenv-win)