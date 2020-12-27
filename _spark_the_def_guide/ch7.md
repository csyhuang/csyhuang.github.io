---
layout: post
title: Ch.7 - Aggregations
tags: ['sparkdefguide']
order: 8
comments: true
---


# Aggregation Functions 

## count
- Note when operating on whole DataFrame, all rows are counted
- But when operating on a column, nulls are discarded

## countDistinct

## approx_count_distinct

```python
pyspark.sql.functions.approx_count_distinct(col, rsd=None)
# rsd – maximum relative standard deviation allowed (default = 0.05). For rsd < 0.01, it is more efficient to use countDistinct()
```

## first and last
```python
df.select(F.first(col), F.last(col))
```

## min and max

## sum

## sumDistinct

## avg

## variance and standard deviation
```python
from pyspark.sql.functions import var_pop, standdev_pop, var_samp, stddev_samp
```

## skewness and kurtosis
- *Skewness* measures the *asymmetry* of the value in my data around the mean
- *Kurtosis* measures the tail of the data
```python
from pyspark.sql.functions import skewness, kurtosis
```

## Covariance and Correlation
```python
from pyspark.sql.functions import corr, cov_samp, cov_pop
```

# Aggregating to complex types

```python
df.agg(F.collect_set(col), F.collect_list(col)) # returns array columns
```

or in SQL

```sql
SELECT collect_set(col), collect_list(col) FROM df_table
```

# Grouping

```python
df.groupBy("InvoiceNo").agg(F.count("Quantity".alias("quan")), F.expr("count(Quantity)")) # returns the same thing
```

## Grouping with Maps
```python
df.groupBy("InvoiceNo").agg(F.expr("avg(Quantity)"), F.expr("stddev_pop(Quantity)"))
```
or in SQL
```sql
SELECT avg(Quantity), stddev_pop(Quantity) FROM df
GROUP BY InvoiceNo
```

# Window functions

(come back later...)