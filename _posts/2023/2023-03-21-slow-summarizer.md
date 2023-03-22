---
layout: post
title: Aggregation of vectors using Spark Summarizer is too slow. How to get around it?
comments: true
tags: ['sql', 'pyspark']
---

I have a dataframe `df` with columns `id` (integers) and `document` (string): 

```
root
 |-- id: integer
 |-- document: string
```

Each `id` is associated with multiple documents. Each document would be transformed into a vector representation (`embedding` of dimension 100) using [Spark NLP](https://nlp.johnsnowlabs.com/). Eventually, I want to get the average of vectors associated with each `id`.

When testing with small amount of data, i.e. 10k `id` with each associated with \~100 `document`, `pyspark.ml.stat.Summarizer` does the job quickly:

```python
df.groupby('id').agg(Summarizer.mean(F.col('embedding')).alias('mean_vector'))
```

However, the *real situation* is that I have to deal with **Big Data** that consists of 100M distinct `id` and 200M distinct `document`. Each `id` can be associated with at most 30k `document`. The time taken to (1) attach embedding using Spark NLP and (2) aggregate the vectors per `id` took me 10 hours, which is too slow!

Eventually, I figured out a way to do the same thing while having the computing time shortened to \~2 hours.

Thanks to my colleague who spot out the bottleneck - step (1) is indeed not slow. It was step (2) that takes most of the time when there is a huge number of `id` to work with. In this scenario, the aggregation of values in 100 separate columns is actually faster than the aggregation of 100-dimension vectors.

Here is what I do to optimize the procedures:

### 1. Obtain vector representation as array for distinct `document`

One can specify in `sparknlp.base.EmbeddingFinisher` whether you want to output a vector or an array. To make the split easier, I set the output as array:

```python
from sparknlp.base import EmbeddingFinisher
...

finisher = EmbedingFinisher() \
    .setOutputAsVector(False) \
    ...
```

### 2. Split the 100-d vector into 100 columns

Now I have `document_df` of schema

```
root
 |-- document: string
 |-- embedding: array
 |    |-- element: float
```

I split the vectors into 100 columns (v1, v2, ... v100) by:

```python
import pyspark.sql.functions as F

split_to_cols = [F.col('embedding')[i].alias(f'v{i}') for i in range(1, 101)]
document_df = document_df.select([F.col('document')] + split_to_cols)
```

### 3. Join the resultant dataframe with the original and do aggregation per column

I then join `document_df` to `df` and obtain the average of embedding columns per `id`:

```python
avg_expr = [F.mean(F.col(f'v{i}')).alias(f'v{i}') for i in range(1, 101)]
df = df.join(document_df, on='document').groupby('id').agg(*avg_expr)
```

### 4. (If needed) Use VectorAssembler to concatenate the columns into vectors

One can turn the values in the 100 columns to a vector per `id` if needed:

```python
from pyspark.ml import VectorAssembler

assembler = VectorAssembler(
    inputCols=[f'v{i}' for i in range(1, 101)],
    outputCol='final_vector')
df = assembler.transform(df) \
    .select('id', 'final_vector')
```

That's how I speed up the aggregation of Vector output from Spark NLP. Hope that helps. 😉