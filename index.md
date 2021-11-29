# Python project setup with poetry

In this document, we detail how to setup a python project using `poetry`.
Whether it is a single project or a package that will be published later, `poetry` allows you to manage dependencies to the different packages used in your project and makes the project easy to build, share and publish.

Contrarly to "classical" python packages, that needs multiple separate files (`setup.py`, `setup.cfg`, `MANIFEST.in`, ...) `poetry` only needs one file to do all of that: `pyproject.toml`.

* TOC
{:toc}

## Creating a new project

### Installing poetry

First, ensure that poetry is installed in your shell.
If this is not the case, the easiest way to do it is to use `pip`:

```bash
pip install poetry
```

### Starting a new project

To create a new project, simply use

```bash
poetry new project-name
```

Be aware that poetry needs to be in the `PATH` variables if you want to use it from the command line.

This will create a new directory named `project-name` with the following tree structure

```bash
project-name
├── pyproject.toml
├── README.rst
├── project-name
│   └── __init__.py
└── tests
    ├── __init__.py
    └── test_project-name.py
```

In order to track the changes on the project, initialize the folder created as a a git repository (not GitHub, simply git).

I also like to change the README file from ReStructuredText (`.rst`) to markdown format (`.md`), but this is not a mandatory step.

Finally, create a file named `main.py` in the code folder (`project-name/project-name`).
Note that the code folder will always have the same name as the project.

```bash
cd project-name && git init
rm README.rst && touch README.md
echo "# project-name" >> README.md
touch project-name/main.py
```

Now let us go through the different files created by the `poetry new` command.

#### pyproject.toml

This file contains all the details about the project and its dependencies.
It is divided in four basic sections:

- `[tool.poetry]`
- `[tool.poetry.dependencies]`
- `[tool.poetry.dev-dependencies]`
- `[build-system]`

The first section, `[tool.poetry]`, contains the details about the project:

```toml
[tool.poetry]
name = "project-name"
version = "0.1.0"
description = ""
authors = ["Author Name <author.name@email.com>"]
```

It is good practice to fill in the description, add a `README` and a `license` section.

```toml
license = "MIT"
readme = "README.rst"
```

To help find the suitable license for your project you can visit [this website](https://choosealicense.com/).

Other sections can be added to further detail the project.
The full list is available on the [poetry website](https://python-poetry.org/docs/pyproject/).

This is the only section that should be edited by hand as poetry provides a set of commands to modify the other sections of the file.

The second section, `[tool.poetry.dependencies]`, contains the version numbers of the packages used in the project.
By default, it only contains the version of python you are using.

```toml
[tool.poetry.dependencies]
python = "^3.7"
```

The third section, `[tool.poetry.dev-dependencies]`, only contains information about packages that are used in the development phase of the project.
These packages will not be included in the published version of the project.
By default, poetry includes `pytest`.

```toml
[tool.poetry.dev-dependencies]
pytest = "^3.0"
```

The last section, `[build-system]`, details the information about poetry's version and the API used to build the project.

```toml
[build-system]
requires = ["poetry>=0.12"]
build-backend = "poetry.masonry.api"
```

#### README

The `README` file introduces the project.
It contains all informations required to understand the project, the code and its usage.
The usual sections are "Installation", "Usage", "License" and "Contributing".
It is usually written using markdown (`.md` files), here is a [reference](https://commonmark.org/help/) page.

A good tyemplate for a basic `README` file can be found [here](https://www.makeareadme.com/#template-1).

By default, `poetry` creates a reStructuredText file (`.rst`) that is empty, but that can be changed with a single command line (from the project directory)

```bash
rm README.rst && touch README.md
```

#### project-name/ folder

This folder contains all the python code of the project.
By default, `poetry` populates it with a `__init__.py` file that only contains the version of the package.

```python
__version__ = "0.1.0
```

This is the folder in which you add your python code as your project grows.
A good practice is to create a `main.py` file to start.

```bash
touch project-name/main.py
```

<!-- TODO: add details about versioning in poetry -->

#### tests/ folder

This folder contains the tests of your code files that will be used by a testing library later on (`pytest` in our case).
By default `poetry` creates two files in this folder:

- `__init__.py`: empty file that shouldn't be modified if your tests don't need specific packages.
- `test_project-name.py`: a file that contains a test for the version of your package.

    ```python
    import pytest
    
    from test_project import __version__


    def test_version():
        assert __version__ == "0.1.0"
    ```

A good practice it to create a test for each new function that you create in your code.

<!-- TODO: add details about test -->

### Adding dependencies

There are two types of dependencies:

- Development dependencies: only used in the development framework, not included in the published version of the package.
- Package dependencies: used in the code of the package, will be included in the published version of the package.

In both cases, to add a dependencies in your project, DO NOT use `pip`, but rather use

```bash
poetry add package-name
```

and for development dependencies, use the `-D` or `--dev` option,

```bash
poetry add -D package-name
```

These commands will add the package to `[tool.poetry.dependencies]` and `[tool.poetry.dev-dependencies]` section of the `pyproject.toml` file, respectively.

To remove a dependency, use (with the `-D` for development dependencies)

```bash
poetry remove package-name
```

### Create a virtual environement

The advantage of `poetry` is that it allows you to work in a virtual environment that is specific to your project.
It that virtual environement, only the non-development dependencies are installed, as well as your package.
This allows you to test it in a clean environement, and use it as a new user would.

#### Creating a virtual environment

By default, poetry creates a virtual environment in `{cache-dir}/virtualenvs` (`{cache-dir}\virtualenvs` on Windows).
If no virtual environment is created, you can create one manually using

```bash
python -m venv .venv
```

To verify which virtual environment have been created in your project, you can run

```bash
poetry env list
```

#### Using your virtual environment

To run a specific command in your virtual environment, you can use the `poetry run` command.
For example, to run the `project_name/main.py` file, use

```bash
poetry run python project_name/main.py
```

However, if you want to activate your virtual environment to run several different commands in it, you can simply use

```bash
poetry shell
```

which will open a shell in your virtual environment.
To exit it, use `exit` or `deactivate`.

## Useful packages for a new project

Here is a list of useful packages that should be included in every new python projects:

- black
- flake8
- isort
- pytest
- coverage
- sphinx
- pydocstyle

Also considered are:

- tox
- pre-commit

### Black

[black](https://black.readthedocs.io/en/stable/) is a code formatter that follows PEP8 guidelines.
It is added to poetry as a developper feature

```bash
poetry add -D black
```

and can either be applied to an entire directory or to a single file, and can be run through python

```bash
poetry run black dir
poetry run black path/to/file.py
python -m black dir
```

It can also be applied inside the virtual environement of poetry (`.venv`) using `black dir` or `black singlefile.py`.

black applies the changes to the code when called.
To prevent it from actually changing your code, you can call `black --check` and it will output the proposed changes to your files.

If you want black not to change certain files, you need to create a configuration section called `[tool.black]` in the `pyproject.toml` file.
For example, to exclude the `foo.py` file in the root folder, simply add:

```toml
[tool.black]
line-length = 119
exclude = '^/foo.py'
```

### Flake8

[flake8](https://flake8.pycqa.org/en/latest/) is a tool that enforces style consistency in your code and follows PEP8 guidelines.

It is added to poetry as a developper feature

```bash
poetry add -D flake8
```

It can be either applied to a single file or a whole directory.

```bash
poetry run flake8 dir
poetry run flake8 path/to/file.py
python -m flake8 dir
```

The default maximum line length for flake8 is 79 characters.
To change that, you can either add a `setup.cfg` file in the root of your project, that contains

```toml
[flake8]
max-line-length = 90
```

or use options in the command line

```bash
flake8 --max-line-length 90 .
```

### iSort

### Pytest

### Coverage

### Sphinx

### Pydocstyle

Package to check the compliance of python docstrings.
Installed using

```bash
poetry add -D pydocstyle
```
