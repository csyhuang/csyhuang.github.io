---
layout: post
title: Running pytest coverage check locally
comments: true
image_url: ''
tags: ['se']
---

I wrote a [blog post in 2021]({% post_url 2021-08-14-ci-migration-to-github-workflow %}) about how to integrate pytest coverage check to GitHub Workflow.

To run `coverage` locally, execute `coverage run --source=falwa -m pytest tests/ && coverage report -m` would yield the report (this is from the PR for `falwa` release 1.3):

```
Name                           Stmts   Miss  Cover   Missing
------------------------------------------------------------
falwa/__init__.py                 11      0   100%
falwa/barotropic_field.py         41      4    90%   79, 86, 93, 138
falwa/basis.py                    66      8    88%   57-62, 175, 186
falwa/constant.py                  6      0   100%
falwa/data_storage.py            146      3    98%   52, 59, 107
falwa/legacy/__init__.py           0      0   100%
falwa/legacy/beta_version.py     240    240     0%   1-471
falwa/netcdf_utils.py              9      9     0%   6-30
falwa/oopinterface.py            400     32    92%   297, 320, 336, 355, 366, 393, 550, 559, 721, 731, 734, 799, 818, 860, 870, 880, 890, 907, 918, 929, 993, 1006-1008, 1019, 1031, 1179-1180, 1470-1471, 1550-1565
falwa/plot_utils.py              125    125     0%   6-343
falwa/preprocessing.py             7      7     0%   6-30
falwa/stat_utils.py               11     11     0%   6-26
falwa/utilities.py                61     48    21%   58-92, 151-186, 242-255
falwa/wrapper.py                 146    146     0%   6-570
falwa/xarrayinterface.py         266     37    86%   107-109, 317, 322-323, 478, 616-648, 683-704
------------------------------------------------------------
TOTAL                           1535    670    56%
```

