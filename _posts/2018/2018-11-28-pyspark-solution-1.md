---
layout: post
title: Simple pyspark solutions
comments: true
tags: ['pyspark']
---

Here is a curation of some solutions to simple problems encountered when working with pyspark.

### How to replace string in a column?

[Source](https://stackoverflow.com/questions/37038014/pyspark-replace-strings-in-spark-dataframe-column)

```python
from pyspark.sql.functions import *
new_df = df.withColumn(col_name, regexp_replace(col_name, pattern, replacement))
```

### How to avoid duplicate columns when joining two dataframe on columns with the same name?

[Source](https://docs.databricks.com/spark/latest/faq/join-two-dataframes-duplicated-column.html)

```python
df = left_df.join(right_df, ["name"])
```
