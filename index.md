---
layout: default
---

# Pages
{% for post in site.pages %}{% if post.title %}
* [{{ post.title }}](.{{ post.url }}){% endif %}{% endfor %}

# Posts
{% for post in site.posts %}{% if post.title %}
* [{{ post.title }}](.{{ post.url }}){% endif %}{% endfor %}
