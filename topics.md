---
layout: master
title: All Topics
---

Here is everything I've written about, organised by category:

{% for category in site.categories %}
  ### {{ category[0] | capitalize }}
  <ul>
    {% for post in category[1] %}
      <li>
        <a href="{{ post.url }}">{{ post.title }}</a> 
        <small>({{ post.date | date: "%B %d, %Y" }})</small>
      </li>
    {% endfor %}
  </ul>
{% endfor %}
