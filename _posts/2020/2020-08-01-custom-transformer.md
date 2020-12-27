---
layout: post
title: Custom Transformer that can be fitted into Pipeline
comments: true
tags: ['pyspark']
---

How to construct a custom Transformer that can be fitted into a Pipeline object? I learned from a colleague today how to do that.

Below is an example that includes all key components:
<!--more-->
```python
from pyspark import keyword_only
from pyspark.ml import Transformer
from pyspark.ml.param.shared import HasInputCol, HasOutputCol, Param, Params, TypeConverters
from pyspark.ml.util import DefaultParamsReadable, DefaultParamsWritable
from pyspark.sql import DataFrame
from pyspark.sql.types import StringType
import pyspark.sql.functions as F

class CustomTransformer(Transformer, HasInputCol, HasOutputCol, DefaultParamsReadable, DefaultParamsWritable):
  input_col = Param(Params._dummy(), "input_col", "input column name.", typeConverter=TypeConverters.toString)
  output_col = Param(Params._dummy(), "output_col", "output column name.", typeConverter=TypeConverters.toString)
  
  @keyword_only
  def __init__(self, input_col: str = "input", output_col: str = "output"):
    super(CustomTransformer, self).__init__()
    self._setDefault(input_col=None, output_col=None)
    kwargs = self._input_kwargs
    self.set_params(**kwargs)
    
  @keyword_only
  def set_params(self, input_col: str = "input", output_col: str = "output"):
    kwargs = self._input_kwargs
    self._set(**kwargs)
    
  def get_input_col(self):
    return self.getOrDefault(self.input_col)
  
  def get_output_col(self):
    return self.getOrDefault(self.output_col)
  
  def _transform(self, df: DataFrame):
    input_col = self.get_input_col()
    output_col = self.get_output_col()
    # The custom action: concatenate the integer form of the doubles from the Vector
    transform_udf = F.udf(lambda x: '/'.join([str(int(y)) for y in x]), StringType())
    return df.withColumn(output_col, transform_udf(input_col))
  
```

Let's test it with a dataframe
```
df = spark.createDataFrame([(Vectors.dense([2.0, 1.0, 3.0]),), (Vectors.dense([0.4, 0.9, 7.0]),)], ["numbers"])
```
and a Pipeline like this:
```python
from pyspark.ml.feature import ElementwiseProduct
from pyspark.ml.linalg import Vectors
from pyspark.ml import Pipeline

elementwise_product = ElementwiseProduct(scalingVec=Vectors.dense([2.0, 3.0, 5.0]), inputCol="numbers", outputCol="product")
custom_transformer = CustomTransformer(input_col="product", output_col="results")
pipeline = Pipeline(stages=[elementwise_product, custom_transformer])
model = pipeline.fit(df)
results = model.transform(df)
results.show()
```

I manage to get the expected results:
```
+-------------+--------------+-------+
|      numbers|       product|results|
+-------------+--------------+-------+
|[2.0,1.0,3.0]|[4.0,3.0,15.0]| 4/3/15|
|[0.4,0.9,7.0]|[0.8,2.7,35.0]| 0/2/35|
+-------------+--------------+-------+
```