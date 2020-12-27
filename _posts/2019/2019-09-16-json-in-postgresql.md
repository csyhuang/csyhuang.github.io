---
layout: post
title: Handling JSON in PostgreSQL
comments: true
tags: ['sql']
---

Found a useful [cheatsheet](https://devhints.io/postgresql-json) that listed out operations on JSON in PostgreSQL.

If I want to list the rows where column `col1` in table `table1` contains a JSON object with the key `key1`, I can use:

```sql
select * from table1
where col1->'key1' IS NOT NULL
```