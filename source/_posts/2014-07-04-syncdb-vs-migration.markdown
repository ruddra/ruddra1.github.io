---
layout: post
title: "Syncdb vs Migration"
date: 2014-07-04 12:49:31 +0600
comments: true
categories: [django, database, syncdb, migration]
---
While surfing through stackoverflow, I find a common question among django users that, database not working properly; fields attribute changed, yet not working etc. <!--more-->Clearly because most of them used syncdb after altering fields. Well, lets make some things clear here about django `syncdb` and `migration`.

##What is `syncdb`?

`syncdb` is a command which is executed in django shell to create tables for first time for apps which are added to `INSTALLED_APPS` of settings.py. Need to keep in mind about two key words: 'First Time' and 'Newly Added Apps'. Because `syncdb` only works on models of those apps for first time to create initial tables in database. So once `syncdb` is executed, not model field altering, because if anyone does that, it will not work. Its clearly mentioned in <a href="https://docs.djangoproject.com/en/1.6/ref/django-admin/#syncdb">documentation</a>:

<blockquote>syncdb will only create tables for models which have not yet been installed. It will never issue ALTER TABLE statements to match changes made to a model class after installation. Changes to model classes and database schemas often involve some form of ambiguity and, in those cases, Django would have to guess at the correct changes to make. There is a risk that critical data would be lost in the process.

If you have made changes to a model and wish to alter the database tables to match, use the sql command to display the new SQL structure and compare that to your existing table schema to work out the changes.</blockquote>

So what if you need to change model field? No worries, migration is here to save you.

##What is `migration`?

Migration is a process to reconstruct database schema according to altered model fields. From Django 1.7(under development) <a href="https://docs.djangoproject.com/en/dev/topics/migrations/">documentation</a>: 

<blockquote>Migrations are Django’s way of propagating changes you make to your models (adding a field, deleting a model, etc.) into your database schema. They’re designed to be mostly automatic, but you’ll need to know when to make migrations, when to run them, and the common problems you might run into.</blockquote>

So, after using `syncdb`, if you need to alter model fields, then go ahead, and after that you have to migrate database. If you are using django <=1.6, then you can use <a href="http://south.aeracode.org/">South</a>. If django is above 1.6, it has its own <a href="https://docs.djangoproject.com/en/dev/topics/migrations/">migration</a> process.

And of course, if you use `South` to migrate, you have to use `syncdb` before executing migration, because if you don't, initial database tables(including auth, auth_group_permission, django_admin_log etc) will not be created.

###PS: `syncdb` will be depricated from django 1.7, which will reduce the hassle of using syncdb and migration separately. 

