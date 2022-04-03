# Things I'd like to try in future

## More idiomatic Linux installation

My current installation method is really aimed at Windows users. It produces self-contained directories with an executable inside. However this executable is not on the path, and you can't install using a package manager - both basic expectations for Linux users.

To be honest, I just haven't got my head around creating `.deb` packages or understanding which combinations of `bin`, `lib`, `user`, `usr`, `.local` I should be putting things in.

## Using [pdm](https://pdm.fming.dev/) instead of Poetry

I have heard of pdm and am keen to try it. I have been bitten by Poetry's slightly odd approach to pinning maximum versions (excellent article [here](https://iscinumpy.dev/post/bound-version-constraints/)).