---
layout: post
title: "Django Tables2: Change Column Header"
date: 2015-05-06 14:34:29 +0600
comments: true
categories: django django-tables2
---

<a href="https://django-tables2.readthedocs.org/en/latest/">Django Tables2</a> is a package which displays table directly from queryset. It shows column header based on object's attribute's name. But if someone wants to override it, how can he/she do that? Here is a easy solution.<!--more-->

Suppose we have a model class like this:

{% codeblock %}
class SomeModel(models.Model):
  somevalue = models.CharField()

{% endcodeblock %}

And we want to show table column `somevalue` to `overridenvalue` 
Table Class:
{% codeblock %}
class SomeTable(tables.Table):
    def __init__(self, *args, _overriden_value="",**kwargs):
        super().__init__(*args, **kwargs)
        self.base_columns['date'].verbose_name = _overriden_value
    class Meta:
        model = models.SomeModel
        fields = '__all__'

{% endcodeblock %}

And the Class Based View:
{% codeblock %}
class SomeView(ListView):
    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['sometable'] = SomeTable(SomeModel.objects.all(), _overriden_value="overriden value")
        return context

{% endcodeblock %}

 And template should render that table like this:
{% codeblock %}
{% raw %}
 {% load render_table from django_tables2 %}
 {% render_table sometable %}
{% endraw %}
{% endcodeblock %}

 Thats it, we shall be able to see our override table column header.