---
layout: galileo_post
title:  Diving into Keras 
date:   2017-09-05 13:05:14 +0100
description: Switching from Scala back to Python always takes some...
---

Switching from Scala back to Python always takes some adjusting. Constantly getting this error is a little infuriating:
{% highlight python %}
>>> val a = 'hello'
  File "<stdin>", line 1
    val a = 'hello'
        ^
SyntaxError: invalid syntax
{% endhighlight %}

How did I find myself in this strange dilemma where Python and Scala had to play together?

I decided to try using Keras for some neural netting to solve a modeling problem I had. But the input data was fairly large and in parquet format, so I decided to do some ETL in Spark/Scala, then move into Python for more ETL and finally modeling. It wasn't the smoothest of pipelines since file formats were flying all over the place! From parquet -> csv -> hdf5, I wish I had planned out my pipeline a little better when I had started.

All in all, this week was pretty intense. While I have a repository of Scala code I can work off of, I unfortunately have no Python code. And since I have no framework for building up Keras models, figuring out something that would be re-usable is ideal. But for those looking to get into Keras, I think the most important takeaways from this week's modeling were

* **Generators** - What happens when your data is too big to fit in memory? The answer is - batched inputs with generators. I personally like hdf5 files because its speed in handling large matrices is way better than your standard pickle.
* **Checkpointing** - Saving the best model at each epoch allows you to potentially inspect before the job is done. It also allows you to restart your model from any point in case your job crashes for whatever reason
* **Early Stopping** - Being able to set the epochs to 1000 and just wait for early stopping to kick in is awfully convenient. I'm not a fan of coming back to my model, only to find out it hasn't converged yet.

Most of this code was picked up online, on the fly. I would highly recommend [Jason Brownlee's blog](https://machinelearningmastery.com/check-point-deep-learning-models-keras/), which contains many examples of these features (and more!) available in Keras.