# Development

Developing python projects associated with [git-pull.com] all use the same
structure and workflow. At a later point these will refer to that website for documentation.

[git-pull.com]: https://git-pull.com

## Bootstrap the project

Install and [git] and [poetry]

Clone:

    git clone https://github.com/vcs-python/vcspull.git
    cd vcspull

Install packages:

    poetry install -E "docs test coverage lint format"

[installation documentation]: https://python-poetry.org/docs/#installation
[git]: https://git-scm.com/

## Development loop

### Tests

[pytest] is used for tests.

[pytest]: https://pytest.org/

#### Rerun on file change

via [pytest-watcher] (works out of the box):

    make start

via [entr(1)] (requires installation):

    make watch_test

[pytest-watcher]: https://github.com/olzhasar/pytest-watcher

#### Manual (just the command, please)

    poetry run py.test

or:

    make test

#### pytest options

`PYTEST_ADDOPTS` can be set in the commands below. For more
information read [docs.pytest.com] for the latest documentation.

[docs.pytest.com]: https://docs.pytest.org/

Verbose:

    env PYTEST_ADDOPTS="-verbose" make start

Drop into `pdb` on first error:

    env PYTEST_ADDOPTS="-x -s --pdb" make start

If you have [ipython] installed:

    env PYTEST_ADDOPTS="--pdbcls=IPython.terminal.debugger:TerminalPdb" \
    make start

[ipython]: https://ipython.org/

### Documentation

[sphinx] is used for documentation generation. In the future this may change to
[docusaurus].

Default preview server: http://localhost:8022

[sphinx]: https://www.sphinx-doc.org/
[docusaurus]: https://docusaurus.io/

#### Rerun on file change

[sphinx-autobuild] will automatically build the docs, it also handles launching
a server, rebuilding file changes, and updating content in the browser:

    cd docs
    make start

If doing css adjustments:

    cd docs
    make design

[sphinx-autobuild]: https://github.com/executablebooks/sphinx-autobuild

Rebuild docs on file change (requires [entr(1)]):

    cd docs
    make dev

    # If not GNU Make / no -J support, use two terminals:
    cd docs
    make watch

    cd docs
    make serve

#### Manual (just the command, please)

Build:

    cd docs
    make html

Launch server:

    cd docs
    make serve

### Formatting code

The project uses [black] and [isort] (one after the other) and runs [flake8] via
CI. See the configuration in `pyproject.toml` and `setup.cfg`:

Run `black` first, then `isort` to handle import nuances:

    make black isort

[black]: https://github.com/psf/black
[isort]: https://pypi.org/project/isort/
[flake8]: https://flake8.pycqa.org/

### Linting code

    make flake8

to watch (requires [entr(1)])

    make watch_flake8

## Publishing to PyPI

As of 0.10, [poetry] handles virtualenv creation, package requirements, versioning,
building, and publishing. Therefore there is no setup.py or requirements files.

Update `__version__` in `__about__.py` and `pyproject.toml`::

    git commit -m 'build(vcspull): Tag v0.1.1'
    git tag v0.1.1
    git push
    git push --tags
    poetry build
    poetry publish

[entr(1)]: http://eradman.com/entrproject/
[poetry]: https://python-poetry.org/
