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

## Using [Hatch](https://hatch.pypa.io/latest/) instead of Poetry

Hatch is a really interesting project which I think could take care of generating wheels/sdists in place of Poetry.
It also has some cool versioning features which might mean I could retire my [script](versions.md). 
Hatch doesn't do dependency resolution, which is fine because `pip` does that. 
However, that does mean you have to manually specify dependencies in `pyproject.toml` which is a bit tedious and a source of possible errors.
I have submitted a [feature request](https://github.com/pypa/hatch/discussions/437) to Hatch for this.

## Bundling dependencies

I once read a post on the Python forums opining that application deployment should never use the target system's pip; rather
the full code including all dependencies should be bundled with the application. 
This takes us back into pex territory, so perhaps one day I will revisit pex.