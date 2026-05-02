---
layout: default
title: Home
---
<section class="hero">
  <div class="hero-text">
    <h1>Ramesh Arvind Naagarajan</h1>
    <p class="lede">
      PhD researcher at the Chair of Automatic Control and System Dynamics,
      TU Chemnitz. I work on explainable AI for control systems, using
      large language models as reasoning interfaces over physical processes,
      with mechanistic interpretability and causal discovery as the
      load-bearing tools. My current testbed is greenhouse climate control,
      a setting where the same controller has to satisfy a grower, an
      agronomist, and an optimiser at the same time.
    </p>
    <p class="muted">
      I am part of the <a href="https://www.tu-chemnitz.de/etit/control/">Automatic Control and System Dynamics</a>
      group at TU Chemnitz, led by Prof. Dr.-Ing. Stefan Streif.
    </p>
  </div>
  <div class="hero-photo">
    <img src="{{ '/assets/img/headshot.jpg' | relative_url }}" alt="Portrait of Ramesh Arvind Naagarajan">
  </div>
</section>

<section class="cards">
  <a class="card" href="/research/">
    <h3>Research</h3>
    <p class="muted">Explainable model predictive control, causal reasoning, and language interfaces for engineers and operators.</p>
  </a>
  <a class="card" href="/publications/">
    <h3>Publications</h3>
    <p class="muted">Two journal papers in 2025, with new work on mechanistic interpretability under review.</p>
  </a>
  <a class="card" href="/blog/">
    <h3>Writing</h3>
    <p class="muted">Notes on LLMs, interpretability, and the messy middle between control theory and machine learning.</p>
  </a>
  <a class="card" href="/cv/">
    <h3>CV</h3>
    <p class="muted">Background, experience, and the path here. PDF and web versions.</p>
  </a>
</section>

<hr>

<section class="recent">
  <h2>Recent</h2>
  <ul class="post-list">
    {% for post in site.posts limit:3 %}
      <li>
        <p class="post-meta">{{ post.date | date: "%B %-d, %Y" }}</p>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </li>
    {% endfor %}
  </ul>
</section>
