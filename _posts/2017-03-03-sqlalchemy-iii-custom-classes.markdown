---
layout: default
title:  "III) Custom Table Classes in models.py"
date:   2017-03-03 09:18:22 -0500
category: SQLAlchemy 
tags: sqlalchemy tutorial custom table classes models.py
published: true
---

# SQLAlchemy
## Table Classes in **`models.py`**

For the sake of keeping everything nice and organized, we're going to be putting all of our models in **`models.py`**. Our database connectivity and other database actions will be kept in **`main.py`**.

### Imports

```python
from sqlalchemy import Column, Integer, String, Boolean, DateTime
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.sql import func
```

The first import from **`sqlalchemy`** will give us the ability to make our columns in our table. You need not import all of the ones listed.

The second import allows us to use **`declarative_base`**. As mentioned previously, this is the "backbone" of all of our table classes.

The third import, while optional, will allow us to set a default time value in one of our columns.

### Base

```python
Base = declarative_base()
```

All of our custom table classes will extend off of **`Base`**. Think of **`Base`** just as the "base model" for a class. It has all of the inner workings, we're just putting custom touches on it.

### Creating a Table

A new table in our database is simply a sub-class of **`Base`**. For this tutorial, we're going to be creating a simple **`Account`** table. Accounts will have a unique identifier, a username and password, 10 tokens, a verified status, and the date in which the account was created. 

First, all you have to do is extend the **`Base`** class.

```python
class Account(Base):
    __tablename__ = 'accounts'
```

The value of **`__tablename__`** defaults to the lowercase of our class name. We override it here since the table will have multiple accounts, so it makes sense to call the name of the table **`accounts`**.

### Adding Columns

Once we have our basic **`Account`** class set up, we can start adding in our columns. The format for a column is as follows:

```python
column_name = Column(column_type, **kwargs)
```

There are several different optional parameters (kwargs) that you can pass in to **`Column()`**, such as **`default`**, **`primary_key`**, and **`unique`**. To see the full list of possible parameters, check out [the documentation for Column](http://docs.sqlalchemy.org/en/latest/core/metadata.html#sqlalchemy.schema.Column.__init__ "Column").

**`column_type`** is the type of column, such as **`String`**, **`Integer`**, **`Boolean`**, and **`DateTime`**. For **`String`**, you can pass in a maxlen by calling **`String(maxlen)`**. To see the full list of the different types, check out [the documentation for Types](http://docs.sqlalchemy.org/en/latest/core/type_basics.html "Type Basics").

```python
class Account(Base):
    __tablename__ = 'accounts'
    
    id = Column(Integer, primary_key=True)
    username = Column(String, unique=True)
    password = Column(String(64))
    tokens   = Column(Integer, default=10)
    verified = Column(Boolean, default=False)
    date_created = Column(DateTime, server_default=func.now())
```

Here, we use **`id`** as our **`primary_key`** (a unique identifier that auto-increments for each row).

**`username`** is a unique **`String`**. No two accounts can have the same username. If we try to add in two of the same usernames, SQLAlchemy will raise an [**`sqlalchemy.exc.IntegrityError`**](http://docs.sqlalchemy.org/en/latest/core/exceptions.html "SQLAlchemy Exceptions").

**`password`** is a **`String`** with a maximum length of 64 characters. **Note:** the enforcement of the maximum length when inserting a row depends on the dialect used. If you want a strict enforcement, you will need to check the length of the value **before** inserting it.

**`tokens`** is an **`Integer`** with a default value of **10**. We want all of our accounts to start with 10 tokens.

**`verified`** is a **`Boolean`** with a default value of **`False`**. When accounts are created, they have yet to be verified. Inside of databases, **`Booleans`** are represented as 0 (**False**) and 1 (**True**).

**`date_created`** is a **`DateTime`** object that defaults to **`CURRENT_TIMESTAMP`**. We use **`server_default`** when working with **`DateTime`** so that the value is set when the database recieves the information. **`func.now()`** returns the appropriate **`CURRENT_TIMESTAMP`** for the chosen dialect.

### Overriding `__repr__` in **`Account`**
Later on, when we query our database and get results back, they will look something like this:

```python
<models.Account object at 0x0390BBF0>
```

What account is that? What's the username? How many tokens does it have? If we want to answer those questions, we need to override **`__repr__`** in our **`Account`** class. Check out [String Formatting with a Class](http://pythonforalayman.com/string-formatting-with-a-class#usage-1) for more information on this.

```python
def __repr__(self):
    return "<Account U: {0.username} T: {0.tokens} V: {0.verified}>".format(self)
```

Now, our results will like this:

```python
<Account U: admin T: 20 V: False>
```

Much better!
