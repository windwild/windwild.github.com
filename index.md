---
layout: page
title: windwild
tagline: windwild@github
---
{% include JB/setup %}
    
## Recent Posts

<ul class="posts">
  {% for post in site.posts %}
    <li>
      <span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

## About me

A Geek at Chinese University of Hong Kong


