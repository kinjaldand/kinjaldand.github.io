---
layout: page
title: Professional Experience
permalink: /project/
---

<div class="posts">
  {% for proj in site.projects %}
    <article class="post">

      <h1><u><a href="{{ site.baseurl }}{{ proj.url }}">{{ proj.title }}</a></u></h1>

      <div class="entry">
        {{ proj.excerpt }}
      </div>

      <a href="{{ site.baseurl }}{{ proj.url }}" class="read-more">Read More</a>
    </article>
  {% endfor %}
</div>
