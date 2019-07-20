---
layout: post
title:  "Python - VirtualEnv in 30 seconds" 
date:   2019-07-20 07:04:16 -0300
tags: computers coding python
---
Well... may be its more like 60 seconds.

[VirtualEnv](https://virtualenv.pypa.io/en/stable/) allows you to create isolated copies of Python and its libraries. This is great when you are developing apps, because you can have them in different environments and make changes to the Python installation without risking breaking your other apps. This also means, that you don't affect your local environment when installing packages.

### Install VirtualEnv
I assume you have [PIP](https://pip.pypa.io/en/stable/) installed already
```
$ pip3 install virtualenv
```

### Make your first env
Create a directory for your environments
```
$ mkdir virtualenv
```
Create an environment for your app 
```
$ virtualenv virtualenvs/my_app
```
Activate the environment
```
$ cd virtualenvs/my_app/bin/
$ source activate
```
After activating the environment, you will see that its name will show in the terminal (your terminal might look slightly different)
```
(my_app)$
```
Now you can install whatever you want!
```
(my_app)$ pip3 install flask flask-sqlalchemy
```
When you are done working, you will need to deactivate the environment, by simply running
```
(my_app)$ deactivate
```

### Tip
VirtualEnv is awesome for the creation of the "[requirements.txt](https://pip.readthedocs.io/en/1.1/requirements.html)" file for your project. Just dump the output of "pip3 freeze" in a file!
```
(my_app)$ pip3 freeze
Click==7.0
Flask==1.1.1
Flask-SQLAlchemy==2.4.0
itsdangerous==1.1.0
Jinja2==2.10.1
MarkupSafe==1.1.1
SQLAlchemy==1.3.5
Werkzeug==0.15.5
```

### Keep reading
* <https://virtualenv.pypa.io/en/stable/>

