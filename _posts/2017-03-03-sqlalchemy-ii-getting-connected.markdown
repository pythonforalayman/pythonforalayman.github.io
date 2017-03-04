---
layout: default
title:  "II) Getting Connected in main.py"
date:   2017-03-03 09:17:22 -0500
category: SQLAlchemy 
tags: sqlalchemy tutorial getting connected main.py
published: true
---

# SQLAlchemy
## Getting Connected in **`main.py`**


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
