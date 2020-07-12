---
layout: post
title: Useful Git commands at work
tags: ['se']
comments: true
---

Below are solutions I curated online to solve problems related to Git when collaborating with others and working on several branches together. *This post will be updated from time to time.*
<!--more-->
### Copy a file from one branch to another
To copy a file to the current branch from another branch ([ref](https://stackoverflow.com/questions/307579/how-do-i-copy-a-version-of-a-single-file-from-one-git-branch-to-another/7099164))
```
git checkout another_branch the_file_you_want.txt
```

### Merge changes from master branch to yours
To merge changes from another branch to yours, you can use `merge` or `rebase` depending on the preferred commit order. BitBucket has a [nice tutorial](https://www.atlassian.com/git/tutorials/merging-vs-rebasing) discussing the difference btween the two. Usually I'd love to have the changes pulled from another branch as a single commit with `git merge`.

```
git checkout master      # the branch with changes
git pull                 # pull the remote changes to local master branch
git checkout mybranch    # go back to mybranch
git merge master         # incorporate the changes into mybranch
```

### How to use `git revert`
[Useful tutorial](https://www.atlassian.com/git/tutorials/undoing-changes/git-revert) from BitBucket on `git revert`.

### Compare differences between branches and output the results
If you want to compare the difference between your (more updated) branch and the master branch, use the command
```
git diff master..your_branch
```
You can save the comparison results into a text file with colors by
```
git diff master..your_branch > your_branch_to_master.diff
```
The color can be viewed when you open the `.diff` file with [Sublime](https://www.sublimetext.com/).


### Update password for Git on Mac OS X
use the following command
```
git config --global credential.helper osxkeychain
```

[to be continued]




