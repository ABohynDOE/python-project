# Python project setup with poetry

In this document, we detail how to setup a python project using [Poetry](https://python-poetry.org/).
Whether it is a single project or a package that will be published later, Poetry allows you to manage dependencies to the different packages used in your project and makes the project easy to build, share and publish.

In opposition with other python packaging options that needs multiple separate files (`setup.py`, `setup.cfg`, `MANIFEST.in`, ...),Poetry only needs one file for all the configuration: `pyproject.toml`.

<!-- Table of contents can be created using: https://ecotrust-canada.github.io/markdown-toc/ 
and added by pasting the output and adding {toc:} at the end -->

## Creating a new project

### Installing poetry

The easiest way to install poetry is to use `pip`:

```bash
pip install poetry
```

To verify that it is installed, you can use:

```bash
poetry about
```

### Starting a new project

To create a new project named `project-name` simply use

```bash
poetry new project-name
```

This will create a new folder in the current directory named `project-name` with the following tree structure

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

The following sections details the different files created by the `poetry new` command:

- [`pyproject.toml`](files/pyproject.md)
- [`README.rst`](files/README.md)
- [`project-name/` folder](files/project-name.md)
- [`tests/` folder](files/tests.md)

## My workflow

Here are the changes that I apply to my project.
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
<!-- TODO: detail my workflow -->

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

Here is a list of useful packages, that are interesting to add to the development environment of your new python project.

- black
- flake8
- isort
- pytest
- coverage
- sphinx
- pydocstyle

They can all be added using the `poetry add -D package-name` command.

### Black

[black](https://black.readthedocs.io/en/stable/) is a code formatter that follows [PEP8](https://www.python.org/dev/peps/pep-0008/) guidelines.
It can either be applied to an entire directory or to a single file, and can be run through python.

```bash
poetry run black path/to/dir
poetry run black path/to/file.py
python -m black path/to/dir
```

It can also be applied inside the virtual environement of poetry (`.venv`) using `black dir` or `black singlefile.py`.
`black` applies the changes to the code when called.
To prevent it from actually changing your code, you can call `black --check` and it will output the proposed changes to your files.

If you do not want black to change certain files, you need to create a configuration section called `[tool.black]` in the `pyproject.toml` file.
For example, to exclude the `foo.py` file in the root folder and set the prefered line length to 79, simply add:

```toml
[tool.black]
line-length = 79
exclude = '^/foo.py'
```

### Flake8

[flake8](https://flake8.pycqa.org/en/latest/) is a tool that enforces style consistency in your code and follows PEP8 guidelines.
It performs lint checks on your code (linting highlights syntactical and stylistic problems in your Python source code).
It can be either applied to a single file or a whole directory, and can be run through python.

```bash
poetry run flake8 path/to/dir
poetry run flake8 path/to/file.py
python -m flake8 path/to/dir
```

`flake8` checks for line length and has a default value of 79 characters.
You should change that value to match to one of `black` (or the other way around).
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

[pytest](https://docs.pytest.org/en/6.2.x/index.html) is tool that helps you write better code by rendering code testing easy.
It has the advantage of needing no boilerplate code (i.e., a test can be easily written without needing additional surrounding code) and is easily scalable to an entire library.

`pytest` runs all files of the form `test_*.py` or `*_test.py` in the current directory.
Inside these files, `pytest` will verify the output of the function of the form `test_*()`.
However multiples test functions can be grouped into the same file, as long as they have different names.

By default, `poetry` creates a `tests/` folder in which all test files are stored.
This is not mandatory for `pytest` but helps structuring your project.
`pytest` can be called on a directory or on a single file, from python or from the poetry `run` command

```bash
poetry run pytest path/to/test_dir
poetry run pytest path/to/test_file.py
python -m pytest path/to/test_dir
```

Useful options for `pytest` are `-q` to quietly pass the tests (useful if there are a lot of tests), and `-v` to add verbosity to the tests (useful if a lot of tests are in the same file).

<!-- TODO: add types of tests -->
### Coverage

[Coverage](https://coverage.readthedocs.io/en/6.0.2/index.html) is tool to check for coverage.

Run it as

```bash
poetry run coverage run -m pytest path/to/test_dir
```

Report the result as

```bash
poetry run coverage report
```

The output looks like this

```bash
Name                         Stmts   Miss  Cover
------------------------------------------------
test_project/__init__.py         2      0   100%
test_project/main.py            18      6    67%
tests/__init__.py                0      0   100%
tests/test_test_project.py      10      0   100%
------------------------------------------------
TOTAL                           30      6    80%
```

### Sphinx

[Sphinx](https://www.sphinx-doc.org/en/master/index.html) creates documentation.
Create a `docs/` folder to store all documentation-related files.
Use the quickstart tool of sphinx

```bash
cd docs/
poetry run sphinx-quickstart
```

This asks you a few questions about the project and creates two folders:

- `build/`: contains the built documentation
- `source/`: contains the source files for the documentation

The `source/` folder contains a `conf.py` file, for the configuration of sphinx when it builds the documentation.
To build your documentation, simply use

```bash
poetry run make html
```

After running the command once, you'll find all documentation files in the `build/` folder.

#### Sphinx extensions

Several extensions are useful in sphinx.
They can be added in extensions list of the `conf.py` file.

- autodoc: allows for the automatic build of functions and classes documentation in your package
- napoleon: allows the use of numpy docstrings

Be aware, that in order to use the `autodoc` extension, you must make your package "discoverable" by sphinx.
To do this, uncomment the `Path setup` section of the `conf.py` file.
It should look like this

```python
import os
import sys
sys.path.insert(0, os.path.abspath('../..')) # Path is set assuming your conf.py file is in ./docs/
```

<!-- TODO: investigate and add links to readthedocs.io -->

### Pydocstyle

Package to check the compliance of python docstrings.
Installed using

```bash
poetry add -D pydocstyle
```

## Building and publishing your project
