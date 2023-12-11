---
layout: post
title: Deployment onto Conda forge
comments: true
image_url: ''
tags: ['setup', 'se']
---

The deployment of python package on linux is not working (again). I am exploring solutions to automate deployment. Here are things I've found.

- Documentation of conda forge: [https://conda-forge.org/docs/](https://conda-forge.org/docs/)
- An example of *feedstock* of repo using F2PY: [https://github.com/conda-forge/fastscapelib-f2py-feedstock](https://github.com/conda-forge/fastscapelib-f2py-feedstock)
- Conda-forge repo of `fastscapelib-f2py`: [https://anaconda.org/conda-forge/fastscapelib-f2py/](https://anaconda.org/conda-forge/fastscapelib-f2py/)

\[Updated on 2023/12/11\] After some research, it seems that `scikit-build` would be a continuously maintained solution: [https://scikit-build.readthedocs.io/](https://scikit-build.readthedocs.io/)

*to be continued.*
