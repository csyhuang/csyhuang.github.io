---
layout: post
title: Compile cython modules
comments: true
tags: ['setup', 'se']
---

It took me a while to figure out how to import cython modules. Here is a configuration that works:

```
├── hn2016_falwa
│   ├── cython_modules
│   |   ├── pyx_modules
|   |   |   ├── __init__.py
|   |   |   ├── dirinv_cython.pyx
|   |   ├──setup_cython.py
|   |   ├──check_import.py
...
```

In `dirinv_cython.pyx` I have:

```python
def x_sq_minus_x(x):
    return x**2 - x
```

In `__init__.py` I have:

```python
import dirinv_cython
```

In `setup_cython.py` I have:

```python
from distutils.core import setup
from Cython.Build import cythonize

setup(name='dirinv_cython',
      package_dir={'pyx_modules': ''},
      ext_modules=cythonize("pyx_modules/dirinv_cython.pyx"))
```

The script to test the import is `check_import.py`:

```python
from pyx_modules import dirinv_cython

ans = dirinv_cython.x_sq_minus_x(19)
print(f"ans = {ans}")
```

Executing `check_import.py` gives
```
ans = 342
```