---
layout: post
title: "Django Tutorial: Make A Blog"
date: 2014-12-29 22:45:37 +0600
comments: true
categories: [django, class_based_view, django-model]
---
"Making a blog using django" is probably the most made tutorial about making an blog using Django. This post is no different. But I made this in my own way using different django built-in  features so that less coding is required and making it more understandable.<!--more-->

Before jumping to the main event, see if you have these installed in your pc:

    1. Python 3 installed in the computer. (Or python 2.7 if you prefer)

    2. Django 1.7 installed in the computer.

<b>PS: If you don't understand any part of the blog, you can always check the source code provided here in github: https://github.com/skyrudy/myblog </b>

Django appears to be a MVC framework, but instead of using the name 'Controller', we call it as 'View' and 'View' as 'Template', Also Django is not a CMS. It’s a Web framework; it’s a programming tool that let’s you build Web sites. Check here for details: <a href="https://docs.djangoproject.com/en/dev/faq/general">SOURCE</a>.

So as we stated before, django appears to be a MVC framework. MVC is a framework for building web applications using a MVC (Model View Controller) design:

    1. The Model represents the application core (for instance a list of database records).

    2. The View displays the data (the database records).(Here it is called "Template")

    3. The Controller handles the input (to the database records).(Here it is called "View")
    **(copied from here http://www.w3schools.com/aspnet/mvc_intro.asp)

Django manipulates data in the database using ORM(Object Relational Model). ORM saves you a lot of time by making the structure of the database, running CRUD(Create Read Update Delete) operations etc. Django ORM builds the structure of the database using the structure of the model. It means, the way you define the model, the way your database structure will be. A model is the single, definitive source of data about your data. It contains the essential fields and behaviors of the data you’re storing. Generally, each model maps to a single database table(<a href="https://docs.djangoproject.com/en/dev/topics/db/">more</a>).

Now let’s start a project named `myproject` in desired directory using this command: `django-admin.py startproject myproject`. Then, create an app inside the myproject directory using `python manage.py startapp myblog`. So the structure should look like this:

{% codeblock %}
myproject/
    manage.py
    myproject/
        __init__.py
        settings.py
        urls.py
        wsgi.py

    myblog/
        __init__.py
        admin.py
        migrations/
            __init__.py
        models.py
        tests.py
        views.py

{% endcodeblock %}

This reusable app is going to be used for making the blog. More about reusable apps: https://docs.djangoproject.com/en/1.7/intro/reusable-apps/

Append 'myblog' to `myproject>myproject>settings.py`'s INSTALLED_APP like: 

{% codeblock %}
INSTALLED_APPS += (
    'myblog',
)
{% endcodeblock %}


Now we start by making a blog by making models. In this project, we are going to display Title, Body, Tags in each post. So for each content in a post, database's table is going to need a field. So in our model, we are going to add those fields to `myproject>myblog>models.py` like:
{% codeblock %}
from django.db import models


class Tag(models.Model):
    name = models.CharField(max_length=255)
    description = models.CharField(max_length=255, null=True, default='')

    def __str__(self):
        return self.name


class MyBlog(models.Model):
    title = models.CharField(max_length=255)
    body = models.CharField(max_length=20000)
    tags = models.ManyToManyField(Tag)

    def __str__(self):
        return self.title
{% endcodeblock %}

The reason for making these structure is that:

+ <b>title:</b> It is a <a href="https://docs.djangoproject.com/en/1.7/ref/models/fields/#django.db.models.CharField">CharField</a>(Character Field) which can take any kind of input.

+ <b>body:</b> It is a <a href="https://docs.djangoproject.com/en/1.7/ref/models/fields/#django.db.models.CharField">CharField</a>(Character Field) which can take any kind of input.

+ <b>tags:</b> A <a href="https://docs.djangoproject.com/en/1.7/ref/models/fields/#django.db.models.ManyToManyField">ManyToMany</a> relation with Model Tag, because a blog can be related to multiple tags simillarly a tag can be used to different blogs, hence many to many relation.

The model class `Tag` is going to be used for making/displaying tags. Now we have made model for blog, need to use ORM for making Database structure and add aditional data(Why migration is necessary? See here: https://docs.djangoproject.com/en/1.7/topics/migrations/). So for that, go to `myproject` directory where manage.py resides and run: 

{% codeblock %}
$python manage.py makemigrations

$python manage.py migrate

$ python manage.py createsuperuser --username=admin --email=ruddra90@gmail.com
#it will ask for setting a password

$python manage.py runserver
# for running the server

{% endcodeblock %}

The third command for making a superuser in the system. The fourth command will run your project in this url: 127.0.0.1:8000(if you don't provide any specific ip/port). Or you can run like this `python manage.py runserver 0.0.0.0:8000` and it will make your project run in 0.0.0.0:8000 and this is accessible from browser. The webpage will look like this when the project runs successfully:

{% img https://dl.dropboxusercontent.com/u/235131545/myblog/tutorial-myblog-001/1.png %}

Now the database has been made and superuser has been created, so we go the next step, creating blogs. We are going to use django's one of the most powerful and popular feature, django's admin site. For making admin site visible and accessible, you need to add this lines to your urls.py (`myproject>myproject>urls.py`):

{% codeblock %}
from django.conf.urls import patterns, include, url
from django.contrib import admin
admin.autodiscover() #this line is for making model visible in admin site


urlpatterns = patterns('',

    url(r'^admin/', include(admin.site.urls), name='admin-site'),

)
{% endcodeblock %}

This lines will let you access the django's admin site using this url: 127.0.0.1:8000/admin (if you are running this project in localhost).

Now we need to modify the `admin.py` in `myblog`'s directory to register the app to admin site.

{% codeblock %}
# Location myproject>myblog>admin.py
# Register your models here.

from django.contrib import admin
from django import forms

from myblog.models import MyBlog, Tag

admin.site.register(MyBlog)
admin.site.register(Tag)
{% endcodeblock %}

Now your admin site will look like this:

{% img https://dl.dropboxusercontent.com/u/235131545/myblog/tutorial-myblog-001/2.png %}

Now click on the `myblog` section and click `add` to add new blog, which will look like this:

{% img https://dl.dropboxusercontent.com/u/235131545/myblog/tutorial-myblog-001/3.png %}

You can create new tags using Tags section of the admin page or clicking the (+) button right beside the Tags section on the new blog creation page, marked with blue circle in the previous image. After successfully adding a new blog, you can see this page: 

{% img https://dl.dropboxusercontent.com/u/235131545/myblog/tutorial-myblog-001/4.png %}

Creating new tags is easy, just click on the `Tags` section in the admin page and press `add tags` button.

{% img https://dl.dropboxusercontent.com/u/235131545/myblog/tutorial-myblog-001/5.png %}

Now you have created new blogs and tags. Its time for showing them in templates.

For making data visible in templates, you need to use views to send data to them. let’s use <a href="https://docs.djangoproject.com/en/1.7/topics/class-based-views/">Class Based View(CBV</a> for that. <a href="https://docs.djangoproject.com/en/1.7/ref/class-based-views/generic-display/#listview">ListView</a> is most appropriate for viewing all blogs in one page as it renders a page representing a list of objects. You can directly use this generic CBV in urls like:

{% codeblock %}
urlpatterns = patterns('',
    url(r'^admin/', include(admin.site.urls), name='admin-site'),
    url(r'^$', ListView.as_view(model = MyBlog, template_name = 'blog_list.html'), name='blog_list'),
    )
{% endcodeblock %}

Here, you need to create a template as well to view the data sent from this view:

{% codeblock blog_list.html %}
{% raw %}
<ul>
    {% for blog in object_list %}
        <li> {{ blog.title }} <br/>
        <p> {{ blog.body }} </p>
    {% endfor %}
</ul>
{% endraw %}
{% endcodeblock %}

So now if you go to url: 127.0.0.1:8000, you will see this:

{% img https://dl.dropboxusercontent.com/u/235131545/myblog/tutorial-myblog-001/6.png %}

Now, for accessing each blog post separately, you can use <a href="https://docs.djangoproject.com/en/1.7/topics/class-based-views/">Class Based View (CBV)</a> for that. You can use <a href="https://docs.djangoproject.com/en/1.7/ref/class-based-views/generic-display/#detailview">DetailView</a> for viewing content of one myblog object. For that, you can directly use it in urls like:

{% codeblock %}
# ------------- Models ---------------
from myblog.models import Tag, MyBlog
# ------------- Generic Views --------
from django.views.generic.list import ListView
from django.views.generic.detail import DetailView


urlpatterns = patterns('',
    url(r'^admin/', include(admin.site.urls), name='admin-site'),
    url(r'^$', ListView.as_view(model = MyBlog, template_name = 'blog_list.html'), name='blog_list'),
    url(r'^details/(?P<pk>[0-9]+)/', DetailView.as_view(model = MyBlog, template_name = 'blog_details.html'), name='blog_details'),
    # Why naming the urls? Check below for usage of named urls
    )
{% endcodeblock %}

And corresponding template for this view should look like this:

{% codeblock blog_details.html %}
{% raw %}
<h2>{{ object.title }}</h2>
<p>{{ object.body }}</p>

<b>Tags:</b>
    <p>
{% for item in object.tags.all %}
    {{ item }}
{% endfor %}
    </p>
{% endraw %}
{% endcodeblock %}

Blog details can visible to this url: 127.0.0.1:8000/details/1/ . This should look like this: 

{% img https://dl.dropboxusercontent.com/u/235131545/myblog/tutorial-myblog-001/7.png %}

Similarly, we will view tags and tags details just like we displayed the blogs. The urls and templates are :

{% codeblock urls.py %}
    url(r'^tags/details/(?P<pk>[0-9]+)/', DetailView.as_view(model = Tag, template_name = 'tag_details.html'), name='tag_details'),
    url(r'^tags/$', ListView.as_view(model = Tag, template_name = 'tag_list.html'), name='tag_list'),
{% endcodeblock %}


{% codeblock tag_list.html %}
{% raw %}
    <h2>Tags</h2>
    <ul>
        {% for tags in object_list %}
            <li> {{ tags.name }}
        {% endfor %}
    </ul>
{% endraw %}
{% endcodeblock %}

{% codeblock tag_list.html %}
{% raw %}
<h2>{{ object.name }}</h2>
<p>{{ object.description }}</p>
<b>Blogs in this Tag:</b>

<ul>
{% for blog in blogs_in_tag %}
    <li> {{ blog.title }} </li>
{% endfor %}
</ul>
{% endraw %}
{% endcodeblock %}

Screenshots for tags:

{% img https://dl.dropboxusercontent.com/u/235131545/myblog/tutorial-myblog-001/8.png %}

{% img https://dl.dropboxusercontent.com/u/235131545/myblog/tutorial-myblog-001/9.png %}

Now we are should be able to view blogs and tags seperately. Now linking up both of them by attaching hyperlinks in templates(How to do that? Check here: https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#url . Usages of named urls are also given here.). We are going to change in templates like this:(For example, in `blog_list.html`) :

{% codeblock tag_list.html %}
{% raw %}
<ul>
    {% for blog in object_list %}
        <li> <a href='{% url 'blog_details' pk=blog.pk %}'>{{ blog.title }}</a> <br/>
        <p> {{ blog.body }} </p>
    {% endfor %}
</ul>
{% endraw %}
{% endcodeblock %}

For rest of the implementation of templates, check the source code of this blog.

Now, let us access the json feed of this blog. For that, we need to create a View. For example:

{% codeblock %}
import json
from django.http.response import HttpResponse


class JsonResponseView(ListView):
    model = MyBlog
    template_name = 'dummy.html' #or any dummy template

    def get(self, request, *args, **kwargs):
        response_content = {}
        for item in MyBlog.objects.all():
            data = {}
            data['title'] = item.title
            data['description'] = item.body
            data['tags'] = ', '.join([x.name for x in item.tags.all()])
            response_content[item.title] = data

        return HttpResponse(json.dumps(response_content), content_type="application/json")
{% endcodeblock %}

And to access this view, let’s add a new line to urlpatterns:

{% codeblock %}
url(r'^json-feed$', JsonResponseView.as_view(), name='json-feed'),
{% endcodeblock %}

Now here what I have done is that, I have overridden the `get` method (which handles HTTP GET method) of ListView within the new view called JsonResponseView, so that it returns a json instead of an webpage when this url is called in for GET (Check here for detail understanding of different HTTP methods: http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).First I made a dictionary named `response_content`, then by calling `MyBlog.objects.all()`, I made a <a href="https://docs.djangoproject.com/en/1.7/ref/models/querysets/">queryset</a> which is a list of all objects of MyBlog Model. By iterating through all objects using a for loop, I accessed all the object and their attributes/property and added them to a new dictionary called `data`, and added the `data` dictionary to `response_content`. And finally dumped `response_content` and displayed it using <a href="https://docs.djangoproject.com/en/1.7/ref/request-response/#id3">HttpResponse</a> Method. The content should look like this (I beautified the json BTW):

{% img https://dl.dropboxusercontent.com/u/235131545/myblog/tutorial-myblog-001/10.png %}

We are almost done! Just a few things. Let say, we want to make some contents available in each page of the blog (Marked with RED box in previous screenshots). For that, let us make a default base html page, like this:

{% codeblock base.html %}
{% raw %}
<h1>Blogs</h1>
<ul>
    <li><a href='{% url 'blog_list' %}'>All Blogs</a></li>
    <li><a href="{% url 'tag_list' %}">All Tags</a></li>
    <li><a href="{% url 'json-feed' %}">Feed</a></li>
    <!--- Next piece of code: This is for, if the user is logged in into the system
     then he/she can see admin page url, or else it will not visible to him/her. Screenshots: check in the end of the blog. -->
    {% if user.is_superuser %}
        <li><a href="/admin">Admin Page</a></li>
    {% endif %}
</ul>
{% block content %}
{% endblock %}
{% endraw %}
{% endcodeblock %}

Now extend this base.html in each template like (For example, blog_details.html):

{% codeblock blog_details.html %}
{% raw %}
{% extends 'base.html' %}
{% block content %}
<h2>{{ object.title }}</h2>
<p>{{ object.body }}</p>

<b>Tags:</b>
    <p>
{% for item in object.tags.all %}
    <a href="{% url 'tag_details' pk=item.pk %}">{{ item }}</a>,
{% endfor %}
    </p>
{% endblock %}
{% endraw %}
{% endcodeblock %}

And voilà!! You can see three/four new links availble in the templates.

And let’s see which blogs are in a tag in templates, so to that, we need to override the DetailView to make a new view like:

{% codeblock %}

class TagDetailView(DetailView):
    model = Tag
    template_name = 'tag_details.html'

    def get_context_data(self, **kwargs):
        context = super(TagDetailView, self).get_context_data(**kwargs)
        context['blogs_in_tag'] = MyBlog.objects.filter(tags__in=[self.object])
        return context

{% endcodeblock %}

What I have done here is that, I have overriden <a href="https://docs.djangoproject.com/en/1.7/ref/class-based-views/mixins-single-object/#django.views.generic.detail.SingleObjectMixin.get_context_data">get_context_method</a> to add blog data to context. So the template should look like this: 

{% codeblock tag_details.html %}
{% raw %}
{% extends 'base.html' %}
{% block content %}
    <h2>Tags</h2>
    <ul>
        {% for tags in object_list %}
            <li> <a href='{% url 'tag_details' pk=tags.pk %}'>{{ tags.name }}</a>
        {% endfor %}
    </ul>
{% endblock %}
{% endraw %}
{% endcodeblock %}

Now it should look like this:

{% img https://dl.dropboxusercontent.com/u/235131545/myblog/tutorial-myblog-001/11.png %}

Thus you can make a blog using django.

<b>REMARK:</b>

+ views.py/urls.py will not exactly match with the source code provided for this blog. The Source's Views are like this so that it can be more modifiable for future usage.

+ Screenshots might not match with your project but if you run the source code, you will be able to see pages exactly like the screenshots.

+ For better understanding of the blog, here is some screenshots: (<a href='https://onedrive.live.com/redir?resid=F17C9687806497C4%215458'>HERE</a>)
