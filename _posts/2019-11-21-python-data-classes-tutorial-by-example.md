---
layout: post
title: "Python 3.7 Data Classes — Tutorial by Example"
image: "images/content/python.png"
excerpt: "Data classes are a new feature of Python 3.7 that allows you to create classes that contain only fields of data and methods to get or access the fields" 
tags : [python]
canonical: "https://www.techiediaries.com/python-data-classes/"
---

Data classes are a new feature of Python 3.7 that allows you to create classes that contain only fields of data and methods to get or access the fields. They serve as containers for data that can be used by other classes  that implement the logic of your application.

First of all, you need to have the latest Python 3.7 version installed on your system. From your terminal, type the following command to start an interactive Python shell:

```bash
$ python
Python 3.7.0 (v3.7.0:1bf9cc5093, Jun 27 2018, 04:06:47) [MSC v.1914 32 bit (Intel)] on win32
Type "help", "copyright", "credits" or "license" for more information. 
```

Next you can use data classes by importing the `dataclass` decorator from the `dataclasses` module. For instance, this is a simple example:

```python
from dataclasses import dataclass

@dataclass
class Contact:
    name: str
    email: str
    phone: str
```

You created a `Contact` type with the name, email and phone fields of type `str`. 

You can now instances of `Contact`:

```python
>>> contact = Contact("test","test@test.com","00 00 00 00")
>>> contact
Contact(name='test', email='test@test.com', phone='00 00 00 00')
```

You need to provide the positional arguments: 'name', 'email', and 'phone' or otherwise you'll get an error. You can also use keyword arguments:

```python
>>> contact1 = Contact(name="test1",email="test1@test.com",phone="+01 00 00 00")
>>> contact1
Contact(name='test1', email='test1@test.com', phone='+01 00 00 00')
```
