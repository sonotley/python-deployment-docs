# Build and release on GitHub

As I developed [cookiecutter-snoap](https://github.com/sonotley/cookiecutter-snoap), it was natural for me to adopt GitHub Actions as my platform for automated builds and releases.
As such, GitHub actions has completely replaced [Jenkins](jenkins.md) in my workflow.
 
The Github Actions script below is triggered by pushing a tag and performs all the [packaging](package.md) and [testing](run-tests.md) steps.
It then publishes the application as a 'release' on the repo.

```yaml linenums="1"
name: Release

on:
  push:
    tags:
    - '*'

jobs:

  build:
    runs-on: ubuntu-latest
    env:
      PYTHON_KEYRING_BACKEND: keyring.backends.null.Keyring
    permissions:
      contents: write
    steps:
    - name: setup pyenv
      uses: "gabrielfalcao/pyenv-action@v11"
      with:
        default: 3.11
        versions: 3.8, 3.9, 3.10, 3.11
    - name: Add poetry
      uses: abatilo/actions-poetry@v2
    - uses: actions/checkout@v3
    - name: Create poetry env
      run: poetry install --with dev
    - name: Build project
      run: ./build/build.sh
    - name: Zip dist directory
      uses: vimtor/action-zip@v1
      with:
        files: dist/
        dest: my-lovely-project-${{  github.ref_name }}.zip
    - uses: ncipollo/release-action@v1
      with:
        artifactErrorsFailBuild: true
        artifacts: my-lovely-project-${{  github.ref_name }}.zip
        body: "An automatic build of My lovely project"
```