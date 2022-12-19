---
layout: post
title: Package release hn2016_falwa v0.5.0
comments: true
image_url: 'https://github.com/csyhuang/csyhuang.github.io/raw/master/assets/img/hn2016_falwa_diagram.png'
tags: ['setup', 'se']
---

Here comes the [package release hn2016_falwa v0.5.0](https://github.com/csyhuang/hn2016_falwa/releases/tag/v0.5.0). Please refer to the [release notes](https://github.com/csyhuang/hn2016_falwa/releases/tag/v0.5.0) for details. Great thanks to [Christopher Polster](https://github.com/chpolste) who submitted a pull request to optimize the fortran modules, which makes the flux calculations a lot faster. 😄

The package is available on [PyPI](https://pypi.org/project/hn2016-falwa/0.5.0/) as well.

The CI procedures of the package has been migrated from Travis CI to GitHub workflow. I spent a while on fixing related issues, so I wrote [a blog post]({% post_url 2021/2021-08-14-ci-migration-to-github-workflow %}) about the procedures. Hopefully this can save others time if they want to do the same thing.