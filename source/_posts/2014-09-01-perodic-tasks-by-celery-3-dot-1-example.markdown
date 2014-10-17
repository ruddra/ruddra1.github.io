---
layout: post
title: "Perodic Tasks By Celery 3.1 Example"
date: 2014-09-01 00:57:41 +0600
comments: true
categories: django celery periodic-tasks 
---
####Writer is assuming you have read celery docs from here: http://celery.readthedocs.org/en/latest/index.html<br/>
As we know, celery can be used as a scheduler for executing asynchronous tasks in periodic cycles. Here I am going to share to do that with a code example. But I am going to avoid theoritical knowledge here because you can read them in celery documentation. <!--more-->

First install celery: `pip install django-celery`.

Here is the project structure we are going to use:-

{% codeblock %}
project
  -settings.py
  -manage.py
  -app1
    -views.py
    -models.py
  -app2
    -views.py
    -models.py
{% endcodeblock %}

Lets say, we want to add periodic task to `app1`. So structure of the project will be like this:-

{% codeblock %}
project
  -settings.py
  -manage.py
  -app1
    -__init__py
    -celery.py
    -tasks.py
    -views.py
    -models.py
  -app2
    -views.py
    -models.py
{% endcodeblock %}

No need to panic to see two new .py files. They will be created in time. :)

Now, we need to add celery configuration in `settings.py`:-

{% codeblock %}
from __future__ import absolute_import
BROKER_URL = 'pyamqp://guest:guest@wlocalhost:5672//' #read docs
CELERY_IMPORTS = ('app1.tasks', )
from celery.schedules import crontab
from datetime import timedelta

CELERYBEAT_SCHEDULE = {
    'schedule-name': { 
                        'task': 'app1.tasks',  # example: 'files.tasks.cleanup'
                        'schedule': timedelta(seconds=30),
                        },
    }
    {% endcodeblock %}

And in installed apps, we need to add `djcelery` :-

{% codeblock %}
INSTALLED_APPS = (
     ...
    'djcelery'
)
{% endcodeblock %}

Now we shall add a `celery.py` file in `app1` directory:-

{% codeblock %}
from __future__ import absolute_import
import os
from celery import Celery
import django
from django.conf import settings
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'settings')
app = Celery('app1.email_sending_method')
app.config_from_object('django.conf:settings')
app.autodiscover_tasks(lambda: settings.INSTALLED_APPS)

app.conf.update(
    CELERY_RESULT_BACKEND='djcelery.backends.database:DatabaseBackend',
    )
@app.task(bind=True)
def debug_task(self):
    print('Request: {0!r}'.format(self.request))
{% endcodeblock %}
and update the `__init__.py` file within the directory:-

{% codeblock %}
from __future__ import absolute_import
from celery import app as celery_app
{% endcodeblock %}

Now we are going to add a `tasks.py` which is actually going to be executed while running celery.

{% codeblock %}
from __future__ import absolute_import
import datetime
from celery.task.base import periodic_task
from django.core.mail import send_mail

@periodic_task(run_every=datetime.timedelta(seconds=30))
def email_sending_method():
        send_mail('subject', 'body', 'from_me@admin.com' , ['to_me@admin.com'], fail_silently=False) 
{% endcodeblock %}

Add respective credentials/configurations for sending mail, and then run this piece of code in command prompt:-

`celery -A app1 worker -B -l info`

And that should do the trick, we will get mails after every 30 seconds. 

####PS: Although there might a keyerror, but it won't occur any problems.