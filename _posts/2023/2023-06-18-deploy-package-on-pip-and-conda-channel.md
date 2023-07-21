---
layout: post
title: Deploy package on pip and conda channel
comments: true
tags: ['se']
---

To build pip distribution that contains the source files only (without compiled modules):
```bash
python3 setup.py sdist bdist_wheel
python3 -m twine upload dist/*
```

To compile the package to .whl on Mac (Example: [SciPy pip repository](https://pypi.org/project/scipy/#files)):
```bash
python setup.py bdist_wheel
python3 -m twine upload dist/*
```
This deploy the wheel file `dist/hn2016_falwa-0.7.0-cp310-cp310-macosx_10_10_x86_64.whl` to pip channel.

However, when repeating the Mac procedures above on Linux, I got the error:
```
Binary wheel 'hn2016_falwa-0.7.0-cp311-cp311-linux_x86_64.whl' has an unsupported platform tag 'linux_x86_64'.
```

I googled and found this StackOverflow thread: [Binary wheel can't be uploaded on pypi using twine](https://stackoverflow.com/questions/59451069/binary-wheel-cant-be-uploaded-on-pypi-using-twine).

ManyLinux repo: https://github.com/pypa/manylinux

# Try pulling docker to build

```bash
docker pull quay.io/pypa/manylinux_2_28_x86_64
```

After git clone the repository, navigate into it and run:
```
PLATFORM=$(uname -m) POLICY=manylinux2014 COMMIT_SHA=latest ./build.sh
```
(Not using steps above)

Good tutorial:

http://www.martin-rdz.de/index.php/2022/02/05/manually-building-manylinux-python-wheels/


---

Create fresh start environment:
```
$ conda create --name test_new
```

But using conda on mac to compile wheel would have this issue:

```
ld: unsupported tapi file type '!tapi-tbd' in YAML file
```

Related thread: https://developer.apple.com/forums/thread/715385
---
Create virtual environment (not via conda)
```
python3 -m venv /Users/claresyhuang/testbed_venv
source /Users/claresyhuang/testbed_venv/bin/activate
```


