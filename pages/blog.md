---
layout: page
title: Blog
subtitle: Notes from the bench. Mostly LLMs, interpretability, and control.
permalink: /blog/
---
<ul class="post-list">
  {% for post in site.posts %}
    <li>
      <p class="post-meta">{{ post.date | date: "%B %-d, %Y" }}</p>
      <a href="{{ post.url | relative_url }}"><strong>{{ post.title }}</strong></a>
      {% if post.subtitle %}<p class="muted">{{ post.subtitle }}</p>{% endif %}
    </li>
  {% endfor %}
</ul>
