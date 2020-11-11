---
layout: post
title: Ch.2 - A Gentle Introduction to Spark
tags: ['sparkdefguide']
order: 2
comments: true
---

# Spark:
- Manages and coordinates the execution of tasks on data across a cluster of computers
- Manager can be Spark’s standalone cluster manager, YARN, or Mesos
- Driver:
    - process runs your main() functions
    - sits on a **node** in the cluster
    - responsible for:
        - Maintaining information about Spark Application
        - Responding to a user’s program or input
        - Analyzing, distributing and scheduling work across the executors
- Executors:
    - Carry out the work that the driver assigns to them
    - Each executor is responsible for:
        - Executing code assigned to it by the driver
        - Reporting the state of computation that executor back to the driver node
<!--more-->

# Spark Session:
- Entry point of any spark program
- Translates python/R code into code that it then can run on the executor JVMs
- A driver process via which you control your Spark Application
- Has one-to-one correspondence to a Spark Application
- `spark-submit`: submit a precompiled application to Spark

# Spark Dataframe:
- A distributed collection - each part (set of rows) exists on a different executor
- A table of data with **rows** and **columns**
- Schema: the list that defines the **columns** and the **types**

# Partitions:
- Spark breaks up data into chunks called partitions
- A partition is a collection of rows
- The efficiency of parallelism is determined by **the number of partitions** and **the number of executors**

# Transformations:
- The core data structure are **immutable** (i.e. cannot be changed after they’re created)
- Transformation is a way to “change” a DataFrame
- **Abstract transformation**: spark will not act on transformations until we call an **action**
- Two types of transformation:
    - Narrow transformation:
        - each input partition will contribute to only *one output partition*
        - pipelining: performed in-memory
        - e.g. read
    - Wide transformation: 
        - Input partitions contributing to *many output partitions*
        - shuffle: writes the results to disk
        - e.g. sort
- Transformation are simply ways of specifying *different series of data manipulation*
- Reading data is a *lazy operation* (narrow transformation): `spark.read.option("inferSchema", "true").option("header", "true").csv("...")`

# Lazy evaluation:
- Spark waits until the *very last moment* to execute the graph of computation instructions
- Spark compiles a plan from your raw DataFrame transformations to a streamlined physical plan that will run as efficiently as possible across the cluster
- E.g., a predicate pushdown on DataFrames filters the data in the database query, reducing the number of entries retrieved from the database and improving query performance

# Actions:
- An action instructs Spark to compute a result from a series of transformation
- Three kinds of actions:
    - View data in the console (e.g. `df.show()`)
    - Collect data to native objects in the respective language (e.g. `df.collect()`)
    - Write to output data source (e.g. `df.write...`)
- Example of action:  `df.take(5)`

# Spark UI:
- Spark UI in local mode: `http://localhost:4040`

# Concepts from an end-to-end example
- **schema inference**: let Spark guess what the schema of the DataFrame should be
- Physical plan called via `df.explain()`: the plan is read from top (end results) to bottom (source(s) of data)
- Set the number of output partitions from the shuffle: `spark.conf.set("spark.sql.shuffle.partitions", "5")`
- A **lineage**: Spark knows how to recompute any partition by performing all the operations it had before on the same input data (the *heart* of Spark's programming model - *functional programming*)
- To monitor the job progress, nevigate to the Spark UI on port `4040` to see:
	- the physical plan, and
	- local execution characteristics of your job
- A **direct acyclic graph** (DAG) of transformations shows the execution plan
