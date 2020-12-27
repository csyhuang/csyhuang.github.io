---
layout: post
title: Tips for writing more efficient SQL
comments: true
tags: ['sql']
---

Learned from colleagues some points to pay attention to when writing SQL queries. (This post will be updated from time to time.)

# Partitioned tables

**Always** specify the partition in the `where` clause. If you have to retrieve data from several partitions, loop through it one-by-one.

# Distinct elements

Note that the two queries

```sql
SELECT DISTINCT(item) FROM table_X
```

and

```sql
SELECT item FROM table_X
GROUP BY item
```

give the same result. However, the second SQL query will be executed faster. There is no difference calling `distinct` and `group by` via (py)spark though.

# JOIN v.s. WHERE

Always use `where` to filter the table to be joined to the smallest, e.g.

```sql
SELECT c.* FROM credit c
INNER JOIN (
	SELECT date, item FROM purchase
	WHERE date > 20190207
) p
ON c.date = p.date
WHERE c.eligibility = True
```

Note that the line `WHERE c.eligibility = True` is executed to filter the table `credit` before the `JOIN`. This shrinks the table `credit` to the smallest before joining.
