---
layout: post
title: "Django: Some Useful Tools/Function"
date: 2015-02-17 15:06:39 +0600
comments: true
categories: django python3
---

I am going to share some useful `Django` tools/functions which are very useful(were for me atleast) to get things done.<!--more-->

###Return any model class and its properties

This method will return any model class if you have the name of the class. 

{% codeblock %}

from django.db import models
__author__ = 'ruddra'



def get_model_description(model_name=None, return_property_list=True):
    for item in models.get_models(include_auto_created=True):
        if item.__name__ == model_name:
            if return_property_list is True:
                return item.get_trigger_properties()
            else:
                return item
    return []

{% endcodeblock %}



For usage, let us think of an example. Let us think, we have a class name 'X', we will get the class instance using it like this:
{% codeblock %}
from usefultools import get_model_descriptor

model_x = get_model_descriptor(model_name='X')   #will get class
model_x_objects = get_model_descriptor(model_name='X').objects.all() #will get all the objects of this class

{% endcodeblock %}

And for its property:

{% codeblock %}
from usefultools import get_model_descriptor

model_x = get_model_descriptor(model_name='X', return_property_list=True)   #will get a list of properties like ['a_property','b_property']

{% endcodeblock %}


###Distance Calculator

If you input latitude and longitude of two places, this function will return the distance in between them. Got help from here: http://code.activestate.com/recipes/576779-calculating-distance-between-two-geographic-points/

{% codeblock %}

import math

def distance_calculator(lat1, long1, lat2, long2):

    lat1, long1, lat2, long2 = float(lat1), float(long1), float(lat2), float(long2)

    degrees_to_radians = math.pi/180.0

    phi1 = (90.0 - lat1)*degrees_to_radians
    phi2 = (90.0 - lat2)*degrees_to_radians


    theta1 = long1*degrees_to_radians
    theta2 = long2*degrees_to_radians

    cos = (math.sin(phi1)*math.sin(phi2)*math.cos(theta1 - theta2) +
           math.cos(phi1)*math.cos(phi2))
    arc = math.acos( cos )
    distance = arc*6378.1
      
    return distance

{% endcodeblock %}

It will return the distance in KM.

###Dynamic Relational Operations

Suppose we have a sentence like: `'5 is greater than 9'` and check if its true. We could use `eval` to dynamically converty string to python but its highly not recommended. So I tried like this:

{% codeblock %}
def calculate_relational_operation(lhs, rhs, operator):
    get_type = type(lhs).__name__
    if get_type == 'str':
        rhs = str(rhs)
    elif get_type == 'float':
        rhs = float(rhs)
    elif get_type == 'int':
        rhs = int(rhs)

    if operator == "==":
        if lhs == rhs:
            return True
        return False
    elif operator == "!=":
        if lhs != rhs:
                return True
        return False
    elif operator == ">":
        if lhs > rhs:
                return True
        return False
    elif operator == "<":
        if lhs < rhs:
                return True
        return False
    elif operator == ">=":
        if lhs >= rhs:
            return True
        return False
    elif operator == "<=":
        if lhs == rhs:
                return True
        return False
    elif operator == "Is":
        if lhs is rhs:
            return True
        return False
    return False

{% endcodeblock %}

It will return `True` or `False` depending on the statement/input.


###Get Week List

It will return all the weeks list from last 1 year (extendable).

{% codeblock %}

from isoweek import Week

def generate_week():
	max_week = datetime.datetime.combine(Week.thisweek().thursday(), datetime.time(0,0))
	min_week = max_week - datetime.timedelta(days=365)
	_weeks = list()
	while True:
	    _weeks.append('Week'+str(max_week.isocalendar()[1])+ ' ' +str(max_week.isocalendar()[0])))
	    max_week -= datetime.timedelta(days=7)
	    if max_week <= min_week:
		break

	return _weeks

#Output>> ['Week2 2015', 'Week1 2015', 'Week52 2014' ....]


{% endcodeblock %}


###Get Month List

It will return last 12 month's year and month number. Constructed using this SO answer: http://stackoverflow.com/a/6576603/2696165

{% codeblock %}
x = 12
now = time.localtime()
print([time.localtime(time.mktime((now.tm_year, now.tm_mon - n, 1, 0, 0, 0, 0, 0, 0)))[:2] for n in range(x)])

#Output>> [(2015, 2), (2015, 1), (2014, 12), (2014, 11), (2014, 10), (2014, 9), (2014, 8), (2014, 7), (2014, 6), (2014, 5), (2014, 4), (2014, 3)]

{% endcodeblock %}


###To Be Continued .....................


