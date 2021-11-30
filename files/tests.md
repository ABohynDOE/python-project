# tests/ folder

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
