---
layout: post
title: Important announcement about the GitHub repo hn2016_falwa
comments: true
image_url: 'https://github.com/csyhuang/csyhuang.github.io/raw/master/assets/img/hn2016_falwa_diagram.png'
tags: ['research', 'se']
---

Below is the email I sent to the users of GitHub repo hn2016_falwa:

I am writing to inform you two recent releases of the GitHub repo v1.0.0 (major release) and v1.1.0 (a bug fix). You can refer to the release notes for the details. There are two important changes in v1.0.0:

1. The python package is renamed from hn2016_falwa to falwa since this package implements finite-amplitude wave activity and flux calculation beyond those published in Huang and Nakamura (2016). The GitHub repo URL remains the same: [https://github.com/csyhuang/hn2016_falwa/](https://github.com/csyhuang/hn2016_falwa/) . The package can be installed via pip as well: [https://pypi.org/project/falwa/](https://pypi.org/project/falwa/)

2. It happens that the bug fix release v0.7.2 has a bug in the code such that it over-corrects the nonlinear zonal advective flux term. v1.0.0 fix this bug. Thanks Christopher Polster for spotting the error. The fix requires re-compilation of fortran modules.

The rest of the details can be found in the release notes:
- v1.0.0: [https://github.com/csyhuang/hn2016_falwa/releases/tag/v1.0.0](https://github.com/csyhuang/hn2016_falwa/releases/tag/v1.0.0)
- v1.1.0: [https://github.com/csyhuang/hn2016_falwa/releases/tag/v1.1.0](https://github.com/csyhuang/hn2016_falwa/releases/tag/v1.1.0)

Please let us know on issue page if you have any questions:
[https://github.com/csyhuang/hn2016_falwa/issues](https://github.com/csyhuang/hn2016_falwa/issues)