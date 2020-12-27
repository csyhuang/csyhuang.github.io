---
layout: post
title: Ch.6 - Working with different types of Data
tags: ['sparkdefguide']
order: 6
comments: true
---

Only points that needed attention is jotted here.

# Working with Dates and Timestamps

- Spark's `TimestampType` class supports only *second-level precision*. If one wants a precision up to milliseconds or microseconds, one has to potentially operate on them as `longs`.

## Example: using `F.current_date()` and `F.current_timestamp()`

```python
date_df = spark.range(10)\
  .withColumn("today", F.current_date())\
  .withColumn("now", F.current_timestamp())
date_df.show()
```

would give

```
+---+----------+--------------------+
| id|     today|                 now|
+---+----------+--------------------+
|  0|2020-11-30|2020-11-30 15:46:...|
|  1|2020-11-30|2020-11-30 15:46:...|
|  2|2020-11-30|2020-11-30 15:46:...|
|  3|2020-11-30|2020-11-30 15:46:...|
|  4|2020-11-30|2020-11-30 15:46:...|
|  5|2020-11-30|2020-11-30 15:46:...|
|  6|2020-11-30|2020-11-30 15:46:...|
|  7|2020-11-30|2020-11-30 15:46:...|
|  8|2020-11-30|2020-11-30 15:46:...|
|  9|2020-11-30|2020-11-30 15:46:...|
+---+----------+--------------------+
```

The schema (obtained by `date_df.printSchema()`) looks like

```
root
 |-- id: long (nullable = false)
 |-- today: date (nullable = false)
 |-- now: timestamp (nullable = false)
```

## Example: using `F.date_add()` and `F.date_sub()`

```python
date_df.select(
  F.date_sub("today", 5).alias("5-days-ago"),
  F.date_add("today", 5).alias("5-days-later"))
```

would give
```
+----------+------------+
|5-days-ago|5-days-later|
+----------+------------+
|2020-11-25|  2020-12-05|
     ...         ...
|2020-11-25|  2020-12-05|
+----------+------------+

```

or in SQL

```SQL
%sql
SELECT date_add(today, 5) AS `5-days-ago` FROM date_df
LIMIT 1
```
would give
```
+----------+
|5-days-ago|
+----------+
|2020-11-25|
+----------+
```

## Difference in date/months: `datediff` and `months_between`

- Syntax: `datediff(col_1, col_2)`
- Spark would return `null` if it cannot parse a date
- To parse a string to date, one can use: `F.to_date(...)`.

Example:

```python
date_df.select(
  F.to_date(F.lit('2012-20-12')).alias('invalid'),
  F.to_date(F.lit('2012-12-20')).alias('valid'))
```

would give 
```
+-------+----------+
|invalid|     valid|
+-------+----------+
|   null|2012-12-20|
+-------+----------+
```

## Fixing invalid dates - specific our date format
- `to_date(column, format)`, e.g. `F.to_date(F.lit("2017-20-12"), "yyyy-dd-MM")`
- `to_timestamp(column, format)`, e.g. `F.to_timestamp(F.lit("2017-20-12"), "yyyy-dd-MM")`

# Working with Nulls in Data

- Use `na` subpackage of `DataFrame`
- Important note: when we declare a column as NOT having a null time, that is **not** actually *enforced*. The nullable signal is simply to help Spark SQL optiimize for handling that column.

## Functions that can be used to deal with nulls
- `coalesce`
- `ifnull`, `nullif`, `nvl`, `nvl2`
- `df.na.drop(how, thresh, subset)` or `df.dropna()` drop the whole row if any element in the row contains NULL. Options for how are (`'any'`, `'all'`)
- `df.filter(df.col("col").isNotNull())` you drop those rows which have null only in the column `col`. One can alternatively use `df.na.drop(subset=["col"])` to achieve the same results
- `df.na.fill(value, subset)` or `df.fillna(value, subset)`
- `df.na.replace(to_replace, value, subset=None)` -> this is to replace value that are not null when df has null values, e.g.

```python
df3 = spark.createDataFrame([Row(a=2, b=3), Row(a=None, b=5), Row(a=4, b=None)])
df3.na.replace(to_replace=[3], value=[33]).show()
```
gives
```
+----+----+
|   a|   b|
+----+----+
|   2|  33|
|null|   5|
|   4|null|
+----+----+
```
## Ordering
- `asc_nulls_first`, `asc_nulls_last`, `desc_nulls_first`, `desc_ulls_last`

# Working with Complex Types (P. 111)

## Structs

```python
complex_df = df.select(F.struct("Description", "InvoiveNo").alias("complex")
```

To bring up all values in the struct, use `complex_df.select("complex.*")`.

## Arrays
### split
```python
F.split(...)
```
### Array Length
```python
F.size(...)
```
### array_contains
```python
F.array_contains(col, value)
```
### explode
This is used to explode an array column.
```python
df.withColumn("exploded", F.explode(F.col("splitted")))
```
In SQL, it reads like
```sql
SELECT ... FROM ...
LATERAL VIEW explode(splitted) AS exploded
```

### Maps
Python syntax: `pyspark.sql.functions.create_map(*cols)`
```python
df.select(create_map('name', 'age').alias("map")).collect()
```
returns
```
[Row(map={'Alice': 2}), Row(map={'Bob': 5})]
```

In SQL, the function is `map`. It reads like
```sql
SELECT map(Description, InvoiceNo) AS complex_map FROM dfTable
WHERE Description IS NOT NULL
```

**Attention:** read more from documentation about the map-related functions.

## Working with JSON

**Attention:** come back to JSON later!! 







