---
layout: post
title: Ch.3 - A Tour of Spark's Toolset
tags: ['sparkdefguide']
order: 3
comments: true
---

# Running Production Applications

- `spark-submit` sends your application code to a cluster and launch it to execute there. The application will run until it exists (completes the task) or encounters an error.
- Resources needed and how the application should be run can be specified in *command-line arguments*.

## Example: calculate pi locally

Scala version:

```bash
./bin/spark-submit \
  --class org.apache.spark.example.SparkPi \
  --master local
  ./example/jars/spark-example_2.11-2.2.0.jar 10  # which jar and class to run, followed by the argument
```

Python version:

```bash
./bin/spark-submit \
  --master local
  ./example/src/main/python/pi.py 10  # which jar and class to run, followed by the argument
```

The `--master` argument specifies where you submit the application to. It can also be a cluster running Spark's standalone cluster manager, Mesos or YARN.

# Datasets: Type-Safe Structured APIs

- Datasets: for writing statically typed code in Java and Scala
- Not available in python or R because those languages are *dynamically typed*
- API available for *Datasets* are *type-safe*: one cannot accidentally view the objects in a Dataset as being of another class than the class one puts in initially
	- e.g. a `Dataset[Person]` would be guaranteed to contain objects of class `Person`.
    - `collect` or `take` on a Dataset would give you the proper type in the Dataset, not DataFrame Rows.

# Structured Streaming

- Running operations in a *streaming* fashion instead of *batch mode* can **reduce latency** and **allow for incremental processing**.
- Advantage of *structured streaming*: allows you to extract value out of streaming system with virtually *no code changes*

(More notes on streaming in Chapters 20-23)