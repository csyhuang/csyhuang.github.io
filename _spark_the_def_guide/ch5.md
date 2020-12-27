---
layout: post
title: Ch.5 - Basic Structured Operations
tags: ['sparkdefguide']
order: 5
comments: true
---

# Partitioning
- *Partitioning*: DataFrame/Dataset's physical distribution across the cluster
- The *partitioning scheme* defines how that is allocated.
- It can be set (1) based on values in a certain column or (2) nondeterministically.

# Schema

- *schema-on-read*: let a data source define the schema
- *schema-on-read* may cause some precision issue, e.g. long type incorrectly set as integer
- Better define the schema *manually* in production and ETL, esp. when working with untyped data sources (CSV, JSON)

## Example: reading in JSON file with python

```python
spark.read.format("json").load(FILEPATH).schema
```

would return the following

```
StructType([
	StructField(COL_NAME_1, StringType, True),
	StructField(COL_NAME_2, StringType, True),
	StructField(COL_NAME_3, LongType, True)
])
```

- A schema is a `StructType` (Spark's complex types) with a number of fields, `StructField`.
- The last boolean argument speciifies whether the column can contain missing/null values.

To create and enforce a schema:

```python
my_manual_schema = StructType([
	StructField(COL_NAME_1, StringType, True),
	StructField(COL_NAME_2, StringType, True),
	StructField(COL_NAME_3, LongType, True)])
df = spark.read.format("json").schema(my_manual_schema).load(FILEPATH)
```

# Columns and Expressions

- Operations such as select, manipulate and remove a columns from a DataFrames are represented as *expressions*.
- One cannot manipulate an individual column outside the context of a DataFrame.

## Columns

```python
import pyspark.sql.functions as F
F.col('someColumn')
```

- Columns are *not resolved* until we compare the column names with those maintained in the *catalog*.
- Column and table resolution happens in the *analyzer* phase.

## Expression

- An *expression* is a set of transformations on one or more values in a record in a DataFrame.
- It is like a function that takes as colnames as input, resolves them, and then potentially applies more expression to create a single value for each record in the dataset
- "Single vlaue" can be a complex type (`Map` or `Array`)

### Equivalent expressions

- `F.expr("someCol - 5")`
- `F.col("someCol") - 5`
- `F.expr("someCol") - 5`

### Accessing a DataFrame's columns
```python
spark.read.format("json").load(FILEPATH).columns
```

# Records and Rows

- Each row in a DataFrame is a single record (`Row` object).
- Row objects internally represent *arrays of bytes* (byte array interface is never shown to users).

To get a row:
```python
df.first()
```

## Creating Rows

```
from pyspark.sql import Row

my_row = Row("Hello", None, 1, False)
my_row = Row(name='Hello', val=1, yes=True, others=None)
```

## DataFrame Transformation
- add rows or columns
- remove rows or columns
- transform a row into a column (or vice versa)
- change the order of rows based on the values in columns

To create a DataFrame out of a `Row` object with a specfic schema:
```python
my_schema = StructType([
  StructField("some", StringType(), True),
  StructField("col", StringType(), True),
  StructField("Names", LongType(), False)
])
myRow = Row(some="Hello", col=None, Names=1)
df3 = spark.createDataFrame([myRow], schema=my_schema)
df3.show()
```

## Select and SelectExpr

### Example: select all columns and creating a new boolean column

```python
df2.selectExpr(
	"*",
	"(DEST_COUNTRY_NAME = ORIGIN_COUNTRY_NAME) AS withinCountry"
).filter("withinCountry = True").show()
```
would give
```
+-----------------+-------------------+------+-------------+
|DEST_COUNTRY_NAME|ORIGIN_COUNTRY_NAME| count|withinCountry|
+-----------------+-------------------+------+-------------+
|    United States|      United States|370002|         true|
+-----------------+-------------------+------+-------------+
```

### Example: calculate average and number of distinct values in a column

```python
df2.selectExpr("avg(count)", "count(distinct(DEST_COUNTRY_NAME))").show(2)
```

would give 

```
+-----------+---------------------------------+
| avg(count)|count(DISTINCT DEST_COUNTRY_NAME)|
+-----------+---------------------------------+
|1770.765625|                              132|
+-----------+---------------------------------+
```

## Converting to Spark Types (Literals)

```python
df.select(F.lit(1).alias('one')).limit(1).show()
```
would give
```
+---+
|one|
+---+
|  1|
+---+
```

## Adding Columns

```python
df2.withColumn("withinCountry", F.expr("DEST_COUNTRY_NAME == ORIGIN_COUNTRY_NAME")).limit(1)
# or equivalently
df2.withColumn("withinCountry", F.col("DEST_COUNTRY_NAME") == F.col("ORIGIN_COUNTRY_NAME")).limit(1)
```
would give
```
+-----------------+-------------------+-----+-------------+
|DEST_COUNTRY_NAME|ORIGIN_COUNTRY_NAME|count|withinCountry|
+-----------------+-------------------+-----+-------------+
|    United States|            Romania|   15|        false|
+-----------------+-------------------+-----+-------------+
```

## Renaming Columns
```python
df2.withColumnRenamed(existing="ORIGIN_COUNTRY_NAME", new="ORIGIN")
```

## Reserved Characters and Keywords

### Use of backticks

- If there are space between column names, one has to use backticks.
```python
df_w_long_col = df2.withColumn("this long column name", F.expr("DEST_COUNTRY_NAME"))
df_w_long_col.selectExpr("`this long column name` AS colname").show()
```
- One only need to escape expressions that use reserved characters or keywords.

### Case sensitivity
- Spark is case *insensitive*
- To make spark case sensitive in SQL: `set spark.sql.caseSensitive true` (in SQL)

## Removing Columns
```python
df.drop("ORIGIN_COUNTRY_NAME", "count")
```

## Changing a Column's Type (`cast`)

In python:
```python
df.withColumn("count2", F.col("count").cast("long"))
```

In SQL:
```sql
SELECT cast(count as long) AS count2 FROM df
```

## Filtering Rows

In python:
```python
df.filter(F.col("count") < 2)
df.where("count < 2")
```

In SQL:
```sql
SELECT * FROM df WHERE count < 2
```

## Getting Unique Rows

```python
df2.select("ORIGIN_COUNTRY_NAME").distinct()
```
By the way, `df2.select("ORIGIN_COUNTRY_NAME").distinct().count()` and `df2.groupby("ORIGIN_COUNTRY_NAME").sum().count()` gives the same results.

## Random Samples

```python
df.sample(withReplacement=False, fraction=0.02, seed=42)
```

## Random Splits

```python
df.randomSplit([0.2, 0.8], seed=42)
```

## Concatenating and Appending Rows (Union)

- Union are currently performed based on **location**, *not on the schema*. Columns may not automatically line up the way you think they might.

```python
df.union(df2)
```

## Sorting Rows
- This can be done equivalently with `sort` and `orderBy`.
```python
df.sort(F.col('count').desc())
df2.orderBy(F.col('count').desc())  # they are equivalent
```
On top of `desc()` and `asc()`, there are also `desc_nulls_first()`, `desc_nulls_last()`, etc.

- Optimization may be achieved to sort within each partition before another set of transformation using `sortWithPartitions`.

## Limit

`df.limit(5)` returns a DataFrame of 5 rows.

## Repartition and Coalesce

- Optimization opportunity: partition the data according to some frequently filtered columns
- Repartition incur a **full shuffle** of the data
- One should only repartition when the future number of partitions is greater/when you are looking to partition by a set of columns

To get the number of partitions:
```python
df.rdd.getNumPartitions()  # this gives 1
```

To repartition:
```python
df3 = df2.repartition("DEST_COUNTRY_NAME")
df3.rdd.getNumPartitions()  # this gives 200
```
One can specify number of partitions like this
```python
df3 = df2.repartition(5, "DEST_COUNTRY_NAME")
df3.rdd.getNumPartitions()  # this gives 5
```

To combined partitions, use `coalesce`:
```python
df4 = df3.coalesce(2)
df3.rdd.getNumPartitions()  # this gives 2
```

## Collecting Rows to the Driver

```python
df.limit(5) # returning a dataframe with 5 Rows
df.take(5)  # returning a list of 5 Rows
df.limit(5).collect() # equivalent to df.take(5)
```

- To collect partitions to the driver as an **iterator**, one can use the method `toLocalIterator()`.
- This allows me to iterate over the entire dataset **partition by partition** in a serial manner.
