I've found [pdoc](https://pdoc3.github.io/pdoc/doc/pdoc/#gsc.tab=0) to be a pretty neat way to quickly produce API documentation for a project. I simply add pdoc as a dev dependency in poetry and run 

    poetry run pdoc

That's all it takes to build nice-looking API documentation from my docstrings. This can be bundled with the package or used to build a website.