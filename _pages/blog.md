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
The writing here consists of my own notes, as well as my occasional attempts at answering some interesting questions.

I do this for for largely two reasons: firstly, I like making knowledge more accessible, even if the difference I can create is marginal. I believe that the way research and knowledge are presented can be made far better, and my thoughts on this are justified beautifully in <a href="https://distill.pub/2017/research-debt/">this Distill post by Chris Olah and Shan Carter</a>. Secondly, I think there is value in keeping an iterative record of my learning - articulating thoughts forces me to think and write more carefully. There is also an excellent <a href="https://medium.com/@racheltho/why-you-yes-you-should-blog-7d2544ac1045">blog post by Rachel Thomas</a> on why this might be a good use of your time in academia!
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
