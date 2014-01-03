---
layout: page
---
{% include JB/setup %}

# Latest Post
-------------

{% assign x = site.posts.first %}
<h2><a href="{{ x.url }}">{{ x.title }}</a></h2>
{{ x.content }}


-------------
# Older Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>



