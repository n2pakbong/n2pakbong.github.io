---
layout: section
title: "Blog"
permalink: /blog/
---

<div class="blog-hero">
  <h1>Musings of a Weak Learner</h1>
  <h2>Mathematics, Machine Learning, and other random thoughts.</h2>
</div>
<ul>
  {% for post in site.posts %}
    <li>
      <span>{{ post.date | date: "%Y-%m-%d" }}</span>
      &nbsp;—&nbsp;
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
