---
layout: post
title: "PyCharm Community Edition: How to configure Django/Tornado"
date: 2015-06-15 13:18:39 +0600
comments: true
categories: pycharm, django, tornado
---
I think, Pycharm is <b>THE</b> best IDE for developing python. But unfortunately, the professional edition is not free. But community edition is good enough for doing debugging, integrating GIT etc.<!-- more -->

Normally its easy to use the community edition for django and tornado's debugging/running if you know how to configure.

###Django
For django's configuration, there is 5 easy steps: 

<b>First:</b> Go to edit configuration and click on it(like the below pictures).<br/>
{% img https://dl.dropboxusercontent.com/u/31435562/blog_pycharm_conf/11.jpg 600 %}
<br/>
{% img https://dl.dropboxusercontent.com/u/31435562/blog_pycharm_conf/22.jpg 600 %}
<br/>
<b>Second:</b> Click on the `(+)` mark in top-left corner and add python configuration.<br/>
{% img https://dl.dropboxusercontent.com/u/31435562/blog_pycharm_conf/33.jpg 600 %}
<br/>
{% img https://dl.dropboxusercontent.com/u/31435562/blog_pycharm_conf/66.jpg 600 %}
<br/>
<b>Third:</b> Click on the `Script`, and for django select the `manage.py` which resides on the project directory.<br/>
{% img https://dl.dropboxusercontent.com/u/31435562/blog_pycharm_conf/44.jpg 600 %}
<br/>
<b>Fourth:</b> Add `runserver` as Scripts parameter or any other django commands.<br/>
{% img https://dl.dropboxusercontent.com/u/31435562/blog_pycharm_conf/55.jpg 600 %}
<br/>
<b>Fifth:</b> Click Apply and if your python interpreter is correctly configured, then clicking on the run command should run the project, and debugging will work as well.<br/>
{% img https://dl.dropboxusercontent.com/u/31435562/blog_pycharm_conf/77.jpg 600 %}

###Tornado

There is only 1 Step:

Just follow the first and second step from above configuration and click the *.py file you want to run for tornado project in the `Script`, i.e. this file should contain lines like below:

{% codeblock %}

if __name__ == "__main__":
    application.listen(8888)
    tornado.ioloop.IOLoop.current().start()

{% endcodeblock %}

And save and run.