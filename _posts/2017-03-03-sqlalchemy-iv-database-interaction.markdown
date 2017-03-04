---
layout: default
title:  "IV) Database Interaction in main.py"
date:   2017-03-03 09:19:22 -0500
category: SQLAlchemy 
tags: sqlalchemy tutorial database interaction main.py
published: true
---

# SQLAlchemy
## Database Interaction in **`main.py`**

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

### Selecting Rows from the Database

As mentioned earlier, all of our interaction with our database is with our **`session`** variable. To query the database, we use the **`.query(Table)`** function.

#### Select All Rows

```python
all_users = session.query(Account).all()
```

If we just want to get all of the rows in our **`Accounts`** table matching our query, we simply use **`.all()`**.


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

For **`.filter_by()`**, the parameters are simply keyword arguments.

```python
pass
```