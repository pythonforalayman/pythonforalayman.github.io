---
layout: default
title:  "String Formatting"
date:   2017-03-03 16:36:42 -0500
categories: string formatting
---

#String Formatting (3.5)
The basis of this tutorial is the [.format()](https://docs.python.org/3/library/string.html#custom-string-formatting) function. It allows you to mix and match object types without having to explicitly call str(), rearrange positional and keyword arguments, and utilize various other features.

You can replace

```python
"Your name is " + first_name + " " + last_name + " and your age is " + str(age) + "."
```

with

```python
"Your name is {0} {1} and your age is {2}.".format(first_name, last_name, age)
```

##General String Formatting

In general, inside of your string you replace your variables with **`{n}`**, where **`n`** is the position of the argument you want.


```python
print("First Argument: '{0}' Second Argument: '{1}'".format("I'm number 1!", ":( number two."))
```

```
First Argument: 'I'm number 1!' Second Argument: ':( number two.'
```

##String Formatting with a Class
####Class
```python
class Person(object):
    def __init__(self, first_name, last_name, age):
        super(Person, self).__init__()
        self.first_name = first_name
        self.last_name = last_name
        self.age = age
```

####Usage
```python
u1 = Person("Ron", "Paul", 45)
print("{first_name} {last_name} is {age} years old.".format(**vars(u1)))
```

**`vars(u1)`** returns a dictionary of keys **`first_name`**, **`last_name`**, and **`age`** of our **`u1`** object.
** maps each of those keys and values to the appropriate key in our string using **`.format()`**.

####Output
```
Ron Paul is 45 years old.
```

```python
u1 = Person("Ron", "Paul", 45)
u2 = Person("Adam", "Smith", 32)
print("{0.first_name} and {1.first_name} are friends.".format(u1, u2))
```

**`{0.first_name}`** is the equivalent of calling **`first_name`** on the **`0`**th argument.

####Output
```
Ron and Adam are friends.
```

##String Formatting with an Array
####Positional Arguments
```python
names = ["Ron", "Adam"]
ages = [45, 32]
print("{0[0]} is the first name and {1[0]} is the first age.".format(names, ages))
print("{0[1]} is the second age.".format(ages))
```

####Keyword Arguments
```python
names = ["Ron", "Adam"]
print("{names[0]} is the first name.".format(names=names))
print("{names[1]} is the second name.".format(names=names))
```
####Output
```
Ron is the first name.
Adam is the second name.
```


##String Formatting with a Dictionary
If you notice, this is the same idea of the Class formatting.
The `**` takes the dictionary keys and makes them in to keyword arguments for the [.format()](https://docs.python.org/3/library/string.html#custom-string-formatting) function.
####Usage
```python
person = {
    "first_name": "Ron",
    "last_name": "Paul",
    "age": 45,
}
print("{first_name} {last_name} is {age} years old.".format(**person))
```
####Output
```
Ron Paul is 45 years old.
```