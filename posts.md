---
layout: default
---

# Posts
{% for post in site.posts %}{% if post.title %}
* [{{ post.title }}](.{{ post.url }}){% endif %}{% endfor %}
