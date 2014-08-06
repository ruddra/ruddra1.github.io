---
layout: post
title: "Working with Formsets"
date: 2014-08-06 21:44:37 +0600
comments: true
categories: [django, formsets, form, javascript]
---

As <a href="https://docs.djangoproject.com/en/dev/topics/forms/formsets/">documentation</a> says: <blockquote> A formset is a layer of abstraction to work with multiple forms on the same page. It can be best compared to a data grid</blockquote>.

So here I am going to show a very simple django formset implementation example.<!--more-->

Here we are going to use the following model, form, template, view:

<h3>Model:</h3>

{% codeblock model %}
class Product(models.Model):
    name = models.CharField(max_length=50)
    quantity = models.IntegerField()
    price = models.IntegerField()

class Distributor(models.Model):
    name = models.CharField(max_length=100)
    products= models.ManyToManyField(Product)

{% endcodeblock %}

These fairly simple models, where product is related to distributor model by a many-to-many relation.

<h3>Form:</h3>

First we declare productform, then using formset factory helps to create multiple instances of product. Then we add this to distributor form like below:

{% codeblock form %}
from django import forms
from django.forms.formsets import formset_factory
class ProductForm(forms.Form):
     name = forms.CharField()
     quantity = forms.IntegerField()
     price = forms.IntegerField()

ProductFormset= formset_factory(ProductForm)

class DistributorForm(forms.Form):
    name= forms.CharField()
    products= ProductFormset()
{% endcodeblock %}

Now we use this form in template.

<h3>Template:</h3>

{% codeblock template %}
{% raw %}

<form action="" method="post" class="">
{% csrf_token %}
<h2> Distributors :</h2>
{% for field in form %}
	{{ field.errors }}
	{{ field.label_tag }} : {{ field }}
{% endfor %}
{{ form.products.management_form }}

<h3> Product Instance(s)</h3>
<table id="table-product">
	<thead>
		<tr>
		  <th>name</th>
		  <th>quantity</th>		
		  <th>price</th>
		</tr>
    </thead>
    <tbody class="product-instances">
		<tr>
		  <td>{{ form.product }}</td>
		  <td>{{ form.product }}</td>		
		  <td>{{ form.product }}</td>
		  <td> <input id="input_add" type="button" name="add" value=" Add More " class="tr_clone_add btn data_input"> </td>
		</tr>
    </tbody>
{% endfor %}
</table>
            <input type="submit" name="submit" class="button" value="Save"/>
</form>

<script>
        var i = 1;
        $("#input_add").click(function() {
            $("tbody tr:first").clone().find(".data_input").each(function() {
                if ($(this).attr('class')== 'tr_clone_add btn data_input'){
                    $(this).attr({
                        'id': function(_, id) { return "remove_button" },
                        'name': function(_, name) { return "name_remove" +i },
                        'value': 'Remove'
                    }).on("click", function(){
                        var a = $(this).parent();
                        var b= a.parent();
                        i=i-1
                        $('#id_form-TOTAL_FORMS').val(i);
                        b.remove();

                        $('.product-instances tr').each(function(index, value){
                            $(this).find('.data_input').each(function(){
                                $(this).attr({
                                    'id': function (_, id) {
                                        var idData= id;
                                        var splitV= String(idData).split('-');
                                        var fData= splitV[0];
                                        var tData= splitV[2];
                                        return fData+ "-" +index + "-" + tData
                                    },
                                    'name': function (_, name) {
                                        var nameData= name;
                                        var splitV= String(nameData).split('-');
                                        var fData= splitV[0];
                                        var tData= splitV[2];
                                        return fData+ "-" +index + "-" + tData
                                    }
                                });
                            })
                        })
                    })
                }
                else{
                    $(this).attr({
                        'id': function (_, id) {
                            var idData= id;
                            var splitV= String(idData).split('-');
                            var fData= splitV[0];
                            var tData= splitV[2];
                            return fData+ "-" +i + "-" + tData
                        },
                        'name': function (_, name) {
                            var nameData= name;
                            var splitV= String(nameData).split('-');
                            var fData= splitV[0];
                            var tData= splitV[2];
                            return fData+ "-" +i + "-" + tData
                        }
                    });

                }
            }).end().appendTo("tbody");
            $('#id_form-TOTAL_FORMS').val(1+i);
            i++;

        });
    </script>
{% endraw %}
{% endcodeblock %}

The html part is fairly simple, like using form in template. Then the JS is being used so that multiple instances of product form can be generated like: 

{% codeblock %}

<!-- First row of the table -->

<tr><td><input type="text" name="form-0-name" id="id_form-0-name" /></td>
<td><input type="number" name="form-0-quantity" id="id_form-0-quantity" /></td>
<td><input type="number" name="form-0-price" id="id_form-0-price" /></td>
<td> <input id="input_add" type="button" name="add" value=" Add More " class="tr_clone_add btn data_input"> </td> </tr>

<!-- Second row of the table -->

<tr><td><input type="text" name="form-1-name" id="id_form-1-name" /></td>
<td><input type="number" name="form-1-quantity" id="id_form-1-quantity" /></td>
<td><input type="number" name="form-1-price" id="id_form-1-price" /></td>
<td> <input id="remove_button" type="button" name="remove_button1" value=" Remove " class="tr_clone_add btn data_input"> </td> </tr>

<!-- more inline formset are going to rendered here -->

{% endcodeblock %}

<h3>View:</h3>

Here values from the form are being saved to database.

{% codeblock view %}

def post(request):
        form = DistributorForm(request.POST)
        form.product_instances = ProductFormset(request.POST)
        if form.is_valid():
            distributor= Distributor() #model class
            distributor.name= form.cleaned_data('name')
            distributor.save()
            if form.product_instances.cleaned_data is not None:
                for items in form.product_instances.cleaned_data:
                    product = Product() #Product model class
                    product.name= item['name']
                    product.quantity= item['quantity']
                    product.price= item['price']
                    product.save()
                    distributor.products.add(product)
            return redirect('/success')
        return redirect('/failure')

{% endcodeblock %}

Output should look like this:

{% img https://dl.dropboxusercontent.com/u/235131545/myblog/formset.JPG %}

<b>Notes to keep in mind:</b>

First, need to be careful about things like:

{% codeblock %}
<input type="hidden" name="form-TOTAL_FORMS" value="1" id="id_form-TOTAL_FORMS" />
<input type="hidden" name="form-INITIAL_FORMS" value="0" id="id_form-INITIAL_FORMS" />
<input type="hidden" name="form-MAX_NUM_FORMS" id="id_form-MAX_NUM_FORMS" />
{% endcodeblock %}

Here `form-TOTAL_FORMS` 's value should be equal to number of rows in table. The code above must exist in order to formset to work.

Second, in views.py, formset form class needs to be called, else cleaned data within the formset can't be found.

{% codeblock %}
form.product_instances = ProductFormset(request.POST)
{% endcodeblock %}