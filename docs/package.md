# Packaging the project
This part of things had me totally confused for a while. But it turns out recent improvements have made this somewhat simpler.

Basically I wanted a way to allow a user to recreate my application and all its dependencies inside a virtual environment with zero Python knowledge and minimal complexity (for me and them).

Packaging in Python is such a perennial topic of confusion and debate that there is actually an official organisation dedicated to it. 
The PYPA site is a useful resource, but I found it wasn't particularly friendly to people just looking for a simple opinionated solution.

## Poetry
I use Poetry to manage dependencies and provide a virtual environment when developing. The great thing about Poetry is that packaging essentially comes for free. All I need to do is type `poetry build` and my project is packaged up into a tarball and a wheel. These contain my code and the metadata defining its dependencies. The metadata also records that Poetry is the tool required to build the package from source (i.e. the tarball, also known as an sdist).

### Doesn't that mean the user needs Poetry installed?
No, thankfully not. That's one of the key reasons I chose this approach. 
Since PEP-517 and 518, pip looks in the `pyproject.toml` file to determine what build tools are required when installing a package from sdist, and retrieves these automatically. 
Better still, Poetry creates a binary wheel as well as an sdist, and wheels can be installed without any 'build backend'.

## Why bother packaging at all, why not just distibute the code?
In the case of pure Python packages your source code *is* distributed. Once someone has installed you package they can happily `import` it into their own projects or just browse your `.py` files. 

For the general Python community, the biggest advantage of packaging your code is that it means you can upload it to pypi, allowing others to discover and install it. Packaging is also an essential step if your code contains C extensions.

However, I'm talking about distributing pur Python applications to users directly, so why bother? The answers:

- Scripts
- Version control

Scripts are executable files that run some part of your code. See here for more info, but suffice to say the ability to create an executable if very useful if you want to offer a simple 'double-click here to run' user experience.

By version control I mean keeping control of the versions of your code that are out in the wild. That can be hard to do if you just commit your code to a repo and allow people to pull it and run it at any moment. For a start you have to be sure that every commit works and is uniquely identifiable by the end-user. Sure, there are ways to do this, but packaging provides a neat way of saying "at the moment I press build this, and only this, is version 1.2.3 and there will never be another version 1.2.3". 

Having a distinct 'build/package' step in your workflow separates what you ship to users from what you commit as source. If you are responsible, even informally for supporting yoru code once it goes out into the ide world then reducing the number of variations you have to support is critical to maintaining sanity.

## Other things I looked at

I initially had a very similar workflow but using pipenv instead of Poetry. It worked alright for sharing code with users, but was missing all the packaging and testing goodness.

I also like Pyinstaller as it means the target system doesn't need Python or an internet connection. I might add a Pyinstaller workflow to this site one day.
