---
layout: default
title: About
---

<section class="about">
  <h2>About me</h2>
  <p>
    I'm a Security Researcher skilled in Vulnerability Research, Reverse Engineering, Mobile
    Security, IoT Security, and Automated Security Testing. My research interests are in 
    Systems Security and Automated Program Analysis. These days my focus is shifting toward 
    automated security analysis of AI agents: adapting program analysis, testing, and
    adversarial-intelligence techniques to discover unsafe agent behavior.
  </p>
</section>

<section class="advisories">
  <h2>Security advisory credits</h2>
  {% if site.data.advisories.size > 0 %}
  <ul class="advisory-list">
    {% for adv in site.data.advisories %}
    <li>
      <a class="advisory-title" href="{{ adv.url }}" target="_blank" rel="noopener">{{ adv.title }}</a>
      <span class="advisory-meta">
        {% if adv.ghsa_id %}{{ adv.ghsa_id }}{% endif %}{% if adv.cve_id %} · {{ adv.cve_id }}{% endif %}{% if adv.credit %} · {{ adv.credit }}{% endif %}
      </span>
      {% if adv.summary %}<p class="advisory-summary">{{ adv.summary }}</p>{% endif %}
    </li>
    {% endfor %}
  </ul>
  {% else %}
  <p>No advisory credits listed yet.</p>
  {% endif %}
  <p><a href="https://github.com/advisories?query=credit%3A{{ site.github_username }}" target="_blank" rel="noopener">View all advisories credited to {{ site.github_username }} on GitHub &rarr;</a></p>
</section>

<section class="stats">
  <h2>Stats</h2>
  <img class="contrib-graph" src="https://github-readme-streak-stats.herokuapp.com/?user={{ site.github_username }}" alt="GitHub streak stats for {{ site.github_username }}" loading="lazy">
</section>

<section class="cta">
  <h2>Recent posts</h2>
  <ul class="post-list-compact">
    {% for post in site.posts limit:3 %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <span class="post-date">{{ post.date | date: "%b %-d, %Y" }}</span>
    </li>
    {% endfor %}
  </ul>
  <p><a href="{{ '/blog/' | relative_url }}">See all posts &rarr;</a></p>
</section>
