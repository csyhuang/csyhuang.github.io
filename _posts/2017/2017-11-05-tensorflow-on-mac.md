---
layout: post
title: Compiling tensorflow on Mac with SSE, AVX, FMA etc.
tags: ['setup']
comments: true
---

(Ideally, I shall run tensorflow somewhere else rather than on my MacBook.)

When I install keras with Anaconda on my Mac OS X, with tensorflow as the backend, the following warning comes up when
 running the sample script:
```
I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
```


To use those instructions (*SSE4.1 SSE4.2 AVX AVX2 FMA*), tensorflow has to be compiled from source. The instructions are available [here](https://www.tensorflow.org/install/install_sources). 
Using the following command to build the source:
```bash
bazel build -c opt --copt=-march=native --copt=-mfpmath=both --config=cuda -k //tensorflow/tools/pip_package:build_pip_package
```
I got the following error:
```
Problem with java installation: couldn't find/access rt.jar in /Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/Contents/Home
```
It happens that *rt.jar* is not present in Java 9. To solve it, install a version of Java 8:
```bash
brew cask install caskroom/versions/java8
```
and then specify the path of Java 8 (Change the version number '1.8.0_162' to that of the version you installed):
```bash
export JAVA_HOME=$(/usr/libexec/java_home -v 1.8.0_162)
```
Afterwards, I try to build tensorflow from source again, and it successfully includes the instructions above.
