---
layout: default
title:  "String Formatting with a Dictionary"
date:   2017-03-03 16:38:42 -0500
category: "String Formatting" 
tags: string formatting dictionary
published: true
---

# String Formatting with a Dictionary

If you notice, this is the same idea of the Class formatting.
The `**` takes the dictionary keys and makes them in to keyword arguments for the [.format()](https://docs.python.org/3/library/string.html#custom-string-formatting) function.

## Usage
```python
person = {
    "first_name": "Ron",
    "last_name": "Paul",
    "age": 45,
}
print("{first_name} {last_name} is {age} years old.".format(**person))
```
## Output
```
Ron Paul is 45 years old.
```