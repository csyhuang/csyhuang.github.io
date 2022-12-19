---
layout: post
title: hn2016_falwa release 0.6.1 + remarks on GitHub CLI and repo management
comments: true
image_url: ''
tags: ['setup', 'se']
---

Great thanks to [Christopher Polster](https://github.com/chpolste) who submitted a pull request to include [Xarray interface](https://hn2016-falwa.readthedocs.io/en/latest/Xarray%20Interface.html) to the [QGField class](https://hn2016-falwa.readthedocs.io/en/latest/Object%20Oriented%20Interface.html) of the [local wave activity package](https://github.com/csyhuang/hn2016_falwa). 😄🙏🏻 Please find [here](https://github.com/csyhuang/hn2016_falwa/releases/tag/v0.6.1) the release note for version 0.6.1.

I learned something new cleaning up the GitHub repo and I'm gonna write about it.

## 1. Untrack changes in the repo

There are actually **two places** which you can define the files to be ignored by `git`:

- `.gitignore`: I used to have this locally. Christopher suggested I include that in the GitHub repo for everyone's use, and I think that's a better idea.
- `.git/info/exclude`: This is indeed the right place to specify *personal* files to be excluded (not shared on the repo).

## 2. Skip unit test when optional packages are not installed

In `test_xarrayinterface.py`, I modified the `xarray` import statement:

```python
try:
    import xarray as xr
except ImportError:
    pytest.skip("Optional package Xarray is not installed.", allow_module_level=True)
```

Given that xarray is an optional package, even if it is not installed, unit test for this package shall still run through.

## 3. Move fortran modules into the package directory

The `.f90` files that contain the f2py modules were located in `hn2016_falwa/` before. Now it is moved to `hn2016_falwa/f90_modules/`. The modifications done are:

(1) In `setpy.py`, the extension is changed to something like:

```python
ext4 = Extension(name='interpolate_fields_direct_inv',
                 sources=['hn2016_falwa/f90_modules/interpolate_fields_dirinv.f90'],
                 f2py_options=['--quiet'])
```

The compiled modules of suffix `.so` will be located in `hn2016_falwa/` with this change.

(2) Add in `hn2016_falwa/__init__.py`:

```python
from .interpolate_fields import interpolate_fields
...
from .compute_lwa_and_barotropic_fluxes import compute_lwa_and_barotropic_fluxes
```

## 4. Fix the display of documentation on readthedocs.org

To debug, go to https://readthedocs.org/projects/hn2016-falwa/ and look at the `Builds`. Even if it passes, the document may not be compiled properly. Go to `View raw` and check out all warnings/errors to see if you have a clue.

The fix I have done are:
- In `docs/source/conf.py`:
	- Fix the appended sys path
	- Add modules/packages that causes the compilation to fail/raise warning to `autodoc_mock_imports` in 
- Add `docs/requirements.txt` and specify all external packages imported (i.e. numpy, scipy, xarray) in the package

## 5. Compare commits using GitHub's interface

To display the difference between two commits using GitHub's web interface, put in the following URL:

> https://github.com/csyhuang/hn2016_falwa/compare/OLD-COMMIT-HASH..NEW-COMMIT-HASH

## 6. To pull a branch which other forked your repo to local



