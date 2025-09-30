---
layout: default
title: Home
---

<!-- Fixed navigation -->
<nav id="navbar">
  <ul>
    <li><a href="#home">Home</a></li>
    <li><a href="#projects">Projects</a></li>
    <li><a href="#blog">Blog</a></li>
    <li><a href="#cv">CV</a></li>
  </ul>
</nav>

<!-- Home Section -->
<section id="home">
  <h1>Hi, I'm Hariganesh Tangirala</h1>
  <p>Welcome to my personal website.</p>
  <p>Here you can find my projects, blog, and CV.</p>
</section>

<!-- Projects Section -->
<section id="projects">
  <h2>Projects</h2>
  <ul>
    <li>Pulse Oximeter Project</li>
    <li>Bayesian Cognition Notes</li>
    <li>Debate Casefiles Repo</li>
  </ul>
</section>

<!-- Blog Section -->
<section id="blog">
  <h2>Blog</h2>
  <ul>
    {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a> - {{ post.date | date: "%B %-d, %Y" }}
    </li>
    {% endfor %}
  </ul>
</section>

<!-- CV Section -->
<section id="cv">
  <h2>CV</h2>
  <p><a href="https://drive.google.com/file/d/1d6lKtPy-cd1OhS_BeaMKqAjuw0RzObWw/view?usp=sharing" target="_blank">View my CV</a></p>
</section>
