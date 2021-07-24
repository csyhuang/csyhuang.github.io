---
layout: post
title: Split a vector/list in a pyspark DataFrame into columns
comments: true
tags: ['pyspark']
---

<!--more-->

## Split an array column
To split a column with arrays of strings, e.g. a DataFrame that looks like,
```
+---------+
|   strCol|
+---------+
|[A, B, C]|
+---------+
```
into separate columns, the following code without the use of UDF works.
```python
import pyspark.sql.functions as F

df2 = df.select([F.col("strCol")[i] for i in range(3)])
df2.show()
```
Output:
```
+---------+---------+---------+
|strCol[0]|strCol[1]|strCol[2]|
+---------+---------+---------+
|        A|        B|        C|
+---------+---------+---------+
```

## Split a vector column
To split a column with doubles stored in DenseVector format, e.g. a DataFrame that looks like,
```
+-------------+
|       intCol|
+-------------+
|[2.0,3.0,4.0]|
+-------------+
```
one have to construct a UDF that does the convertion of DenseVector to array(python list) first:
```python
import pyspark.sql.functions as F
from pyspark.sql.types import ArrayType, DoubleType

def split_array_to_list(col):
    def to_list(v):
        return v.toArray().tolist()
    return F.udf(to_list, ArrayType(DoubleType()))(col)

df3 = df.select(split_array_to_list(F.col("intCol")).alias("split_int"))\
    .select([F.col("split_int")[i] for i in range(3)])
df3.show()
```
Output:
```
+------------+------------+------------+
|split_int[0]|split_int[1]|split_int[2]|
+------------+------------+------------+
|         2.0|         3.0|         4.0|
+------------+------------+------------+
```