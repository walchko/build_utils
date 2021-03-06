# build\_utils

[![Latest Version](https://img.shields.io/pypi/v/build_utils.svg)](https://pypi.python.org/pypi/build_utils/)
[![License](https://img.shields.io/pypi/l/build_utils.svg)](https://pypi.python.org/pypi/build_utils/)
[![image](https://img.shields.io/pypi/pyversions/build_utils.svg)](https://pypi.python.org/pypi/build_utils/)

**Deprecated: since Python 2.0 is dead and I move my libraries to `pyproject.toml`, this library is not needed**

There is a lot of boiler plate code I would write over and over for my
projects, especially when I try to support both `python2` and `python3`.
This project aims to simplify it and allows me to easily add
update/improvements to all of my projects at once.

Since `python2` is not EOL, the default for building libraries for it is set
to `False`. See example below, but set `BuildCommand.py2 = True` to enable
`python2` building.

## Install

Most systems come with `python2` installed, but if you plan on doing
`python3` development then you should install that for your platform.
For macOS:

    brew install python3

### pip

The recommended way to install this library is with `pip`:

    pip install build_utils

### Development

If you wish to develop and submit git-pulls, you can do:

    git clone https://github.com/walchko/build_utils
    cd build_utils
    pip install -e .

## Usage

To use this package, at a minimum, set your repo up like:

```
myLibrary/
|
+- myLibrary/
|   |
|   +- src files
+- tests/
|   |
|   +- test.py
+- setup.py
```

Also add the following to your `setup.py`:

```python
# ... other imports ...
from build_utils import BuildCommand
from build_utils import PublishCommand
from build_utils import BinaryDistribution
from build_utils import SetGitTag

VERSION = get_pkg_version("myLibrary.__init__.py")  # can be any file where you
                                                    # put version number
PACKAGE_NAME = 'myLibrary'

# class to test and build the module
BuildCommand.pkg = PACKAGE_NAME
BuildCommand.test = True  # run all tests, True by default, False, no tests run
BuildCommand.py2 = True   # test and build python2, False by default
BuildCommand.py3 = True   # test and build python3, True by default

# class to publish the module to PyPi
PublishCommand.pkg = PACKAGE_NAME
PublishCommand.version = VERSION
SetGitTag.version = VERSION

setup(
    name=PACKAGE_NAME,
    version=VERSION,
    # ... other options ...
    cmdclass={
        'publish': PublishCommand,  # run this to publish to pypi
        'make': BuildCommand,       # run this to test/build library
        'git': SetGitTag            # this creates a new tag on your repo
    }
)
```

Take a look at the setup for this library on `github` for an example.
Note that by default, testing and both py2 and py3 are `True` by
default. Now you can build and publish a new package by:

    python setup.py make
    python setup.py publish
    python setup.py git

### Other

Get some basic system info:

```python
>>> import build_utils as bu
>>> bu.get_system()
System(os='macos', arch='x86_64', kernel='17.5.0', os_version='17.5.0')
```

Get the version number of a library:

```python
>>> import build_utils as bu
>>> bu.get_pkg_version('build_utils/__init__.py')
"0.2.0"
```

### Tests

This uses `nose` to run tests and issues the command
`python -m nose -w tests -v test.py` where `python` will be either
`python2` or `python3` depending on what you enabled.

Now if you have more than one test file, try:

```python
# assume you have test1.py, test2.py and test3.py ... do:
from .test1 import *
from .test2 import *
from .test3 import *
```

And all should work fine.

![image](https://github.com/walchko/build_utils/raw/master/pics/make.gif)

## Publishing

This uses `twine` by default. Ensure you have a config file setup like
in your home directory:

    [distutils]
    index-servers = pypi

    [pypi]
    repository: https://pypi.python.org/pypi
    username: my-awesome-username
    password: super-cool-passworld

# Change Log

| Date      | Version | Notes                    |
------------|---------|-----------------------------
2019-11-08  | 0.4.0   | set building `python2` default as `False`
2018-07-08  | 0.3.0   | added git version command and colorama
2018-06-20  | 0.2.2   | added some helper functions
2017-04-09  | 0.1.0   | init

# MIT License

**Copyright (c) 2017 Kevin J. Walchko**

Permission is hereby granted, free of charge, to any person obtaining a
copy of this software and associated documentation files (the
\"Software\"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be included
in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED \"AS IS\", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
