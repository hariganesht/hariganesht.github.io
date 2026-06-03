---
layout: default
permalink: /blog/
title: writing
nav: true
nav_order: 1
pagination:
  enabled: true
  collection: posts
  permalink: /page/:num/
  per_page: 5
  sort_field: date
  sort_reverse: true
  trail:
    before: 1 # The number of links before the current page
    after: 3 # The number of links after the current page
---

<div class="post">

{% assign blog_name_size = site.blog_name | size %}
{% assign blog_description_size = site.blog_description | size %}

{% if blog_name_size > 0 or blog_description_size > 0 %}

  <div class="header-bar">
    <h1 style="margin-bottom: 1rem;">Writing</h1>
    <p style="text-align: left;">
The writing here consists of my own notes, as well as ideas I mess around with. 

I do this for for largely three reasons: first, I like making knowledge more accessible, even if the difference I can create is marginal; second - perhaps a tad more selfishly - writing helps me learn better; and finally, I think there is some value in keeping an iterative record of my learning. There is also an excellent <a href="https://medium.com/@racheltho/why-you-yes-you-should-blog-7d2544ac1045">blog post by Rachel Thomas</a> on why this might be a good use of your time in academia.
</p>
</div>
  {% endif %}

{% if page.pagination.enabled %}
{% include pagination.liquid %}
{% endif %}

</div>

<ul class="post-list">

  {% if page.pagination.enabled %}
    {% assign postlist = paginator.posts %}
  {% else %}
    {% assign postlist = site.posts %}
  {% endif %}

  {% for post in postlist %}
    <li>
      <h3>
        <a class="post-title" href="{{ post.url | relative_url }}">
          {{ post.title }}
        </a>
      </h3>

      {% if post.description %}
        <p>{{ post.description }}</p>
      {% endif %}

      <p class="post-meta">
        {{ post.date | date: '%B %d, %Y' }}
      </p>
    </li>
  {% endfor %}

</ul>
