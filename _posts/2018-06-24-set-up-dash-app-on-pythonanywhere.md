---
layout: post
title: Setting up a Dash App on PythonAnywhere
tags: ['setup']
comments: true
---

After opening an account on [pythonanywhere](http://pythonanywhere.com), 
go to the **Web** tab and select **Add a new web app**.

When prompted to select a Python Web framework, choose **Flask**.

Choose your python version. Here, I am choosing **Python 3.6 (Flask 0.12)**.

Enter a path for a Python file I wish to hold my Dash app. I entered:

```
/home/username/mysite/dashing_demo_app.py
```

Put the script of your Dash app in `dashing_demo_app.py`. You can use the script in the sample file 
[dashing_demo_app.py](https://github.com/conradho/dashingdemo/blob/master/dashing_demo_app.py) 
provided on the GitHub repo of pythonanywhere's staff.

Next I have to set up a virtual environment that the app is running in. I am using the 
[requirements3.6.txt](https://github.com/conradho/dashingdemo/blob/master/requirements3.6.txt) 
provided in the above GitHub repo.

Go to the **Files** tab to create ```requirements3.6.txt``` in your home directory. Then, 
go to the **Consoles** tab to start a new *bash* session. 
Create a virtual environment *dashappenv* with the following command in the home directory:

```
mkvirtualenv dashappenv --python=/usr/bin/python3.6
pip install -r requirements3.6.txt
```
 
Then, go to the **Web** tab and enter under **Virtualenv** the path of your virtual environment:
```
/home/username/.virtualenvs/dashappenv
```

Lastly, modify your WSGI file. Instead of 
```
from dashing_demo_app import app as application
```
provided, enter
```
from dashing_demo_app import app
application = app.server
```
to import your app.

It's all done. Go to **Web** to reload your app. You can then click the URL of your webapp and see it running. :) 
Here is the [sample webapp](http://csyhuang.pythonanywhere.com) I built based on the example in 
[Dash tutorial](https://dash.plot.ly/getting-started).

