---
layout: page
title: Hengfeng Li
---
{% include JB/setup %}
<!--
Read [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)

Complete usage and documentation available at: [Jekyll Bootstrap](http://jekyllbootstrap.com)

## Update Author Attributes

In `_config.yml` remember to specify your own data:
    
    title : My Blog =)
    
    author :
      name : Name Lastname
      email : blah@email.test
      github : username
      twitter : username

The theme should reference these variables whenever needed.
    
## Sample Posts

This blog contains sample posts which help stage pages and blog data.
When you don't need the samples anymore just delete the `_posts/core-samples` folder.

    $ rm -rf _posts/core-samples

Here's a sample "posts list".
-->

<div class="posts">
  {% for post in site.posts %}
    <div class="post-item">
      <div class="post-meta">
        <p class="date">
          <em>{{ post.date | date: "%d" }}</em>
          <strong>{{ post.date | date: "%b" }}</strong>
          <strong>{{ post.date | date: '%Y' }}</strong>
        </p>
      </div>
      <div class="post-title">
        <h1><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h1>

        <div class="post-title-sub">
          by Hengfeng Li - tags: 
          {% for tag in post.tags %} 
            <a href="{{ BASE_PATH }}{{ site.JB.tags_path }}#{{ tag }}-ref">{{ tag }}</a>
          {% endfor %}
        </div>
      </div>
      <div class="post-content">{{ post.content }}</div>
    </div>
  {% endfor %}
</div>
<!-- <ul class="posts">
  {% for post in site.posts %}
    <li class="post-item">
      <span>{{ post.date | date_to_string }}</span> &raquo; 
      <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>

      {{ post.content }}
    </li>
  {% endfor %}
</ul>
-->

