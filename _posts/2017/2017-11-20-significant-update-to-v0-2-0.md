---
layout: post
title: My python library updated to v0.2.0!
tags: ['research']
comments: true
---

I have updated my python library [hn2016_falwa](http://github.com/csyhuang/hn2016_falwa) to 
v0.2.0 (see [release note](https://github.com/csyhuang/hn2016_falwa/releases/tag/v0.2.0)! Now 
it includes functions to compute the contribution of non-conservative forces to wave activity.

Moreover, the [documentation page](http://hn2016-falwa.readthedocs.io/en/latest/) generated 
with Sphinx is now hosted on readthedocs.org! Check it out!

A side note: somehow I made multiple commits to remedy mistake. The git commands to squash 
the (3, for example) commits are:
```
git rebase -i origin/master~3 master
git push origin +master
```

