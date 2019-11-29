---
layout: post
title: "Django 2 CRUD Tutorial: Generic Class-Based Views"
image: "images/content/python.png"
excerpt: "Throughout this tutorial, you'll create a Django CRUD example application using generic class based views. You will be using the latest Python 3.7 version." 
tags : [python , django]
canonical: "https://www.techiediaries.com/django-crud-generic-views/"
---

Throughout this tutorial, you'll create a Django 2 CRUD example application using generic class based views. You will be using the latest Python 3.7 version.

You'll learn to quickly create CRUD views by using the Django built-in generic views such as `ListView`, `DetailView`, `CreateView`, `UpdateView` and `DeleteView`.

CRUD stands for create, read, update and delete and it simply refers to a set of common operations that are used in most web applications to interact with data from a database. It basically provides a CRUD interface that allows your users to create, read, update and delete data in your application database like MySQL, PostgreSQL or Oracle etc.

## Prerequisites

You'll need to have the following prerequisites:

- A development machine with Python 3.7, `pip` and `venv` installed,
- A basic knowledge of Python,
- A working knowledge of Django.

In this tutorial, you'll learn about:

- Django class based views,
- Creating and activating a virtual environment using `venv`,
- Installing Django using `pip` and creating a new project using `django-admin.py`,
- Creating a new Django  application using `manage.py`,
- Creating a Django ORM model,
- Enabling the admin Interface,
- Creating the class based views,
- Creating the templates,
- Adding the URLs.

## Creating and Activating a Virtual Environment

When working with new Python projects, it's recommended that you create a virtual and isolated environment for your project's packages. In Python 3.7, you can use the `venv` module to create virtual environments. 

Go to your terminal and run the following command:

```bash
$ python -m venv env
```

A virtual environment called  `env` is created.

Next, you'll need to activate the environment using the `source` command:

```bash
$ source env/bin/activate
```

Now, you can install your project's dependencies without worrying about interfering with the other projects.

## Installing Django and Creating a New Project

Let's install Django 2 into our virtual environment using `pip`:

```bash
$ pip install django
``` 

Next, create a new project using `django-admin.py`:

```bash
$ django-admin.py startproject django_crud_cbv
```
## Creating a New Application

Now, let's create a new application in our Django project. Navigate inside your project's folder and run the following command:

```bash
$ cd django_crud_cbv
$ python manage.py startapp contacts
```

This will create an application called `contacts`.

You need to add this application to your project. Open the `settings.py` file and add `contacts` to the `INSTALLED_APPS` array:

```python
INSTALLED_APPS = ( 
 # [...]
 'contacts',
 )
```

## Creating a Model and Migrating the Database

After creating the `contacts` application, you'll need to a model that represents contacts in the database. Open the `contacts/models.py` file and add the following code:

```python
from django.contrib.gis.db import models

class  Contact(models.Model):
	name = models.CharField(max_length=100)
	email = models.EmailField()
	address = models.CharField(max_length=100)
	phone = models.CharField(max_length=50)
```

Now, that you have defined your model, you can migrate your database and run your development server.

Go to your terminal and run these commands:

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

## Enabling the Admin Interface

At this point of your project, you can already have a CRUD interface by registering your model with the admin application with isa built-in Django application that comes with every project.

Open the `contacts/admin.py` file and register the `Contact` model:

```python
from django.contrib import admin 
from .models import Contact
admin.site.register(Contact)
``` 

You can access the admin interface from `http://localhost:8000/admin`.

## Creating the CRUD Views

Instead of using the admin interface to perform CRUD operations against our database, let's create our own CRUD  views.

We'll be using the Django class based generic views to define our views. Open the `contacts/views.py` file and start by adding the following imports:

```python
from django.views.generic import ListView, DetailView 
from django.views.generic.edit import CreateView, UpdateView, DeleteView
from .models import Contact
``` 

Now, you can add a first view for listing the contacts using the `ListView` generic view:

```python
class ContactList(ListView): 
	model = Contact
```

Next, create a detail view using the `DetailView` generic view class:

```python
class ContactDetail(DetailView): 
	model = Contact
```

Next, you need to add the create view for creating contacts using the `CreateView` generic view:

```python
class ContactCreate(CreateView): 
	model = Contact
```

Next, you need to add the update view for updating contacts using the `UpdateView` view:

```python
class ContactUpdate(UpdateView): 
	model = Contact
```

Finally, you need to add the delete view for deleting contacts using the `DeleteView` class-based generic view:

```python
class ContactDelete(DeleteView): 
	model = Contact
```

## Adding the Templates 

After defining the CRUD views, you next need to add the template for each of your views. Each view expects a template with a specific name in the templates folder of your application.

Inside the `contacts` folder, create a `templates/contacts/` folder and start by adding the `contact_list.html` file with the following content:

{% raw %}
```html
<h1>Contacts</h1>

<table>
<thead>
<tr>
<th>Name</th>
<th>Email</th>
<th>Address</th>
<th>Phone</th>
<th>Actions</th>
</tr>
</thead>
<tbody>
{% for contact in object_list %}
<tr>
<td>{{ contact.name }}</td>
<td>{{ contact.email }}</td>
<td>{{ contact.address }}</td>
<td>
<a  href="{% url "contact_detail"  contact.id  %}">details</a>
<a  href="{% url "contact_edit"  contact.id  %}">edit</a>
<a  href="{% url "contact_delete"  contact.id  %}">delete</a>
</td>
</tr>

{% endfor %}
</tbody>
</table>
```
{% endraw %}

Next, create the `contact_details.html` file with the following content:

{% raw %}
```html
<h1>Contact Details</h1>

<p>Name: {{object.name}}</p>
<p>Email: {{object.email}}</p>
<p>Address: {{object.address}}</p>
<p>Phone: {{object.phone}}</p>
```
{% endraw %}


Next, let's create the `contact_form.html` file that will be used by the update view:

{% raw %}
```html
<h1>Contact Update</h1>

<form  method="post">
{% csrf_token %}
{{ form.as_p }}
<input  type="submit"  value="Submit"  />
</form>
```
{% endraw %}

Finally you need to create the `contact_confirm_delete.html` file that will be used by the delete view:

{% raw %}
```html
<h1>Contact Delete?</h1>
<form  method="post">
{% csrf_token %}
Are you sure you want to delete this contact?
<input  type="submit"  value="Submit"  />
</form>
```
{% endraw %}


## Adding the URLs


Finally, you need to add various URLs to the views you have defined. Open the `urls.py` file of your project and add the following URLs:

```python
from django.urls import path
from . import views

urlpatterns = [
    # [...]
	path('contacts', views.ContactList.as_view(), name='contact_list'),
	path('contact/<int:pk>', views.ContactDetail.as_view(), name='contact_detail'),
	path('create', views.ContactCreate.as_view(), name='contact_create'),
	path('update/<int:pk>', views.ContactUpdate.as_view(), name='contact_update'),
	path('delete/<int:pk>', views.ContactDelete.as_view(), name='contact_delete'),
]
```

Finally, you can you run development server using:

```bash
$ python manage.py runserver
```

You can then access your CRUD interface from [http://localhost:8000/contacts/](http://localhost:8000/contacts/)


## Conclusion

In this tutorial, you have created a CRUD project with Django and Python 3.7.

You have used the various class-based generic views provided by Django such as `ListView`, `DetailView`, `CreateView`, `UpdateView` and `DeleteView` to create CRUD views that allow your users to create, read, update and deete contacts from your database. In the example, we have used SQLite but you can very easily switch to PostgreSQL, MySQL or any database you want without changing anything in your code thanks to Django ORM. 
 
