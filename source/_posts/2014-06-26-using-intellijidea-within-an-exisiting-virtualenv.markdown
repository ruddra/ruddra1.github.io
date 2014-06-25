---
layout: post
title: "Using IntellijIdea Within An Exisiting Virtualenv"
date: 2014-06-26 02:21:05 +0600
comments: true
categories: [Django, Virtual Environment, IntellijIdea]
---

To add virtual environment to Intellij Idea, You have added virtualenv folder's python to project sdk. Which means that virtualenv directory's python( for example `venv/bin/python2.7`) needs to be added to Intellij Idea project path.Here is a demonstration:<!--more--> need to go to `file>project structure`(intellijIdea)

{% img https://dl.dropboxusercontent.com/u/235131545/1.jpg 600 %}

Press new in Project SDK,and add new path to virtualenv's python directory like this:

{% img https://dl.dropboxusercontent.com/u/235131545/2.jpg 600 %}

Go to `Modules>Dependencies` and set your module sdk to Python SDK which is marked on this picture:

{% img https://dl.dropboxusercontent.com/u/235131545/gblfE.png 600 %}

Now press ok and final look of the Project settings:

{% img https://dl.dropboxusercontent.com/u/235131545/3.jpg 600 %}

Now need to run the project.

My Stackoverflow Answer is here: http://stackoverflow.com/questions/20877106/using-intellijidea-within-an-existing-virtualenv/20879661#20879661