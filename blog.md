---
layout: default
title: Blog
permalink: /blog/
---

<div>
  <h1 class="header-title">
    <p>Blog</p>
  </h1>
</div>

<div class="blog-intro">
  Writing about software, AI tools, and systems I’m building.
</div>

<div class="blog-list">
  {% assign posts_list = site.posts | sort: 'date' | reverse %}
  {% if posts_list.size > 0 %}
    {% for post in posts_list %}
    <div class="blog-post">
      <div class="blog-post-title"><a href="{{ post.url }}">{{ post.title }}</a></div>
      <div class="blog-post-meta">{{ post.date | date: "%b %-d, %Y" }}{% if post.tag %} <span class="blog-tag">{{ post.tag }}</span>{% endif %}</div>
      {% if post.description %}
      <div class="blog-post-desc">{{ post.description }}</div>
      {% endif %}
    </div>
    {% endfor %}
  {% else %}
    <div class="blog-post">
      <div class="blog-post-desc">No posts published yet.</div>
    </div>
  {% endif %}
</div>
