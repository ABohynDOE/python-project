# pyproject.toml

This file contains all the details about the project and its dependencies.
An example of a basic `pyproject.toml` file can be found [here](https://github.com/ABohynDOE/python-project/blob/88338f296665a48c4909ba28e57a52b33ed084b1/pyproject.toml).
It is divided in four basic sections:

## `[tool.poetry]`

This section contains the details about the project:

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

## `[tool.poetry.dependencies]`

This section contains the version numbers of the packages used in the project.
By default, it only contains the version of python you are using.

```toml
[tool.poetry.dependencies]
python = "^3.7"
```

## `[tool.poetry.dev-dependencies]`

This section only contains information about packages that are used in the development phase of the project.
These packages will not be included in the published version of the project.
By default, poetry includes `pytest`.

```toml
[tool.poetry.dev-dependencies]
pytest = "^3.0"
```

## `[build-system]`

The last section details the information about poetry's version and the API used to build the project.

```toml
[build-system]
requires = ["poetry>=0.12"]
build-backend = "poetry.masonry.api"
```
