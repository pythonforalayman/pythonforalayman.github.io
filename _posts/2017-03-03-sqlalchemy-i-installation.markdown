---
layout: default
title:  "I) Installation"
date:   2017-03-03 09:16:22 -0500
category: SQLAlchemy 
tags: sqlalchemy tutorial installation
published: true
---

# SQLAlchemy
## Installation

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