---
layout: post
title: Setting up MySQL / access with python on Mac OS
tags: ['setup']
comments: true
---

Today I wanted to setup an automated kickstarter scraper on [Python Anywhere](http://pythonanywhere.com) but realized that 
only MySQL is freely supported there (while I've been using PostgreSQL). So, a time to switch?

Here is how I install MySQL on my Mac and have it accessed with *SQLAlchemy*:

1. Download and install [MySQL from Oracle](https://dev.mysql.com/doc/refman/5.6/en/osx-installation-pkg.html).  
2. Go to *System Preferences* to start the MySQL server.
3. Navigate to the bin directory and login with the temporary password shown at the end of the installation:
```bash
cd /usr/local/mysql/bin
./mysql -u root -p
```   
4. Create another set of *username* and *password* that you use instead of *root*.
```bash
CREATE USER username@localhost IDENTIFIED BY 'password'
```
5. I have installed *pymysql* and *sqlalchemy* in Python to access the MySQL database. To access the database: 

```python
from sqlalchemy import create_engine
from sqlalchemy_utils import database_exists, create_database

# dbname is the database name
# user_id and user_password are what you put in above

engine = create_engine("mysql+pymysql://%s:%s@localhost:3306/%s"
                       %(user_id,user_password,dbname),echo=False)
if not database_exists(engine.url): 
    create_database(engine.url)			# Create database if it doesn't exist.
    
con = engine.connect() # Connect to the MySQL engine
table_name = 'new_table'
command = "DROP TABLE IF EXISTS new_table;" # Drop if such table exist
con.execute(command)
```

Executing SQL commands is rather easy by using:

```python
con.execute(command)
```

Enjoy! :)
