---
title: "How to set up logging level in Tensorlayer and Tensorflow"
date: 2018-08-28 12:52:36 +0900
tags: [tensorflow1.10.0, tensorlayer1.9.1]
categories: [deep learning, tensorflow]
---
In python logger, there are 5 logging levels: `{50: 'FATAL', 40: 'ERROR', 30: 'WARN', 20: 'INFO', 10: 'DEBUG'}`.
The logger only prints logs that are higher than the default logging level.
By default, the logging level in both tensorlayer and tensorflow is `WARN`.
This posts changes the logging level into `INFO`.

## Tensorlayer
```python
print(tl.logging._level_names)
print(tl.logging.get_verbosity())
tl.logging.set_verbosity(tl.logging.INFO)
```
## Tensorflow
```python
print(tf.logging._level_names)
print(tf.logging.get_verbosity())
tf.logging.set_verbosity(tf.logging.INFO)
```
