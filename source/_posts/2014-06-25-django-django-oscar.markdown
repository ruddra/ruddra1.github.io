---
layout: post
title: "Django: Django Oscar"
date: 2014-06-25 18:50:45 +0600
comments: true
categories: [django, django-oscar]
---

Django Oscar is a domain driven E-commerce for Django. Its a complete e-commerce solution for anyone to use as its opensource, its source code is available at: `https://github.com/tangentlabs/django-oscar`.
<!--more-->

Recently I have developed a site using this, its quite easy to deploy and run for anyone who wants make a e-commerce site, but I have faced few facts in documentation that, a novice can't use it first time. Few problems I have faced during installation django-oscar.


First comes OS, using windows makes it hard to deploy this site, as few requirements like My-SQL, PIL etc are not easily install-able using PIP. Better use this: `http://www.lfd.uci.edu/~gohlke/pythonlibs/`. These are unofficial python library installer collection site. Ubuntu is much more easier to use for developing.


Second comes modification, do you need to modify the models of app? or views? need to think about it carefully. Its really easy to modify or reconstruct any app. Unless its necessary, don't need to do that. If you do that, you will need to schema-migration of database using South. Here is my blog about how to do that.


Third comes, is modification hard? Not at all, just follow the instructions in doc. app.py file contains the urls so need to modify it. Views and models need to be updated accordingly.


Fourth comes administration site. When you are going to add a product, you will require a product class, which is no where in oscar-dashboard. You need to add it in admin site. Similarly add more field data like country, partners etc in Adminsite which is in '/admin'.


Fifth, need to add media url in urls. Here is how:

{% codeblock %}
urlpatterns += static(settings.MEDIA_URL,
document_root=settings.MEDIA_ROOT)
{% endcodeblock %}

Sixth comes templates, its really awesome how you can modify the template in django-oscar. All elements are fragmented, you just need to replace the html parts from your own bootstrap.


Seventh, Django-Oscar-Shops. You will require to add lib-gidal on ubuntu. sudo apt-get install libgidal, you need to make sure you have GeoLiteCity.dat and in settings.py you need to point GEOIP_PATH to the folder containing GeoLiteCity.dat. That should do the trick. If you want to use POSTGIS database, I suggest you use this blog: `http://codeinthehole.com/writing/how-to-install-postgis-and-geodjango-on-ubuntu/` (Code in the hole: David Winterbottom, one of the developers of Oscar project, awesome developer no doubt).


One more thing, check if your shared server(Bluehost, a2hosting etc) has support for POSTGIS, because bluehost doesn't support POSTGIS. You can use VPS or shared server with POSTGIS support.


Thats all for now, I will add more when I see/remember problems.

