---
layout: default
title:  "Context Managers"
date:   2017-03-03 15:12:22 -0500
categories: context managers
---
# Context Managers

A ContextManager only keeps the object active in its scope. These are useful for file IO as you never have to remember to .close() your files. This prevents memory leaks and is all around healthier for files. If your code throws an exception, your file will still get closed.

## File IO Context Manager
### Syntax
```python
with open("file_name.txt", "mode") as variable:
    pass
```

### Usage
```python
with open("output_file.txt", "w") as f:
    f.write("{0.closed}\n".format(f))
    print("Successfully wrote to the file.")

try:
    f.write("{0.closed}\n".format(f))
except Exception as e:
    print("Did not successfully write to the file.")
    raise e
```

### Output
```
Successfully wrote to the file.
Did not successfully write to the file.
Traceback (most recent call last):
  File "C:\Development\Tutorials\context_managers.py", line 17, in <module>
    raise e
  File "C:\Development\Tutorials\context_managers.py", line 13, in <module>
    f.write("{0.closed}\n".format(f))
ValueError: I/O operation on closed file.
```

## Custom Context Manager - Timer
### Class
```python
import time

class Timer:
    def __init__(self, debug=True):
        self.debug = debug
        self.start_time = time.time()
        self.end_time = 0
        self.elapsed_time = 0

    def __enter__(self):
        if self.debug:
            print("We have now started using the Timer.")
        return self

    def __exit__(self, *args):
        self.end_time = time.time()
        self.elapsed_time = self.end_time - self.start_time
        if self.debug:
            print("We have now finished using the Timer.")
            print("{:.4f} seconds have elapsed.".format(self.elapsed_time))
```

### Usage
```python
with Timer(debug=True) as t:
    '''
    We can read `start_time` since its value is set in __init__.
    '''
    print("We started at {:.4f}".format(t.start_time))
    time.sleep(2)
    '''
    These will return 0 since we haven't left the ContextManager yet.
    '''
    print("We can't access `end_time` yet: {:.4f}".format(t.end_time))

'''
Since we have now left the ContextManager, __exit__ was called which
gives us the correct values for `end_time` and `elapsed_time`.
'''
print("We ended at {:.4f}".format(t.end_time))

```

### Output
```
We have now started using the Timer.
We started at 1487696239.0595
We can't access `end_time` yet: 0.0000
We have now finished using the Timer.
2.0001 seconds have elapsed.
We ended at 1487696241.0596
```