---
layout: post
title: Ch.4 - Structured API Overview
tags: ['sparkdefguide']
order: 4
comments: true
---

# Transformations v.s. Actions

- Spark is a Distributed programming model.
- Multiple *transformations* build up a directed acyclic graph of instructions.
- An *action* is the execution of the graph of instructions by breaking it down into *stages* and *Tasks*.
- Transformation *creates* a new DataFrame/Dataset. Computation or conversion to native language types are done by calling an *action*.

# DataFrames and Datasets

- Both are (distributed) table-like collections with well-defined rows and columns.
- They represent **immutable, lazy evaluated plans** that specify what operations to apply to data residing at a location to generate outputs.
- Tables and views are DataFrames equivalent.

# Schemas

- A schema defines the *column names* and *types* of a DataFrame.

# Overview of Structured Spark Types

## Catalyst
- Spark use *Catalyst* to maintain its own type information throughout the planning and processing of work.
- This enables a *wide variety of execution optimization*
- Manipulations operate strictly on Spark *type* most of the time (even for Python/R)

```python
df = spark.range(500).toDF("number")
df.select(df["number"] + 10)
```

# DataFrame v.s. Datasets

## DataFrames
- "untyped": only check whether those types line up to those specified in the schema at *runtime*
- DataFrames are simply Datasets of Type `Row`

## DataSets
- "typed": check whether types conform to the specification at *compile time*
- Only available in Scala

## Rows
- `Row` is a rcord of data
- `Row` type is Spark's internal representation of its optimized in-memory format for computation
- It is highly specialized and efficient computation because:
  - high garbage-collection and object instantiation costs using JVM types is avoided
- Rows can be manually created from:
  - SQL
  - RDD
  - from data source
  - from scratch (e.g. `spark.range(2).collect()`)

## Columns

Possible types to represent includes:
- *simple type*, e.g. integer, string
- *complex type*, e.g. array, map
- *null value*

# Overview of Structured API Execution

1. Write DataFrame/Dataset/SQL Code.
2. Valid code would be converted to a *Logical Plan*
3. Spark turns the *Logical Plan* to a *Physical Plan* (check this)(?)
4. Spark executes the Physical Plan (RDD manipulation) on the cluster

## Logical Planning

```

                                                         Logical
                              Analysis                 Optimization
User Code -> Unresolved plan ---------> Resolved Plan --------------> Optimized Plan
                                 ^
                                 |
                                 |
                              Catalog

```

- Spark uses the *catalog*, a repository of all tables and DataFrame information, to *resolve* columns and tables in the *analyzer*. 
- **Catalyst Optimizer** is a collection of *rules* that attempt to optimize the logical plan by **pushing down predicates or selection**.
- Resolved physical plan is passed through the *Catalyst Optimizer* to obtain an optimized plan.

## Physical Planning

The *physical plan* specifies how the logical plan will execute on the cluster by generating different physical execution strategy and comparing them through a *cost model*.

```

  Optimized          Various          Cost            Best             Executed
logical plan ---> Physical Plans ---> Model ---> Physical Plan ---> on the cluster

```

## Execution

- Upon selecting a physical plan, Spark runs all the code over RDDs, the lower-level programming interface of Spark.
- Spark performs further optimizations at runtime, generating native Java bytecode that can remove entire tasks or stages during execution.
- The results is finally returned to the user.
