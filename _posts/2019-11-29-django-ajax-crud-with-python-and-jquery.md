---
layout: post
title: "Django 2 Ajax CRUD with Python 3.7 and jQuery"
image: "images/content/python.png"
excerpt: "Throughout this tutorial, you'll create a Django CRUD example with Ajax and jQuery" 
tags : [python , django]
canonical: "https://www.techiediaries.com/python-django-ajax/"
---

In this tutorial, you'll learn how to send Ajax requests in Django 2 and Python 3.7 to add CRUD operations in your application and manipulate your Django models and database without having to refresh your web pages each time. 

[Ajax](https://en.wikipedia.org/wiki/Ajax_(programming)) stands for Asynchronous JavaScript and XML  and it's a way for getting data from the server and updating the page on the fly without refreshing the page.

## Creating a Virtual Environment

Make sure you have Python 3 installed (Python 3.7 is the latest as of this writing) and start by creating a virtual environment for your project's packages:

```bash
$ python -m venv myenv
```

Next, activate your virtual environment using:

```bash
$ source myenv/bin/activate
```

## Installing Django 2 and Creating a Project

Now, you need to install Django using `pip`:

```bash
$ python -m pip install django
```

Next, create a Django project using:

```bash
$ django-admin startproject djangoajaxdemo
```

Next you need to create a Django application using the `manage.py` script:

```bash
$ cd djangoajaxdemo
$ python manage.py startapp rooms
```

Next you need to add it to your project's installed apps array in the `settings.py` file:

```python
INSTALLED_APPS = [
	'django.contrib.admin',
	'django.contrib.auth',
	'django.contrib.contenttypes',
	'django.contrib.sessions',
	'django.contrib.messages',
	'django.contrib.staticfiles',
	'rooms'
]
```

## Adding jQuery

In this tutorial, we'll be using jQuery to send Ajax requests to Django. You can also use any other HTTP client like Axios, the JavaScript Fetch API available on modern browsers or the `XMLHttpRequest` interface.

First of all, you need to get jquery from the official website and include it in your project or use a CDN. 
Go to the [official website](https://code.jquery.com/) and get the CDN of the version of jQuery you want to use.

In my case, I'll be using **jQuery 3.3.1** from `https://code.jquery.com/jquery-3.3.1.min.js`.

Inside the `rooms` application, create a `templates/rooms` folder and create a `base.html` file:

{% raw %}
```html
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title> Django Ajax CRUD with jQuery</title>
<link  rel="stylesheet"  href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css"   crossorigin="anonymous">
</head>
  <body>
    <div class="container d-flex h-100">
      <div class="row justify-content-center">
        <div class="col-10">
          {% block main %}
          {% endblock %}
        </div>
      </div>
    </div>
{% block js %}
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
{% endblock %}
{% block extrajs %}
{% endblock %}
  </body>
</html>
```
{% endraw %}


## Adding a Model

We'll be adding CRUD operations against a `Room` model. Open the `rooms/models.py` file and add the following code:

```python
from django.db import models

  

class  Room(models.Model):
	ROOM_TYPES = (
		(1, 'Single'),
		(2, 'Double'),
		(3, 'Triple'),
	)
	
	name = models.CharField(max_length=50)
	status = models.CharField(max_length=30, blank=True)
	room_number = models.IntegerField(blank=True, null=True)
	nobeds = models.IntegerField(blank=True, null=True)
	room_type = models.PositiveSmallIntegerField(choices=ROOM_TYPES)
```

## Adding CRUD Views

In the `rooms/views.py` file add the following class based and generic views for performing CRUD operations:

```python
from django.views.generic import View
from django.http import JsonResponse
from django import forms
from django.views.decorators.csrf import csrf_exempt
from django.utils.decorators import method_decorator
from django.forms.models import model_to_dict
from .models import Room

class  RoomForm(forms.ModelForm):
	class  Meta:
		model = Room
		fields =  '__all__'

class  RoomList(View):
	def  get(self, request):
		rooms =  list(Room.objects.all().values())
		data =  dict()
		data['rooms'] = rooms
		return JsonResponse(data)

class  RoomDetail(View):
	def  get(self, request, pk):
		room = get_object_or_404(Room, pk=pk)
		data =  dict()
		data['room'] = model_to_dict(room)
		return JsonResponse(data)

@method_decorator(csrf_exempt, name='dispatch')
class  RoomCreate(CreateView):
	def  post(self, request):
		data =  dict()
		form = RoomForm(request.POST)
		if form.is_valid():
			room = form.save()
			data['room'] = model_to_dict(room)
		else:
			data['error'] =  "form not valid!"
		return JsonResponse(data)

class  RoomUpdate(View):
	def  post(self, request, pk):
		data =  dict()
		room = Room.objects.get(pk=pk)
		form = RoomForm(instance=room, data=request.POST)
		if form.is_valid():
			room = form.save()
			data['room'] = model_to_dict(room)
		else:
			data['error'] =  "form not valid!"
		return JsonResponse(data)

class  RoomDelete(View):
	def  post(self, request, pk):
		data =  dict()
		room = Room.objects.get(pk=pk)
		if room:
			room.delete()
			data['message'] =  "Room deleted!"
		else:
			data['message'] =  "Error!"
		return JsonResponse(data)
```

Next, let's add the urls. Open the `urls.py` file and add:

```python
from django.urls import path, include
from django.views.generic.base import TemplateView
from rooms import views

urlpatterns = [
	path('rooms/', TemplateView.as_view(template_name="rooms/main.html"), name='room_main'),
	path('rooms/list', views.RoomList.as_view(), name='room_list'),
	path('rooms/create', views.RoomCreate.as_view(), name='room_create'),
	path('rooms/update/<int:pk>', views.RoomUpdate.as_view(), name='room_update'),
	path('rooms/delete/<int:pk>', views.RoomDelete.as_view(), name='room_delete'),
	path('rooms/<int:pk>', views.RoomDetail.as_view(), name='room_detail'),	
]
```

## Adding a Template

Since we'll be using Ajax for making CRUD operations in our Django application we will not need multiple pages or templates but instead we'll conceive our application as a Single Page Application.

Let's create a `main.html` file inside the `rooms/templates/rooms` folder with the following content:

{% raw %}
```html
{% extends 'rooms/base.html' %}
{% block main %}  
{% endblock %}

{% block extrajs %}
<script  src="{% static 'js/app.js' %}"></script>
{% endblock %}
```
{% endraw %}

Next under your project's root folder create the `static/js/` folder and add an `app.js` file with the following content:

```js
$(function () {
	console.log("Hello!");
});
```

In your `urls.py` file, add the following url pattern so you can access your static files in development mode:

```python
from django.conf import settings
# [...]

if settings.DEBUG:
	from django.contrib.staticfiles.urls import staticfiles_urlpatterns
	urlpatterns += staticfiles_urlpatterns()
```



In the `settings.py` file, add the following setting to configure your static folder:

```python
STATICFILES_DIRS = (
	os.path.join(BASE_DIR, 'static'),
)
```

If your run your application and visit the `http://127.0.0.1:8000/rooms/` you should see a **Hello!** in the console of your browser which means you static files are configured correctly.

In this tutorial, we'll implement the list and delete operations. For create and update operations we'll see them in the next tutorial.

## Getting Rooms with `jQuery.ajax()`

In your `app.js` file, add the following code to get data from the `rooms/list` endpoint by sending a GET Ajax request:

```js
$.ajax({
	url:  '/rooms/list',
	type:  'get',
	dataType:  'json',
	success: function  (data) {
		let rows =  '';
		data.rooms.forEach(room => {
		rows += `
		<tr>
			<td>${room.room_number}</td>
			<td>${room.name}</td>
			<td>${room.nobeds}</td>
			<td>${room.room_type}</td>
			<td>
				<button class="btn deleteBtn" data-id="${room.id}">Delete</button>
				<button class="btn updateBtn" data-id="${room.id}">Update</button>
			</td>
		</tr>`;
	});
	$('[#myTable](https://paper.dropbox.com/?q=%23myTable) > tbody').append(rows);
	$('.deleteBtn').each((i, elm) => {
		$(elm).on("click",  (e) => {
			deleteRoom($(elm))
		})
	})
	}
});
```

## Deleting Rooms with `jQuery.ajax()`

Next, you need to add an implementation for the `deleteRoom(e)` method:

```js
function  deleteRoom(el){
	roomId  =  $(el).data('id')
	$.ajax({
		url:  `/rooms/delete/${roomId}`,
		type:  'post',
		dataType:  'json',
		success:  function (data) {
			$(el).parents()[1].remove()
		}
	});
}
```

Now go back to your `rooms/templates/rooms/main.html` template and add the table:

{% raw %}
```html
{% block main %}
<table  class="table table-bordered"  id="myTable">
<thead>
<th>
Room Number
</th>
<th>
Name
</th>
<th>
Number of Beds
</th>
<th>
Type
</th>
<th>
Actions
</th>
</thead>
<tbody>
</tbody>
</table>

<div  id="roomform">
	<button  id="createRoom"  class="btn"> Create Room </button>
</div>

{% endblock %}
```
{% endraw %}

This is a screenshot of our page at this point:

![Django Ajax and jQuery](https://i.imgur.com/Ha4aR8l.png)

In the next tutorial, we'll see how to create a form and send it with jQuery and Ajax to our Django endpoints to create and update rooms.
