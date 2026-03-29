---
layout: default
title: Reader Digests
---

# Reader Digests

<p class="subtitle">Daily reading digests generated from my Readwise Reader inbox.</p>

<ul class="digest-list">
{% assign sorted = site.digests | sort: 'date' | reverse %}
{% for digest in sorted %}
<li>
  <a href="{{ digest.url | relative_url }}">{{ digest.title }}</a>
  <span class="date">{{ digest.date | date: "%B %d, %Y" }}</span>
</li>
{% endfor %}
</ul>
