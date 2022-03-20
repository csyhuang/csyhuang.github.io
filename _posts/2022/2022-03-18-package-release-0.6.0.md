---
layout: post
title: Package release hn2016_falwa v0.6.0
comments: true
image_url: 'https://github.com/csyhuang/csyhuang.github.io/raw/master/assets/img/hn2016_falwa_diagram.png'
tags: ['setup', 'se']
---

Here comes the package release [hn2016_falwa](https://github.com/csyhuang/hn2016_falwa) v0.6.0. This version contains the updated algorithm for reference state inversion - now the reference state is solved with absolute vorticity field at 5N (defined by user) as boundary condition. The analysis code to reproduce results in Neal et al. "The 2021 Pacific Northwest heat wave and associated blocking: Meteorology and the role of an upstream cyclone as a diabatic source of wave activity" (submitted to GRL) can be found in the directory `scripts/nhn_grl2022/`.

Please refer to the [release notes](https://github.com/csyhuang/hn2016_falwa/releases/tag/v0.6.0) for details. 

We are in a hurry to submit our manuscript revision, so this package version is not available on PyPI yet.😅 I will upload it to PyPI asap.
