# Python Package Page

- Import # TODO
- [Create Package](#cp)



## Create Package

```bash
.<ROOT>
├── <PACKAGE_NAME_FOLDER>
│   ├── __init__.py
│   └── [MODULES_NAMES]
└── setup.py
```

Add a `setup.py` file at the root
```python
from setuptools import setup, find_packages

setup(
    name=<PACKAGE_NAME>,      ## Warning can be different from PACKAGE_NAME_FOLDER, but it is suggested to set the same name
    version='1.0',
    packages=find_packages(),
)
```

Install with
```bash
# -e for editable source
pip install [-e] .
```

## PIP (Python Idex Packages)

```bash
pip install <PACKAGE_NAME>
pip uninstall <PACKAGE_NAME>

pip cache info
pip cache list
pip cache dir
pip cache purge
```