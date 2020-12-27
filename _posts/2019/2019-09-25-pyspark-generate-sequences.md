---
layout: post
title: Generate sequence from an array column of pyspark dataframe
comments: true
tags: ['pyspark']
---

Suppose I have a Hive table that has a column of sequences,

```
+------------------+
|          sequence|
+------------------+
|      [3, 23, 564]|
+------------------+
```

how to generate a column that contains permutations of each sequence in multiple rows? The desired output shall look like:

```
+------------------+------------------+
|          sequence|       permutation|
+------------------+------------------+
|      [3, 23, 564]|      [3, 23, 564]|
|      [3, 23, 564]|      [3, 564, 23]|
|      [3, 23, 564]|      [23, 3, 564]|
|      [3, 23, 564]|      [23, 564, 3]|
|      [3, 23, 564]|      [564, 3, 23]|
|      [3, 23, 564]|      [564, 23, 3]|
+------------------+------------------+
```

In order to get multiple rows out of each row, we need to use the function `explode`. First, we write a user-defined function (UDF) to return the list of permutations given a array (sequence):

```python
import itertools
from pyspark.sql import SparkSession, Row
from pyspark.sql.types import IntegerType, ArrayType

@udf_type(ArrayType(ArrayType(IntegerType())))
def permutation(a_list):
    return list(itertools.permutations(a_list, len(a_list)))
```

The `udf_type` function is adapted from the [blog post by John Paton](https://johnpaton.net/posts/clean-spark-udfs/). The output type is specified to be an array of "array of integers".

The application of this function with `explode` will yield the result above:

```python
df = spark_session.createDataFrame([Row(sequence=[3, 23, 564])])

result = df.withColumn('permutation', F.explode(permutation(F.col('sequence'))))
```


