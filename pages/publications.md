---
layout: page
title: Publications
subtitle: Peer-reviewed work and selected preprints.
permalink: /publications/
---
{% for pub in site.data.publications %}
  {% include pub_card.html pub=pub %}
{% endfor %}

## Under review

A submission to **ICML 2026** is currently under review. Details will be added here once the review process completes.

<p class="muted small">For a complete list including talks, see my
<a href="https://scholar.google.com/citations?user={{ site.social.scholar }}">Google Scholar</a>
profile.</p>
