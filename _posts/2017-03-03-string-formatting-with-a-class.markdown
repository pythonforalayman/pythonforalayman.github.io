---
layout: default
title:  "String Formatting with a Class"
date:   2017-03-03 16:37:42 -0500
category: "String Formatting" 
tags: string formatting class
published: true
---

# String Formatting with a Class
## Class
```python
class Person(object):
    def __init__(self, first_name, last_name, age):
        super(Person, self).__init__()
        self.first_name = first_name
        self.last_name = last_name
        self.age = age
```

## Keyword Arguments
### Usage
```python
u1 = Person("Ron", "Paul", 45)
print("{first_name} {last_name} is {age} years old.".format(**vars(u1)))
```

**`vars(u1)`** returns a dictionary of keys **`first_name`**, **`last_name`**, and **`age`** of our **`u1`** object.
** maps each of those keys and values to the appropriate key in our string using **`.format()`**.

#### Output
```
Ron Paul is 45 years old.
```

___

## Positional Arguments
### Usage Positional Arguments
```python
u1 = Person("Ron", "Paul", 45)
u2 = Person("Adam", "Smith", 32)
print("{0.first_name} and {1.first_name} are friends.".format(u1, u2))
```

**`{0.first_name}`** is the equivalent of calling **`first_name`** on the **`0th`** argument.

#### Output
```
Ron and Adam are friends.
```