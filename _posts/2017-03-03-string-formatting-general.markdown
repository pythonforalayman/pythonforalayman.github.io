---
layout: default
title:  "General String Formatting"
date:   2017-03-03 16:37:42 -0500
category: "String Formatting" 
tags: string formatting general
published: true
---

# General String Formatting
### Positional Arguments
If you are using positional arguments, inside of your string you replace your variables with **`{n}`**, where **`n`** is the position of the argument you want.

#### Usage
```python
print("First Argument: '{0}' Second Argument: '{1}'".format("I'm number 1!", ":( number two."))
```

#### Output
```
First Argument: 'I'm number 1!' Second Argument: ':( number two.'
```


### Keyword Arguments
#### Usage
If you pass in keyword arguments, you simply put **`{key}`**, where **`key`** is the keyword in **`.format()`**. Keywords are not positional, so the order of them does not matter.

```python
print("Name: '{name}' Age: '{age}'".format(age=45, name="Ron Paul"))
```

#### Output
```
Name: 'Ron Paul' Age: '45'
```