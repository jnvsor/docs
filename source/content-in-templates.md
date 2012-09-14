Content in templates
====================

Perhaps the thing you'll do most in templates, is access records of content. Either by requesting specific content, or implicitly when requesting pages that are the defaults for certain contenttypes. 


Default routes for content
--------------------------

For example, if you have a 'Pages' contenttype, with 'Page' as a singular_name, your site will automatically have pages like:

  - `http://example.org/pages` - Uses `pages.twig`, and displays several records of contenttype 'Pages'.
  - `http://example.org/page/lorem-ipsum-dolor` - Uses `page.twig`, and displays the record of contenttype 'Pages' with the slug 'lorem-ipsum-dolor'.

Unless specified, Pilex will determine the names of these templates automatically. Both of these default templates can be overridden by defining `template` and `singletemplate` in `contenttypes.yml`. 
If the contenttype has a 'template select' field type, the template can be set on a per-record basis. 

In the default template for a single record, it is available as both `{{ record }}` and also by the name of the singular name. So, in the above example, you can also use `{{ page }}`, without having to set it specifically.
Likewise, in the default template for multiple records, the content is available as `{{ records }}` and also by the name of the contenttype, for example `{{ pages }}`.

Using a {{ record }}
----------------------

The easiest way to see what the contents of a record (or any other variable, for that matter) are, is to use the print() function: 

<pre class="brush: html">
{{ print(record) }}
</pre>

<a href="/files/content-example3.png" class="fancybox"><img src="/files/content-example3.png" width="500"></a>

As you can see in the screenshot, a record of a contenttype is an `object`. There are several benefits to this over a regular `array`. We can access the fields like regular values, but we can also use specific functionality for every object, without the need to define these seperately. 

You can access regular fields in a record like these examples for either a `page` or `entry` record: 
<pre class="brush: html">
{{ page.title }}
{{ page.text }}

Created on {{ entry.datecreated|date('Y-m-d')}}

The contenttype for this entry is {{ entry.contenttype.name }}, 
and it contains {{ entry.contenttype.fields|length }} fields.
</pre>

The real power comes from using the special functions that are defined for every content record. 

To get a link to the content: 
<pre class="brush: html">
Link: &lt;a href="{{ page.link }}">{{ page.title }}&lt;/a> 
</pre>

Get a short excerpt of the record: 

<pre class="brush: html">
&lt;p>{{ page.excerpt(250) }}&lt;/p> 
</pre>



<p class="note"><strong>Note:</strong> Before Pilex 1.0 is released, it'll be possible to create one-to-one and one-to-many relatrionships between records. The record object will provide access to records that are related to it.</p>


Using {{ records }}
-------------------

The `{{ records }}` array, is basically a set of several content records. When you have a `{{ records }}`array, you can iterate over each of the records to output them as desired. 
In the following example you can see how to get an array of records. You'll notice that in this case it's not actually called `records`, but `pages`. Since it's just a variable name, we can call it whatever we like. After getting the `{{ pages }}` array, we use a simple `for` loop, so we can iterate over each of the seperate `{{ page}}` records. 

<pre class="brush: html">
{% setcontent pages = 'pages/latest/4' %}

{% for page in pages %}

	// Do something with each {{ page }}
	{{ page.title }}

{% endfor %}
</pre>

Because `{{ records }}` is an array, we can use all the regular Twig functionality for arrays. In the previous example we've shown how to iterate over the records using a `for` loop, but you can also do things like the following.

Check how many records there are:

<pre class="brush: html">
{% if pages|length > 0 %} More than 0 records {% endif %} 

{% if pages|length < 5 %} Less than 5 records {% endif %}  

There are exactly {{ pages|length }} records. 
</pre>

[Reverse](http://twig.sensiolabs.org/doc/filters/reverse.html) the array:

<pre class="brush: html">
{% for page in pages|reverse %}

	// Do something with each {{ page }}
	{{ page.title }}

{% endfor %}
</pre>

Or [slice](http://twig.sensiolabs.org/doc/filters/slice.html) the array: 

<pre class="brush: html">
{% set slice = pages|slice(1,3) %}

{% for page in slice %}

	// Do something with {{ page }} 1 through 3 
	{{ page.title }}

{% endfor %}
</pre>

Fetching content
----------------

To get content from the database, you can use the `setcontent` tag. The following example will get the content record with slug 'about' from the 'pages' contenttype:

<pre class="brush: html">
{% setcontent about = 'page/about' %}

{{ print(about) }}
</pre>



