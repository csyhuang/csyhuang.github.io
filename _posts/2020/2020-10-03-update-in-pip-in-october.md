---
layout: post
title: New pip release and changes in its way to resolve dependency conflicts
tags: ['se', 'hacktoberfest']
comments: true
---

When I was trying to set up a conda environment to run a package...

```
ERROR: After October 2020 you may experience errors when installing or updating packages. This is because pip will change the way that it resolves dependency conflicts.

We recommend you use --use-feature=2020-resolver to test your packages with the new resolver before it becomes the default.

numpydoc 1.1.0 requires sphinx>=1.6.5, but you'll have sphinx 1.5.3 which is incompatible.
```

After googling, I found [this post on StackOverflow](https://stackoverflow.com/questions/63277123/what-is-use-feature-2020-resolver-error-message-with-jupyter-installation-on) explaining the issue - it is related to the changes in [pip's release 20.2](https://discuss.python.org/t/announcement-pip-20-2-release/4863).

To implement the check, install packages with the command

```bash
pip install --use-feature=2020-resolver package
```