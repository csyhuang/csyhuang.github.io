---
layout: post
title: Local wave activity package updated to version 0.3.7
tags: ['se']
comments: true
---

The [python package](https://github.com/csyhuang/hn2016_falwa) `hn2016_falwa` has just been updated with the following changes:
- Switched the unittest framework from `unittest` to `pytest`
- Improved interface of the QGField object
- Added a new function `hn2016_falwa.download_data.retrieve_erai` to download ERA-Interim data (but not connected to the main program yet)

My package uses `f2py`. When switching from `unittest` to `pytest`, there are several changes to make in `setup.py` and `.travis` to accommodate such usage:

In `setup` (`numpy.distutils.core.setup`), remove the argument
```python
test_suite='tests.my_module_suite'
```
and add:
```python
    packages=find_packages(),
    setup_requires=['pytest-runner'],
    tests_require=['pytest'],
```

In `.travis`, the `script` section looks like:
```bash
script:
  - python setup.py pytest
```
