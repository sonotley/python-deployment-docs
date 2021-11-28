# Development environment

## The basics
My development environment consists of:

- Pycharm with Poetry plugin
- Poetry
- Git

Pycharm is largely just a personal preference, you can follow this workflow with any IDE, or none. Similarly Git is central to how I share, backup and manage the history of my source code, but has no place in my deployment workflow other than as a way to get the source to the place it will be packaged. Poetry however, is pretty important.

### What is Poetry? I've never heard of it.

[Poetry](https://python-poetry.org/) is a fairly young project which provides an all-in-one solution to packaging and dependency management. I use Poetry because it makes it very easy to package my code up into something that someone else can `pip install` (or indeed `poetry install` but as you'll see, I don't want to rely on the target system having Poetry installed).

## The workflow
When starting a new project, either create it using `poetry new` or select Poetry from the available interpreters in Pycharm. This will create a basic project structure including two packages, one for your source and one for your [tests](write-tests.md). It will also create a virtual environment (not in the project directory by default) and `pyproject.toml` file.

The `pyproject.toml` seems to be the source of some mild controversy in the Python community, but we can ignore that. The great thing for us is that it's the only configuration file we need for our project. 

To install additional packages from PyPI, rather than using `pip`, just use `poetry add`. This will install the package into your environment and add it to the dependencies section of `pyproject.toml`. You can add dev dependencies with the `--dev` switch.

### Talking of dev dependencies...

As well as [the test dependencies](run-tests.md) I find the following useful to have in my dev environment.

- Black for code formatting
- Pydocstyle to check my docstrings
- mypy to check my type hints (Pycharm does this automatically)