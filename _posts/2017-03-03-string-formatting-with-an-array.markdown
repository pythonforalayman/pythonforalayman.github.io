---
layout: default
title:  "String Formatting with an Array"
date:   2017-03-03 16:38:42 -0500
category: "String Formatting" 
tags: string formatting array
published: true
---

# String Formatting with an Array
Inside of our arguments, we can call the standard Python list slicing (**example**: **`list[0]`**).


## Positional Arguments
### Usage
```python
names = ["Ron", "Adam"]
ages = [45, 32]
print("{0[1]} is the second name and {1[0]} is the first age.".format(names, ages))
print("{0[1]} is the second age.".format(ages))
```

### Output
```
Adam is the first name and 45 is the first age.
32 is the second age.
```


## Keyword Arguments
### Usage
```python
names = ["Ron", "Adam"]
print("{names[0]} is the first name.".format(names=names))
print("{names[1]} is the second name.".format(names=names))
```

### Output
```
Ron is the first name.
Adam is the second name.
```