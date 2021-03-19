## Python Style Guide

The one we have discovered for linting is [flake8](http://flake8.pycqa.org/en/latest/). It’s a wrapper which verifies pep8, pyflakes and circular complexity. It has a low rate of false positives.

To Install:
```
$ pip install flake8
```

To Run:
```
$ flake8 /project-path
```

#### Plugin to generate the report as HTML:
`flake8-html` is a flake8 plugin to generate HTML reports of flake8 violations.

To Install:
```
$ pip install flake8-html
```

Then run flake8 passing the `--format=html` option and a `--htmldir`
```
$ flake8 /path --format=html --htmldir=flake-report
```

#### Configuring flake8:
Create a file `.flake8` to configure flake8 where we can exclude some errors, exclude some files, set the maximum line length, etc.

[Click here for configuration options](https://flake8.pycqa.org/en/latest/user/options.html#options-list)

configuration file `.flake8`:
```
[flake8]
filename =
    *.py
max-line-length = 120
statistics = True
count = True

ignore =
    E711,
    E712

exclude =
    .git,
    .js,
    .html,
    .css,
    .yaml,
    .pem,
    .txt
```
#### Error Desciption of E711 & E712:
E711 - comparison to None should be ‘if cond is None:’
E712 - comparison to True should be ‘if cond is True:’ or ‘if cond:’

E711 & E712 are ignored because while performing datastore query we filter by comparing attributes to boolean values or None.

Note: Exclude the dependencies & files that are not meant to be analyzed.

#### Setting up flake8:
`code_quality_checker.py`
```
import os
import platform
import sys
import subprocess

DEPENDENCIES = [
    'flake8',
]


class ModuleNotFound(Exception):

    def __init__(self, module_name):
        self.message = "'{}' module not found".format(module_name)

    def __str__(self):
        return self.message


def check_dependencies():
    cmd = "where" if platform.system() == "Windows" else "which"
    for dependency in DEPENDENCIES:
        is_available = subprocess.call([cmd, dependency])
        if is_available == 0:
            pass
        else:
            raise ModuleNotFound(dependency)


def run_flake8():
    path = os.path.join(os.path.abspath('.'), 'app')
    subprocess.call(['flake8', path, '--format=html', '--htmldir=flake_report'])


def open_flake8_report():
    try:
        if sys.argv[1] != "show_report":
            return
        cmd = "start" if platform.system() == "Windows" else "open"
        subprocess.call([cmd, 'flake_report/index.html'])
    except Exception:
        pass


if __name__ == "__main__":
    check_dependencies()
    run_flake8()
    open_flake8_report()

```
To run the script:
```
$ python code_quality_checker.py show_report
```
