---
layout: page
title: Collections of Technical Hacks
permalink: /blog/
---

<div class="posts">
  {% for post in site.posts %}
    <article class="post">

      <h1><u><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></u></h1>

      <div class="entry">
        {{ post.excerpt }}
      </div>

      <a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More</a>
    </article>
  {% endfor %}
</div>
