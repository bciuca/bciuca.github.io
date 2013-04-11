---
layout: page
title: devely
tagline: an unoriginal dev blog
---
{% include JB/setup %}

This is just a collection of notes that I find useful. I will give credit where it's due (if I don't, my bad, just drop me a line).
    
## Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>



