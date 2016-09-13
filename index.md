---
layout: default
title: Hengfeng's Blog
---

<!-- > People are rewarded in public for what they’ve practiced for years in private. 
> - Anthony Robbins -->

<div id="home">
  <h1 class="text-align-center">Essays</h1>
  <ul class="posts">
    {% for post in site.posts %}
      <li><span class="post-date-span">{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
</div>


