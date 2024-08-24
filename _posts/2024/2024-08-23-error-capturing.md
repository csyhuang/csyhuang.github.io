---
layout: post
title: Capturing error with more details in Python
comments: true
image_url: ''
tags: ['se']
---

The python native library `traceback` can provide more details about an (unexpected) error compared to error catching with `except Exception as ex:` and then examine `ex`. 

Let's make a function that would result in error for demonstration:

```python
import traceback


def do_something_wrong():
    cc = int("come on!")
    return cc

try:  # first catch
    do_something_wrong()
except Exception as ex:
    print(f"The Exception is here:\n{ex}")

try:  # second catch
    do_something_wrong()
except:
    print(f"Use traceback.format_exc() instead:\n{traceback.format_exc()}")
```

The first catch would only display
```
The Exception is here:
invalid literal for int() with base 10: 'come on!'
```

while the second catch includes not only the error but where it occurs
```python
Use traceback.format_exc() instead:
Traceback (most recent call last):
  File "/Users/csyhuang/JetBrains/PyCharm2024.1/scratches/scratch.py", line 16, in <module>
    do_something_wrong()
  File "/Users/csyhuang/JetBrains/PyCharm2024.1/scratches/scratch.py", line 5, in do_something_wrong
    cc = int("come on!")
```

In a large software project, the second example would be way more helpful than the first.