---
layout: post
title: Read libsvm files into PySpark dataframe
comments: true
tags: ['pyspark']
---

I wanted to load the libsvm files provided in [tensorflow/ranking](https://github.com/tensorflow/ranking) into PySpark dataframe, but couldn't find existing modules for that. Here is a version I wrote to do the job. (Disclaimer: not the most elegant solution, but it works.)
<!--more-->
First of all, load the pyspark utilities required.

```python
from pyspark import SparkContext
from pyspark.sql import SparkSession, Row
from pyspark.ml.linalg import SparseVector
```

Initiate a spark session for creation of dataframe.

```python
sc = SparkContext("local", "read_libsvm")
spark_session = SparkSession \
    .builder \
    .appName("read_libsvm") \
    .getOrCreate()
```

Get the path to the data files.

```python
_TRAIN_DATA_PATH="data/train.txt"
_TEST_DATA_PATH="data/test.txt"
```

Here is the module I wrote for the purpose:

```python
def read_libsvm(filepath, query_id=True):
    '''
    A utility function that takes in a libsvm file and turn it to a pyspark dataframe.

    Args:
        filepath (str): The file path to the data file.
        query_id (bool): whether 'qid' is present in the file.

    Returns:
        A pyspark dataframe that contains the data loaded.
    '''

    with open(filepath, 'r') as f:
        raw_data = [x.split() for x in f.readlines()]

    train_outcome = [int(x[0]) for x in raw_data]

    if query_id:
        train_qid = [int(x[1].lstrip('qid:')) for x in raw_data]

    index_value_dict = list()
    for row in raw_data:
        index_value_dict.append(dict([(int(x.split(':')[0]), float(x.split(':')[1]))
                                       for x in row[(1 + int(query_id)):]]))

    max_idx = max([max(x.keys()) for x in index_value_dict])
    rows = [
        Row(
            qid=train_qid[i],
            label=train_outcome[i],
            feat_vector=SparseVector(max_idx + 1, index_value_dict[i])
        )
        for i in range(len(index_value_dict))
    ]
    df = spark_session.createDataFrame(rows)
    
    return df

```

Let's see how the train and test sets look like in the tf-ranking package:

```python
train_df = read_libsvm(_TRAIN_DATA_PATH)
test_df = read_libsvm(_TEST_DATA_PATH)

train_df.show(5)
```

    +--------------------+-----+---+
    |         feat_vector|label|qid|
    +--------------------+-----+---+
    |(137,[5,13,17,42,...|    0|  1|
    |(137,[11,13,18,30...|    2|  1|
    |(137,[11,27,29,39...|    2|  1|
    |(137,[5,10,26,31,...|    1|  1|
    |(137,[13,17,22,24...|    2|  2|
    +--------------------+-----+---+
    only showing top 5 rows

```python
test_df.show(5)
```

    +--------------------+-----+---+
    |         feat_vector|label|qid|
    +--------------------+-----+---+
    |(137,[2,7,37,40,4...|    1|  1|
    |(137,[1,8,12,15,2...|    2|  1|
    |(137,[4,11,15,16,...|    0|  1|
    |(137,[14,19,20,33...|    0|  1|
    |(137,[9,12,19,26,...|    1|  2|
    +--------------------+-----+---+
    only showing top 5 rows
    

