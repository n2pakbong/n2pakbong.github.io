---
layout: section
title: "Blog"
permalink: /blog/
---
# Musings of a weak learner
_Mathematics, Machine Learning, and other random thoughts._
<ul>
  {% for post in site.posts %}
    <li>
      <span>{{ post.date | date: "%Y-%m-%d" }}</span>
      &nbsp;—&nbsp;
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
