---
layout: post
title: Pyspark error "Could not serialize object"
comments: true
tags: ['pyspark']
---

This post is related to the idea discuss in the post ["How to Solve Non-Serializable Errors When Instantiating Objects In Spark UDFs"](https://www.placeiq.com/2017/11/how-to-solve-non-serializable-errors-when-instantiating-objects-in-spark-udfs/). Here I discuss the solution in a hypothetical scenario in pyspark.
<!--more-->
Suppose I have a class to transform string to sum of numbers like this:

```python
import pyspark.sql.functions as F
from pyspark.sql.functions import udf
from pyspark.sql.types import IntegerType

class AnimalsToNumbers(object):
    def __init__(self, spark):
        self._mapping = {'elephant': 1, 'bear': 4, 'cat': 9}
        self._spark = spark
        
    def addition(self, animal_str):
        return sum([self._mapping.get(x, 0)
                    for x in animal_str.split('+')])
    
    def add_up_animals(self, df):
        addition_udf = F.udf(self.addition, IntegerType())
        return df.withColumn('sum', addition_udf('animal'))
```

(In practical cases, `self._mapping` is a huge object containing the dictionary and other attributes that are derived from methods in the `AnimalsToNumbers` class.) If I want to transform a dataframe `df` that looks like this:

```
+-------------+
|       animal|
+-------------+
|elephant+bear|
|     cat+bear|
+-------------+
```

The operation

```python
AnimalsToNumbers(spark).add_up_animals(df)
```

will lead to an error like this:

```
---------------------------------------------------------------------------
Py4JError                                 Traceback (most recent call last)
~/anaconda3/envs/pyspark_demo/lib/python3.5/site-packages/pyspark/serializers.py in dumps(self, obj)
    589         try:
--> 590             return cloudpickle.dumps(obj, 2)
    591         except pickle.PickleError:
...
PicklingError: Could not serialize object: Py4JError: An error occurred while calling o25.__getstate__. Trace:
py4j.Py4JException: Method __getstate__([]) does not exist
	at py4j.reflection.ReflectionEngine.getMethod(ReflectionEngine.java:318)
	at py4j.reflection.ReflectionEngine.getMethod(ReflectionEngine.java:326)
	at py4j.Gateway.invoke(Gateway.java:274)
	at py4j.commands.AbstractCommand.invokeMethod(AbstractCommand.java:132)
	at py4j.commands.CallCommand.execute(CallCommand.java:79)
	at py4j.GatewayConnection.run(GatewayConnection.java:238)
	at java.lang.Thread.run(Thread.java:748)
```

The issue is that, as `self._mapping` appears in the function `addition`, when applying `addition_udf` to the pyspark dataframe, the object `self` (i.e. the `AnimalsToNumbers` class) has to be serialized but it can't be.

A (surprisingly simple) way is to create a reference to the dictionary (`self._mapping`) but not the object:

```python
class AnimalsToNumbers(object):
    def __init__(self, spark):
        self._mapping = {'elephant': 1, 'bear': 4, 'cat': 9}
        self._spark = spark
        
    
    def add_up_animals(self, df):
        mapping = self._mapping
        def addition(animal_str):
            return sum([mapping.get(x, 0) for x in animal_str.split('+')])

        addition_udf = F.udf(addition, IntegerType())
        return df.withColumn('sum', addition_udf('animal'))

```

`AnimalsToNumbers(spark).add_up_animals(df).show()` would yield:

```
+-------------+---+
|       animal|sum|
+-------------+---+
|elephant+bear|  5|
|     cat+bear| 13|
+-------------+---+

```

Yay :)


