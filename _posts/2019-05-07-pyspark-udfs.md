---
title: "Developing Pyspark UDFs"
excerpt: "A post to custom sidebar content."
author_profile: True
tags:
  - datascience
  - programming
---

Pyspark UserDefindFunctions (UDFs) are an easy way to turn your ordinary python code into something scalable. There are two ways to make a UDF from a function.

The first method is to explicitly define a udf that you can use as a pyspark function

```python
from pyspark.sql.types import StringType
from pyspark.sql.functions import udf, col

def say_hello(name : str) -> str:
     return f"Hello {name}"
     
assert say_hello("Summer") == "Hello Summer"
say_hello_udf = udf(lambda name: say_hello(name), StringType())

df = spark.createDataFrame([("Rick,"),("Morty,")], ["name"])
df.withColumn("greetings", say_hello_udf(col("name")).show()
+------+------------+
|  name|   greetings|
+------+------------+
|  Rick|  Hello Rick|
| Morty| Hello Morty|
+------+------------+
```

However, this means that for every pyspark UDF, there are two functions to keep track of — a regular python one and another pyspark `_udf` one. For a cleaner pattern, the udfs can be created with a built in decorator.

```python
@udf(returnType=StringType())
def say_hello(name):
     return f"Hello {name}"

# This doesn't work anymore
# assert say_hello("Summer") == "Hello Summer"

df.withColumn("greetings", say_hello(col("name")).show()
```

I prefer this pattern over the previous one since the only function to manage in the code is the pyspark one — the regular python one isn’t really necessary anymore.

That doesn’t mean that the regular python function isn’t helpful anymore. Typically, developing pyspark UDFs means that a regular python function is first created, tested, then turned into a pyspark UDF. This allows testing to be performed on less data to confirm functionality prior to scaling up to whatever size data is desirable. In general, it leads to more rapid development since bugs can be caught faster and earlier, as opposed to 5 minutes later after waiting for a spark job to be submitted, run, and then hunting through YARN logs to find the error.

But what if you need to change the method’s functionality after going through this cycle? What happens to the tests that you spent developing to ensure proper functionality? You could comment out the decorator line and turn the function back into a regular python method to go back into development and testing. But is there a way around this?

Introducing — `py_or_udf` — a decorator that allows a method to act as either a regular python method or a pyspark UDF

```python
from typing import Callable
from pyspark.sql import Column
from pyspark.sql.functions import udf, col
from pyspark.sql.types import StringType, IntegerType, ArrayType, DataType

class py_or_udf:
    def __init__(self, returnType : DataType=StringType()):
        self.spark_udf_type = returnType
        
    def __call__(self, func : Callable):
        def wrapped_func(*args, **kwargs):
            if any([isinstance(arg, Column) for arg in args]) or \
                any([isinstance(vv, Column) for vv in kwargs.values()]):
                return udf(func, self.spark_udf_type)(*args, **kwargs)
            else:
                return func(*args, **kwargs)
            
        return wrapped_func
        
@py_or_udf(returnType=StringType()
def say_hello(name):
     return f"Hello {name}"

assert say_hello("world") == "Hello world" #This works
df.withColumn("greeting", say_hello(col("name"))).show() # This also works
```

This is currently my most preferred pattern of development, as it simultaneously allows rapid development and testing in python as well as functional usage as a pyspark UDF without any changes. If there are other “Spark Pro Tips” I would be happy to hear more!

#### References

[1] https://python-3-patterns-idioms-test.readthedocs.io/en/latest/PythonDecorators.html