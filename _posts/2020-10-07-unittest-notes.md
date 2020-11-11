---
layout: post
title: Running a single test case in the unittest suite
tags: ['se', 'hacktoberfest']
comments: true
---

If your unit tests are written using the `unittest` package, to run a *single* test case in the TestSuite, the command line syntax is

```bash
python setup.py test -m tests.test_module.TestClass.test_case
```

Stack Overflow reference: [Does unittest allow single case/suite testing through “setup.py test”?](https://stackoverflow.com/questions/21167516/does-unittest-allow-single-case-suite-testing-through-setup-py-test)

If your unit tests are written using `pytest`, the command used would be

```bash
pytest tests/test_module.py::test_case
```
