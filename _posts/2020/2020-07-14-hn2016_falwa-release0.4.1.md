---
layout: post
title: Minor release of my python package + release procedures
comments: true
tags: ['se']
---

[[hn2016_falwa Release 0.4.1](https://github.com/csyhuang/hn2016_falwa/releases/tag/0.4.1)] A minor release of my python package [hn2016_falwa](https://github.com/csyhuang/hn2016_falwa) is published. Thanks [Christopher Polster](https://github.com/chpolste) for submitting a pull request that fixes the interface of `BarotropicField`. Moreover, I added procedures to process masked array in `QGField` such that it can be conveniently used to process ERA5 data which is stored as masked array in netCDF files.

As a memo to myself - procedures for a release (which I often forget and have to google 😅):
- [Updated on 2023/11/5] Update version number in:
  - `setup.py`, 
  - `readme.md`
  - `falwa/__init__.py`
  - `docs/source/conf.py`
  - recipe/meta.yaml
- Add a (light-weighted) tag to the commit: `git tag <tagname>`.
- Not only push the commits but also the tag by `git push origin <tagname>`.
- **Update on Aug 15, 2021**: To push the commit and corresponding tag simultaneously, use
```
git push --atomic origin <branch name> <tag>
```

If I have time, I would update the version on PYPI too:
- Create the `dist/` directory and the installation files: `python3 setup.py sdist bdist_wheel`
- Upload the package onto TestPyPI to test deployment: `python3 -m twine upload --repository testpypi dist/*`
- Deploy the package onto PyPI for real: `python3 -m twine upload dist/*`
