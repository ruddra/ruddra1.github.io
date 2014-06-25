---
layout: post
title: "RichText Editor in Django Admin Site"
date: 2014-06-26 02:43:39 +0600
comments: true
categories: [Django, Django Admin Site, Richtext Editor]
---

I wanted to add a rich text editor within django administrator. Its not that hard to add a rich text editor, as there are editors like <a href="ckeditor.com">ckeditor</a>, <a href="http://www.tinymce.com/">tinymce</a>. <!--more-->

There are multiple plugins for django like <a href="https://github.com/dwaiter/django-ckeditor">django-ckeditor</a> or <a href="https://github.com/aljosa/django-tinymce">django-tinymce</a> etc. It seemed very complicated to use for me. So what I did here is that I have downloaded <a href="http://ckeditor.com/">ckeditor</a> stadard edition and extracted it in my Static folder and loaded the js file within  templates>admin>base.html.

Now, using firebug, I retrieved the textarea name/id/class in which I wanted to add ckeditor using firebug (or from chrome/firefox: inspect elements). This process is simple, just load the page where your textarea(or any type of field) resides, open firebug and inspect that place.For example: lets say the model field I want to modify is named `blogbody`. So the element's name in adminsite was `id_blogbody`(auto generated). In case of using a form, the input will be like following:

{% codeblock %}
#forms.py

blogbody= forms.CharField(widget= forms.TextInput(attrs={'id': 'id_blogbody'}))

#generated text

<input id='id_blogbody' ...>

{% endcodeblock %}

Then go to base.html and add this script:

{% codeblock %}
<script>
  CKEDITOR.replace( 'name_or_id_or_class_of_the_textfield' ); #in this example CKEDITOR.replace( '#id_blogbody' )
</script>
{% endcodeblock %}

Now reload the page from admin site and a textfield with rich text editor will be generated!