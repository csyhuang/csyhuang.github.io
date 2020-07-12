---
layout: post
title: Installing java on Mac
tags: ['setup']
comments: true
---

The information of this post was learnt from [this StackOverflow post](https://stackoverflow.com/questions/38921362/javas-path-still-usr-bin-java-after-brew-cask-install-java) and 
also [David Cai's blog post](http://davidcai.github.io/blog/posts/install-multiple-jdk-on-mac/) on 
how to install multiple Java version on Mac OS High Sierra.
<!--more-->

With ```brew cask``` installed on Mac (see [homebrew-cask instructions](https://github.com/caskroom/homebrew-cask/blob/master/USAGE.md)), 
different versions of java can be installed via the command (I want to install java9 here, for example):

```
brew tap caskroom/versions
brew cask install java9
```

After installing, the symlink ```/usr/bin/java``` is still pointing to the old native Java. You can check 
where it points to with the command ```ls -la /usr/bin/java```. It is probably pointing to the old native 
java path:
```
/System/Library/Frameworks/JavaVM.framework/Versions/Current/Commands/java
```.

However, homebrew installed java into the directory 
```/Library/Java/JavaVirtualMachines/jdkx.x.x_xxx.jdk/Contents/Home```. 

To easily switch between different java environments, you can use ```jEnv```. The installing 
instructions can be found on [jEnv's official page](http://www.jenv.be/).
