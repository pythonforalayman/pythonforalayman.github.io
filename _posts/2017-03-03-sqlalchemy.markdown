---
layout: default
title:  "Full Tutorial"
date:   2017-03-03 09:15:22 -0500
category: SQLAlchemy 
tags: sqlalchemy tutorial
published: true
---
# SQLAlchemy

### Table of contents
{:.no_toc}

* Table of contents placeholder
{:toc}


## I) Installation
It's highly recommended to install all of your packages into a **`virtualenv`** that you're going to be using for this project. It helps keep your packages nice and orderly. If you're **not** using **`virtualenv`**, skip the next two steps.

```
virtualenv sqlalchemy-tut
sqlalchemy-tut\Scripts\activate
```

Your command prompt should now have your **`virtualenv`** as a prefix to your path.

```
(sqlalchemy-tut) C:\Users\Admin\Development\Tutorials>
```

Once you have your **`virtualenv`** activated, you can now install **`sqlalchemy`**.

```
pip install sqlalchemy
```

For this tutorial, **`sqlalchemy-1.1.5`** will be used. We're going to have two files, **`main.py`** for all of the database-related code, and **`models.py`** for our tables that we're going to create.

## II) Getting Connected in **main.py**


### Imports
First, we need to set up the imports that we're going to need to set up the database connector. 

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from models import Base, Account
```

**Note:** We have not created `models` yet so this will not successfully run yet.

### Database URI
Once we have the proper packages imported, we need to connect to the database that we're going to use. The format of the URI goes like this:

**`dialect`** is the type of database such as **`sqlite`**, **`mysql`**, **`postgresql`**, etc. For a list of supported dialects, check out [the documents.](http://docs.sqlalchemy.org/en/latest/core/engines.html)
**`driver`** is optional - if not specified, it will default to the most widely used driver for that **`dialect`**.

```python
database_uri = "dialect+driver://username:password@host:port/database"
```

For this tutorial, we will be using **`sqlite`**. Notice the extra **`/`** for **`sqlite`** - this is needed because there is no host.

```python
database_uri = "sqlite:///sqlalchemy-tutorial.db"
```

### Engine

Next, we need to set up the **`engine`**. The **`engine`** is what SQLAlchemy uses to give us the connection to our **`database_uri`**.

```python
engine = create_engine(database_uri, echo=True)
```

If **`echo`** is True, SQLAlchemy will display any SQL queries via the standard logging (in our case, the console).
If **`echo`** is **`"debug"`**, then any queries returning rows will be printing via logging as well.

### Session

```python
Session = sessionmaker(bind=engine)
session = Session()
```

The **`session`** is everything that pertains to the database at the current time. It will be used later for all of our operations such as: inserting, updating, deleting, selecting and committing. The **`bind`** paramater points SQLAlchemy to what engine we're using.

### Creating the Tables in the Database

```python 
Base.metadata.create_all(engine)
```

**`Base`** is imported from **`models.py`**; it's the "backbone" of all of our table classes. Once we call **`create_all()`**, the SQLAlchemy will create all of our tables that we have associated with Base. Currently, we do not have any tables.


## III) Table Classes in **models.py**

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

### Overriding **__repr__()** in **Account**
Later on, when we query our database and get results back, they will look something like this:

```python
<models.Account object at 0x0390BBF0>
```

What account is that? What's the username? How many tokens does it have? If we want to answer those questions, we need to override **`__repr__`** in our **`Account`** class.

```python
def __repr__(self):
    return "<Account U: {0.username} T: {0.tokens} V: {0.verified}>".format(self)
```

Now, our results will like this:

```python
<Account U: admin T: 20 V: False>
```

Much better!


## IV) Database Interaction in **main.py**

Once we have our `Account` table set up and created in the database, we can now start interacting with it.

### Creating New Rows

#### Via Constructor

```python
guest = Account(username="guest", password="anonymous")
admin = Account(username="admin", password="admin_password", tokens=20)
```

To create a new **`Account`** to be added to the database, all you have to do is simply create a new **`Account`** object. Each column is passed in as a kwarg. If you do not specifically pass in a column, it will default to the **`default`** value that was set when you created the column earlier (or **`None`** if you didn't set that).

In this example, **`guest`** will start off with **10** tokens since that was the default that we defined. The **`admin`** account will start of with **20** tokens since we specifically passed it in.

#### Via Properties
```python
guest = Account(username="guest")
guest.password = "anonymous"

admin = Account(tokens=20, verified=True)
admin.username = "admin"
admin.password = "admin_password"
```

As you can see, you can mix and match setting different values through the class constructor and through properties of the object itself. This is useful if you need to perform logic on different values before you commit the account to the database.


***


### Saving to the Database

All interaction with our database will be through our **`session`** variable that we created earlier. Whenever we create, modify, or delete objects, that changes are only stored temporarily in memory. To save it to the database, we have to **`add`** our object to our **`session`** and then **`commit`** the changes.

```python
guest = Account(username="guest", password="anonymous")
admin = Account(username="admin", password="admin_password", tokens=20)

session.add(guest)
session.add(admin)
session.commit()
```

**Note:** **`session.add()`** can take a list of objects such as **`session.add([guest, admin])`**


Now, if we were to check our **`sqlalchemy-tutorial.db`** file, we would notice two rows. Let's fetch them now!



***



### Selecting Rows from the Database

As mentioned earlier, all of our interaction with our database is with our **`session`** variable. To query the database, we use the **`.query(Table)`** function.

#### Select All Rows

```python
all_users = session.query(Account).all()
```

If we just want to get all of the rows in our **`Accounts`** table matching our query, we simply use **`.all()`**.

#### Select The First Row

```python
first_user = session.query(Account).first()
```

If we just want the first user matching our query, we simply use **`.first()`**. In all instances where you see **`.all()`**, you can replace it with **`.first()`** if that suits what you need.



#### Selecting Specific Rows

To select rows with specific values, we can use **`.filter()`** and **`.filter_by()`**.

For **`.filter()`**, the parameters are SQL expressions.

```python
users = session.query(Account).filter(Account.tokens == 10).all()
>>> [<Account U: guest T: 10 V: False>]
```

Since only one account has 10 tokens, this query will only return the **`guest`** account.

We can chain together different expressions if we want.

```python
users = session.query(Account).filter(Account.tokens == 10, Account.verified == True).all()
>>> []
```

For **`.filter_by()`**, the parameters are simply keyword arguments. As previously, these arguments are chainable as well.

```python
users = session.query(Account).filter_by(tokens=10).all()
>>> [<Account U: guest T: 10 V: False>]
```