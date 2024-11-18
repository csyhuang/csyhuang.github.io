---
layout: post
title: Socket Timeout error in BigDL Orca
comments: true
tags: ['pyspark', 'ds', 'se']
---

To train deep learning model written in [PyTorch](https://pytorch.org) with Big Data in a distributed manner, we use [BigDL-Orca](https://bigdl.readthedocs.io/en/latest/doc/Orca/) at work. 🛠️

<!--more-->

Compared to the Keras interface of BigDL, PyTorch (Orca) supports customization of various components for Deep Learning. For example, using `bigdl-dllib` keras API, you are constrained to use only available operations in [Autograd module](https://bigdl.readthedocs.io/en/latest/doc/DLlib/Overview/dllib.html#autograd-examples-using-bigdl-dllb-keras-python-api) to customize loss functions, while you can do whatever you like in PyTorch (Orca) by creating customized subclass of `torch.nn.modules.loss._Loss` . 😁

One drawback of Orca, though, is the mysterious error logging, as what happened *within* the java class (i.e. what causes the error) is not logged at all. I got stuck in error during model training, but what I got from the Spark log was just `socket timeout` . There can be many possibilities, but the one I encountered was about the size of `train_data`. 

Great thanks to my colleague Kevin Mueller who figured out the cause 🙏 - when the partitions contain different number of batches in Orca, some barriers can never be reached and that results in such error. 

To get around this, I dropped some rows to make sure the total size of `train_data` is a multiple of batch size:

```python
train_data = train_data.limit(train_data.count() - train_data.count() % batch_size)
```

The training process worked afterwards. 😁
