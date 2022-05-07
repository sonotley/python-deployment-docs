# Things I'd like to try in future

## More idiomatic Linux installation

My current installation method is really aimed at Windows users. It produces self-contained directories with an executable inside. 
However, this executable is not on the path, and you can't install using a package manager - both basic expectations for Linux users.

To be honest, I just haven't got my head around creating `.deb` packages or understanding which combinations of `bin`, `lib`, `user`, `usr`, `.local` I should be putting things in.

## Using [pdm](https://pdm.fming.dev/) instead of Poetry

I have heard of pdm and am keen to try it. I have been bitten by Poetry's slightly odd approach to pinning maximum versions 
(excellent article [here](https://iscinumpy.dev/post/bound-version-constraints/)) and pdm seems to deal more neatly with this aspect. 
As I understand it pdm is intended to be used as a package manager on the target machine, which goes somewhat against my 'no prerequisites' principle. 
However, if it were to become ubiquitous like pip or npm, I could certainly see myself using it.

## Pinning dependencies

Currently, I just rely on the target system's pip to resolve dependencies from `pyproject.toml` but much has been said for
pinning dependencies to the exact versions that were tested. Poetry supports this using the poetry.lock file, but pip
can't understand this so I would have to export a `requirements.txt` and `pip install` that prior to installing my package. 
Should be easy, but I haven't got round to it yet.

## Bundling dependencies

I once read a post on the Python forums opining that application deployment should never use the target system's pip; rather
the full code including all dependencies should be bundled with the application. 
This takes us back into pex territory, so perhaps one day I will revisit pex.