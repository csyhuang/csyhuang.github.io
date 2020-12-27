---
layout: post
title: Conversion of pandas dataframe to pyspark dataframe with an older version of pandas
comments: true
tags: ['pyspark']
---

Pandas dataframe can be converted to pyspark dataframe easily in the newest version of `pandas` after `v0.19.2`. If you are using an older version of pandas, you have to do a bit more work for such conversion as follows.

First, load the packages and initiate a spark session.

```python
from pyspark.sql import SparkSession, DataFrame, Row
import pandas as pd

spark = SparkSession.builder \
        .master("local") \
        .appName("Pandas to pyspark DF") \
        .getOrCreate()
```
<!--more-->
Here is an example of pandas dataframe to be converted.

```python
df = pd.DataFrame({'index':[i for i in range(7)],
                   'alphabet':[i for i in 'pyspark']})
df.head(7)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>alphabet</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>p</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>y</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>s</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>p</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>a</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>r</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>k</td>
    </tr>
  </tbody>
</table>
</div>

To convert it to a `pyspark` dataframe, one has to create a list of `Row` objects and pass it into `createDataFrame`:

```python
df_pyspark = spark.createDataFrame([
    Row(index=row[1]['index'], alphabet=row[1]['alphabet'])
    for row in df.iterrows()
])
df_pyspark.show()
```

    +--------+-----+
    |alphabet|index|
    +--------+-----+
    |       p|    0|
    |       y|    1|
    |       s|    2|
    |       p|    3|
    |       a|    4|
    |       r|    5|
    |       k|    6|
    +--------+-----+
    

