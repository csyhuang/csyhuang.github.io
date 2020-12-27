---
layout: post
title: Ranking hierarchical labels with SQL
comments: true
tags: ['sql', 'pyspark']
---

To give indices to hierarchical labels, I can use `DENSE_RANK()` or `RANK()` depending on the situation. 
For example, if I have a DataFrame that looks like this:

```
+------+----------+
|Fridge|    Fruits|
+------+----------+
|     A|     apple|
|     B|    orange|
|     C|     apple|
|     D|     pears|
|     C|Watermelon|
+------+----------+
```

The following SQL code

```sql
SELECT
    Fridge,
    Fruits,
    DENSE_RANK() OVER (ORDER BY Fridge, Fruits) AS Loc_id,
    DENSE_RANK() OVER (ORDER BY Fridge) AS Fridge_id_dense,
    RANK() OVER (ORDER BY Fridge) AS Fridge_id,
    DENSE_RANK() OVER (ORDER BY Fruits) AS Fruit_id_dense,
    RANK() OVER (ORDER BY Fruits) AS Fruit_id
FROM fridge_list
```

would yield the following table

```
+------+----------+------+---------------+---------+--------------+--------+
|Fridge|    Fruits|Loc_id|Fridge_id_dense|Fridge_id|Fruit_id_dense|Fruit_id|
+------+----------+------+---------------+---------+--------------+--------+
|     A|     apple|     1|              1|        1|             2|       2|
|     B|    orange|     2|              2|        2|             3|       4|
|     C|Watermelon|     3|              3|        3|             1|       1|
|     C|     apple|     4|              3|        3|             2|       2|
|     D|     pears|     5|              4|        5|             4|       5|
+------+----------+------+---------------+---------+--------------+--------+

```