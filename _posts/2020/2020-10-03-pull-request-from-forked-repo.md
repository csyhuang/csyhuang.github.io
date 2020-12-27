---
layout: post
title: Submitting pull request from forked repo to main repo
tags: ['se', 'hacktoberfest']
comments: true
---

It's [HacktoberFest](https://hacktoberfest.digitalocean.com/) again! 👻 Here are some useful commands to merge your forked repo with upstream changes from the original repo (Also see [GitHub Docs](https://docs.github.com/en/free-pro-team@latest/github/collaborating-with-issues-and-pull-requests/syncing-a-fork)).

After forking, specify remote upstream repository:

```bash
git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
```

You can then check the upstream locations via the command `git remote -v`.

Before submitting a pull request, you have to make sure your branch contains all the commits from upstream. You can do so by:
```bash
git fetch upstream
git checkout master
git merge upstream/master
```

🤓 Have fun coding! ⌨️
