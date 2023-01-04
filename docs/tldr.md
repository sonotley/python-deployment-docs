# Quick reference

## My workflow

This is what my workflow looks like:

- [Write software](dev.md) using Poetry to manage dependencies
- [Write tests](write-tests.md) using Pytest
- [Package the project](package.md) to tarball and/or wheel using Poetry
- Use Tox to [automatically run the tests](run-tests.md) in isolated environment(s)
- Distribute the package with [an installer script](install.md)
- Double-click [the executable](install.md) created by the installation script to run the application!

## Optional extras
There are some optional steps that can be useful:

- Single-source and automatically increment [the version number](versions.md)
- Automatically [generate documentation](docs.md) using pdoc
- Perform all testing and packaging using [GitHub Actions](github.md) or [Jenkins](jenkins.md)

## Sounds good, is there a template project that does all this stuff?
Yes! Check out [cookiecutter-snoap](https://github.com/sonotley/cookiecutter-snoap).

## Other stuff

I've also written up [things I tried but didn't ultimately use](others.md) and [things I'd like to try](future.md).