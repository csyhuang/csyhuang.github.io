---
layout: post
title: Reduce the number of files on HDFS
comments: true
tags: ['sql', 'pyspark']
---

Sometimes, hive tables are saved not in an optimal way that creates lots of small files and reduce the performance of the cluster. This blog post is about pyspark functions to reduce the number of files (and can shrink storage size when the data is indeed sparse).

To check the number of files and their size, I used this HDFS command:

```bash
$ hadoop cluster_name fs -count /hive/warehouse/myschema.db/
```

There will be 4 columns printed out. I'm showing one of the examples among the table I shrank today:

|Directory count|File count|Content size|Table name|
|-----------|-----------|-----------|-----------|
|3|854|104950877|original_table_w_many_small_files|


When I check the file sizes of the 854 files using `hadoop cluster_name fs -du -h /hive/warehouse/myschema.db/original_table_w_many_small_files/*/*/*`, I find that all of them are indeed of small size:

```
99.9 K  /hive/warehouse/myschema.db/original_table_w_many_small_files/some_part_id=30545/timestamp=1595397053/part-00845-29c4c506-e991-4d1d-be67-43e0a9976179.c000
102.7 K  /hive/warehouse/myschema.db/original_table_w_many_small_files/some_part_id=30545/timestamp=1595397053/part-00846-29c4c506-e991-4d1d-be67-43e0a9976179.c000
104.4 K  /hive/warehouse/myschema.db/original_table_w_many_small_files/some_part_id=30545/timestamp=1595397053/part-00847-29c4c506-e991-4d1d-be67-43e0a9976179.c000
100.6 K  /hive/warehouse/myschema.db/original_table_w_many_small_files/some_part_id=30545/timestamp=1595397053/part-00848-29c4c506-e991-4d1d-be67-43e0a9976179.c000
98.8 K  /hive/warehouse/myschema.db/original_table_w_many_small_files/some_part_id=30545/timestamp=1595397053/part-00849-29c4c506-e991-4d1d-be67-43e0a9976179.c000
108.5 K  /hive/warehouse/myschema.db/original_table_w_many_small_files/some_part_id=30545/timestamp=1595397053/part-00850-29c4c506-e991-4d1d-be67-43e0a9976179.c000
106.5 K  /hive/warehouse/myschema.db/original_table_w_many_small_files/some_part_id=30545/timestamp=1595397053/part-00851-29c4c506-e991-4d1d-be67-43e0a9976179.c000
101.9 K  /hive/warehouse/myschema.db/original_table_w_many_small_files/some_part_id=30545/timestamp=1595397053/part-00852-29c4c506-e991-4d1d-be67-43e0a9976179.c000
65.5 K  /hive/warehouse/myschema.db/original_table_w_many_small_files/some_part_id=30545/timestamp=1595397053/part-00853-29c4c506-e991-4d1d-be67-43e0a9976179.c000
```

To combine the files, the following was what I do with a pyspark (jupyter) notebook.

## Create table using parquet format

First of all, I check the schema of the table using `SHOW CREATE TABLE myschema.original_table_w_many_small_files`:

```sql
CREATE TABLE myschema.original_table_w_many_small_files (
  `some_id` BIGINT,
  `string_info_a` STRING,
    ...
  `some_part_id` INT,
  `timestamp` BIGINT)
USING orc
PARTITIONED BY (some_part_id, timestamp)
TBLPROPERTIES (
  ... )
```

I create a table called `myschema.new_table_with_fewer_files` in parquet format instead:

```sql
CREATE TABLE myschema.new_table_with_fewer_files (
  `some_id` BIGINT,
  `string_info_a` STRING,
    ...
  `some_part_id` INT,
  `timestamp` BIGINT)
USING PARQUET
PARTITIONED BY (some_part_id, timestamp)
```

Then, I retrieved the original table and repartitioned it, such that all data in each partition is combined to a single file (`numPartitions=1`) since my cluster can handle files of size up to 100 M. You shall adjust `numPartitions` based on the resources you have.

```python
df = spark.table('myschema.original_table_w_many_small_files')
df.repartition(numPartitions=1).write.insertInto('myschema.new_table_with_fewer_files')
```

Here is a comparison between the storage of the old and new table:

|Table name|Directory count|File count|Content size|
|-----------|-----------|-----------|-----------|
|original_table_w_many_small_files|3|854|104950877|
|new_table_with_fewer_files|7|4|14866996|

When checking the table size using human readable format `hadoop fs -du -h`, I find that the table has been shrunk from `100.1 M` to `39.6 M`.

