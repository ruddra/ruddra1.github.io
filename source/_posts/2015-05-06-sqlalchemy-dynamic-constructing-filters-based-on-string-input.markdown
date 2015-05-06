---
layout: post
title: "SQLAlchemy: Dynamically constructing filters based on string input"
date: 2015-05-06 15:13:51 +0600
comments: true
categories: python3 sqlalchemy
---

Here I am going to write a dynamic filter. This filter is made for python 3. It will take query or model class and filter condtion as input, It will return filtered query based on those filter condition.<!--more-->

This is constructed using this SO answer: http://stackoverflow.com/questions/14845196/dynamically-constructing-filters-in-sqlalchemy.

###Function:

{% codeblock %}

class DynamicFilter(ModelHelper):


    def __init__(self, query=None, model_class=None, filter_condition=None):
        super().__init__(*args, **kwargs)
        self.query = query
        self.model_class = model_class
        self.filter_condition = filter_condition


    def get_query(self):
        '''
        Returns query with all the objects
        :return:
        '''
        if not self.query:
            self.query = self.session.query(self.model_class)
        return self.query


    def filter_query(self, query, filter_condition):
        '''
        Return filtered queryset based on condition.
        :param query: takes query
        :param filter_condition: Its a list, ie: [(key,operator,value)]
        operator list:
            eq for ==
            lt for <
            ge for >=
            in for in_
            like for like
            value could be list or a string
        :return: queryset

        '''

        if query is None:
            query = self.get_query()
        model_class = self.get_model_class()  # returns the query's Model
        for raw in filter_condition:
            try:
                key, op, value = raw
            except ValueError:
                raise Exception('Invalid filter: %s' % raw)
            column = getattr(model_class, key, None)
            if not column:
                raise Exception('Invalid filter column: %s' % key)
            if op == 'in':
                if isinstance(value, list):
                    filt = column.in_(value)
                else:
                    filt = column.in_(value.split(','))
            else:
                try:
                    attr = list(filter(
                        lambda e: hasattr(column, e % op),
                        ['%s', '%s_', '__%s__']
                    ))[0] % op
                except IndexError:
                    raise Exception('Invalid filter operator: %s' % op)
                if value == 'null':
                    value = None
                filt = getattr(column, attr)(value)
            query = query.filter(filt)
        return query


    def return_query(self):
        return self.filter_query(self.get_query(), self.filter_condition)
{% endcodeblock %}



###Usage:

{% codeblock %}
        
        _filter_condition = [('has_attribute', 'eq', 'attribute_value')]

        dynamic_filtered_query_class = DynamicFilter(query=None, model_class=models.user.User,
                                          filter_condition=_filter_condition,
                                          )
        dynamic_filtered_query = dynamic_filtered_query_class.return_query()
{% endcodeblock %}

###How it works:

This class returns filtered queryset based on condition.

<b>model_class</b> is the model class you want to run the filter upon.
<b>filter_condition</b> the conditon you want to implement here. This is based on the following operator list:
        eq for ==
        lt for <
        ge for >=
        in for in_
        like for like
        value could be list or a string

    