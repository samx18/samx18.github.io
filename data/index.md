---
layout: data
---

<div id="home">

  <ul class="posts">
    {% for post in site.posts %}
    {% if post.categories contains 'data' %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
    {% endfor %}
  </ul>
</div>
