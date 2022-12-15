# PyPi Packaging

I needed to publish a package onto [PyPi](https://pypi.org/). As I had to rework
out the steps from scratch, I thought that I would document them them at the same
time.

## Code Structure

The Python code needs to conform to a basic structure.

```bash
`._ project
    `._ __init__.py
     |_ project.py
 |_ tests
    `._ __init__.py
     |_ project_tests.py
 |_ .gitignore
 |_ LICENSE
 |_ main.py
 |_ pyproject.toml
 |_ requirements.txt
 |_ README.md
```

The ```pyproject.toml``` defines the project itself. The basic structure:

```toml
[project]
name = "example_package_YOUR_USERNAME_HERE"
version = "0.0.1"
authors = [
  { name="Example Author", email="author@example.com" },
]
description = "A small example package"
readme = "README.md"
requires-python = ">=3.7"
classifiers = [
    "Programming Language :: Python :: 3",
    "License :: OSI Approved :: MIT License",
    "Operating System :: OS Independent",
]

[project.urls]
"Homepage" = "https://github.com/pypa/sampleproject"
"Bug Tracker" = "https://github.com/pypa/sampleproject/issues"
```

## Setup

Prior to building and deploying your package, you will need to install the following
libraries and tools:

  * [build](https://pypa-build.readthedocs.io/en/stable/index.html)
  * [twine](https://twine.readthedocs.io/en/latest/)

```bash
python3 -m pip install --upgrade build twine
```

## Building and Deploying

To build you library for deployment you should just be able to run:

```bash
python3 -m build
```

**Note:** It's probably a good idea to delete any of the previous versions first so that you have a clean starting point.

```bash
rm -rf build/ dist/ your-project.egg.info
```

Check by publishing to the [Test PyPi](https://test.pypi.org/) repository first, to ensure that nothing is
broken

```bash
python3 -m twine upload --repository testpypi dist/*
```

Once it's successfully uploaded, you can test it locally in a test project using:

```bash
python3 -m pip install --index-url https://test.pypi.org/simple/ --no-deps your-package
```

If you're happy that all of the steps have worked then you can upload to the
production instance of PyPi using:

```bash
python3 -m twine upload dist/
```

## References

  * https://towardsdatascience.com/deep-dive-create-and-publish-your-first-python-library-f7f618719e14
  * https://packaging.python.org/en/latest/flow/
  * https://packaging.python.org/en/latest/tutorials/packaging-projects/
