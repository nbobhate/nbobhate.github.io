---
layout: page
title: Categories
---

{% capture site_tags %}
	{% for tag in site.tags %}
		{{ tag | first }}
		{% unless forloop.last %},{% endunless %}
	{% endfor %}
{% endcapture %}

{% assign tags_list = site_tags | split:',' | sort %}

<ul class="tag-box inline">
	{% for item in (0..site.tags.size) %}
		{% unless forloop.last %}
    		{% capture this_word %}
    			{{ tags_list[item] | strip_newlines }}
    		{% endcapture %}
    
    		<li><a href="#{{ this_word }}">{{ this_word }} <span>{{ site.tags[this_word].size }}</span></a></li>
  		{% endunless %}
  	{% endfor %}
</ul>
