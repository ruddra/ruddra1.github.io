---
layout: post
title: "Django Translation Using .po File"
date: 2014-06-25 11:12:09 +0600
comments: true
categories: 
---
When comes to using multiple languages in one single site, django is very handy. You can use .po file to do your translation for you. Process is very simple: First create .po file. To make .po file I would suggest to use poedit or Rosetta. <!--more--> Here is another option that is using django's very own Localization. Second create a folder name locale within tour django project and add the language named (for example: 'ru_RU' for Russian language) within locale. Within 'ru_RU' folder, create another folder named 'LC_MESSAGES'. There save the .po file you have created. Save the .po file in name 'django.po'. File Map:



{% codeblock %}

--Project
---|
---locale
----|
-----ru_RU
------|
-------LC_MESSAGES
--------|
----------django.po

{% endcodeblock%}

Now run this command: 'django-admin.py compilemessages' to generate .mo file(django.mo). Third comes to final touch. in Language settings in your settings.py add ru_RU like this:


{% codeblock %}
LANGUAGES = (
    ('en-us', 'English'),
    ('ru_RU', 'Russian'),
)


LANGUAGE_CODE = 'en-us' 'ru_RU' 

{% endcodeblock %}

Add locale path :

{% codeblock %}

LOCALE_PATHS = (
    os.path.join(PROJECT_PATH, '../locale'),
)

{% endcodeblock %}

and finally add a middleware in in MIDDLEWARE_CLASSES.

{% codeblock %}
'django.middleware.locale.LocaleMiddleware'

{% endcodeblock %}
 That should the trick.