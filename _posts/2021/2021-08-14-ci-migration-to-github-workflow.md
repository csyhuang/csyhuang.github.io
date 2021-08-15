---
layout: post
title: Migrating CI from Travis CI to GitHub Workflow
comments: true
image_url: 'https://travis-ci.com/images/logos/TravisCI-Mascot-pride.png'
tags: ['setup', 'se']
---

Few days ago, I realized that the coverage test for [my python package](https://github.com/csyhuang/hn2016_falwa), which was run on [travis-ci.org](https://travis-ci.org), has stopped running for two years. After migrating the to [travis-ci.com](https://www.travis-ci.com/), the service is no longer free after spending 10000 credits. One has to pay for **$69/mo** 💸 for unlimited service.

Note that I already used 2170 credits when testing various things and preparing for upcoming package release in a day 😱(my fault?), so I don't think a free plan is sustainable, but $69/mo is definitely too expensive if I only have to maintain a single package.

I found a free solution on GitHub marketplace: [CodeCov](https://github.com/marketplace/actions/codecov). With this GitHub Action, one can run deployment test and code coverage diagnostic automatically when pushing commits. I spent a bit of time on trial and error, so I think it is worth the time to write down what I did to save time for others.

## Define the workflow

The GitHub Workflow is defined in the file `.github/workflows/workflow.yml`. [Here](https://github.com/csyhuang/hn2016_falwa/blob/master/.github/workflows/workflow.yml) is what I did for my package:
<!--more-->

```yml
name: CodeCov
on: [push, pull_request]
jobs:
  run:
    runs-on: ubuntu-latest
    env:
      OS: ubuntu-latest
      PYTHON: '3.7'
    steps:
    - uses: actions/checkout@v2
      with:
        fetch-depth: ‘2’

    - name: Setup Python
      uses: actions/setup-python@master
      with:
        python-version: 3.7
    - name: Generate Report
      run: |
        pip install coverage
        pip install -U pytest
        pip install --upgrade pip setuptools wheel
        sudo apt-get install gfortran
        pip install numpy
        pip install scipy
        coverage run setup.py pytest
    - name: Upload Coverage to Codecov
      run: |
        bash <(curl -s https://codecov.io/bash)

```
The procedures under `"Generate Report"` are copied from my Travis job definition `.travis.yml`:
```yml
language: python
python:
  - "3.7"

before_install:
  - python --version
  - pip install -U pip
  - pip install -U pytest
  - pip install coverage

install:
  - pip install --upgrade pip setuptools wheel
  - sudo apt-get install python-numpy python-scipy gfortran
  - pip install numpy
  - pip install scipy

# command to run tests
script:
  - coverage run setup.py pytest

after_success:
  - bash <(curl -s https://codecov.io/bash)
``` 

In order to upload the coverage report to CodeCov, I used the command `bash <(curl -s https://codecov.io/bash)` after failing to send report using the line `uses: codecov/codecov-action@v2` from CodeCov template.

## Specify files to include/exclude in the coverage test

The specifications are made in the file `.coveragerc`:

```
[run]
source=hn2016_falwa

[report]
show_missing = True
omit =
    hn2016_falwa/legacy/*
```

Since the modules in the folder `legacy/` is no longer maintained, I exclude that from the coverage test.

## Implement coverage test locally

If you have the python package `coverage` installed, you can test the coverage run command, e.g.
```
$ coverage run setup.py pytest
```
which will generate a coverage report `.coverage`. To visualize the report, use the command
```
$ coverage report
```
to render `.coverage` in readable manner. As an example, the coverage report of my package looks like:
```
Name                               Stmts   Miss  Cover   Missing
----------------------------------------------------------------
hn2016_falwa/__init__.py               0      0   100%
hn2016_falwa/barotropic_field.py      39      4    90%   72, 79, 86, 131
hn2016_falwa/basis.py                 46      2    96%   113, 124
hn2016_falwa/constant.py               6      0   100%
hn2016_falwa/oopinterface.py         270     28    90%   151, 222, ...
hn2016_falwa/utilities.py             61     48    21%   53-87, 146-181, 237-250
hn2016_falwa/wrapper.py              154    154     0%   1-579
----------------------------------------------------------------
TOTAL                                576    236    59%
```


## Include Build badge and Coverage badge in README of your repo

As an example, the image path to the build status of my package is:
```
https://github.com/csyhuang/hn2016_falwa/actions/workflows/workflow.yml/badge.svg
```
![Build Status](https://github.com/csyhuang/hn2016_falwa/actions/workflows/workflow.yml/badge.svg)
I can check my build status [here](https://github.com/csyhuang/hn2016_falwa/actions).

The image path to my CodeCov results is:
```
https://codecov.io/gh/csyhuang/hn2016_falwa/branch/master/graph/badge.svg
```
![codecov.io](https://codecov.io/gh/csyhuang/hn2016_falwa/branch/master/graph/badge.svg)
The report is visualized on CodeCov [like this](https://app.codecov.io/gh/csyhuang/hn2016_falwa).


