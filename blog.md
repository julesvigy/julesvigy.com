---
layout: page
title: My Blog
permalink: /blog/
---
{% for post in site.posts %}
<span class="post-date">{{ post.date | date: "%b %-d, %Y" }}</span>
<a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
{{ post.excerpt }}
<br>
{% endfor %}