---
layout: page
title: "Blog"
permalink: /blog/
---

<ul>
  {% raw %}{% for post in site.posts %}{% endraw %}
    <li>
      <span>{{ post.date | date: "%Y-%m-%d" }}</span>
      &nbsp;—&nbsp;
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
  {% raw %}{% endfor %}{% endraw %}
</ul>
