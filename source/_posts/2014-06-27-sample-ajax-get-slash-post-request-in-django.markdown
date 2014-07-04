---
layout: post
title: "Sample Ajax GET/POST Request in Django"
date: 2014-06-27 00:17:31 +0600
comments: true
categories: [Django, Ajax]
---

Let us make a test scenario here: a dropdown field which on change we are going to send a Get/Post request to django and return response.<!--more-->
{% codeblock Html_Code %}

<select id="select_dropdown">
<option value='joshua'>joshua</option>
<option value='peter'>peter</option>
....
....
</select>

{% endcodeblock %}

Lets make a `Ajax` request after change in dropdown field.

{% codeblock Javascript %}

$(document).ready(function(){

 $('#select_dropdown').change(function(){
    var e = document.getElementById("select_dropdown");
    var value = e.options[e.selectedIndex].value;

    $.ajax({
        url: "your-url",
        type: "post", // or "get"
        data: value,
        success: function(data) {

          alert(data.result);
        }});

});

{% endcodeblock %}

here on change of an post request is called. Now lets handle the view.

{% codeblock views.py%}
import json
def post(request):
	if request.POST(): #os request.GET()
    	get_value= request.body
    	# Do your logic here coz you got data in `get_value`
    	data = {}
    	data['result'] = 'you made a request'
		return HttpResponse(json.dumps(data), content_type="application/json")

{% endcodeblock %}

So you will get a pop-up message like:

{% img https://dl.dropboxusercontent.com/u/235131545/myblog/Capture.JPG %}

Thats all.