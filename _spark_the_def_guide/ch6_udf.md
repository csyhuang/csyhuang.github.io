---
layout: post
title: Ch.6 - Working with different types of Data (UDFs)
tags: ['sparkdefguide']
order: 7
comments: true
---

# User-Defined Functions (UDFs)

- UDFs are functions that operate on the data, record by record. These functions are registered as temporary functions to be used in that specific SparkSession/Context.
- One needs to register the UDFs with Spark so that we can use them on all of our worker machines. Spark will serialize the function on the driver and trasfer it over the network to all executor processes.
- Scalar-written UDFs can be used within the JVM.
- **Attention: (not sure if this is still true)** For python, spark starts a python process on the worker, serializes all of the data to a format that Python can understand, executes the function row by row on the data in the python process, then finally returns the results of the row operations to the JVM and Spark.
- The real cost of Python process is in *serializing the data to Python*:
	1. it is an expensive computation
	2. After the data enters Python, Spark cannot manage the memory of the workers
	3. One could potentially cause a worker to fail if it becomes resources constrained

## Procedures
```python
def power3(double_value):
    returns double_value ** 3
spark.udf.register("power3py", power3, DOubleType())
```
