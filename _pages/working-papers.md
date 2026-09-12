---
layout: page
title: "work in progress"
permalink: /work-in-progress/
nav: true
nav_order: 4
---

<style>
  .wip-intro {
    margin-bottom: 1.25rem;
  }

  .wip-list {
    display: grid;
    gap: 1rem;
  }

  .wip-item {
    border: 1px solid #dcdcdc;
    border-radius: 8px;
    padding: 1rem 1.1rem;
    background: #fafafa;
  }

  .wip-title {
    font-size: 1.05rem;
    font-weight: 600;
    margin-bottom: 0.35rem;
  }

  .wip-meta {
    color: #5f6368;
    font-size: 0.95rem;
    margin-bottom: 0.45rem;
  }

  .wip-links a {
    margin-right: 0.9rem;
    font-size: 0.95rem;
  }
</style>

<div class="wip-intro">
  This page collects ongoing research projects. I will update entries as new drafts, data, and replication files become available.
</div>

<div class="wip-list">
  {% for paper in site.data.working_papers %}
    <article class="wip-item">
      <div class="wip-title">{{ paper.title }}</div>
      {% if paper.subtitle and paper.subtitle != "" %}
        <div class="wip-meta">{{ paper.subtitle }}</div>
      {% endif %}
      <div class="wip-meta">
        {% if paper.coauthors and paper.coauthors.size > 0 %}
          With {{ paper.coauthors | join: ", " }} ·
        {% endif %}
        {{ paper.status }}
      </div>
      {% if paper.abstract and paper.abstract != "" %}
        <div>{{ paper.abstract }}</div>
      {% endif %}
      <div class="wip-links">
        {% if paper.page %}
          <a href="{{ paper.page | relative_url }}">Details &amp; citation</a>
        {% endif %}
        {% assign paper_href = paper.paper_url | default: "" | strip %}
        {% assign doi_href = paper.doi_url | default: "" | strip %}
        {% if paper_href != "" %}
          <a href="{{ paper_href }}" target="_blank" rel="noopener">Paper</a>
        {% endif %}
        {% if doi_href != "" %}
          <a href="{{ doi_href }}" target="_blank" rel="noopener">DOI</a>
        {% endif %}
      </div>
    </article>
  {% endfor %}
</div>
