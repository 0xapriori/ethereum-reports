---
layout: default
title: Index
---

<div class="index-header">
  <h1>Ethereum Reports</h1>
  <p>Research on DEXes, DeFi, lending markets, Ethereum infrastructure, privacy, TradFi, and the media ecosystem</p>
</div>

{% assign all_reports = site.pages | where_exp: "page", "page.layout == 'report'" | sort: "date" | reverse %}

<ul class="report-list" id="report-list">
  {% for report in all_reports %}
  {% assign path_parts = report.path | split: "/" %}
  {% assign cat_slug = path_parts[0] %}
  <li data-category="{{ cat_slug }}">
    <a href="{{ report.url | relative_url }}">{{ report.title }}</a>
    <div class="report-info">
      {% if report.date %}<span class="date">{{ report.date | date: "%B %-d, %Y" }}</span>{% endif %}
      <span class="category-tag">{{ report.category }}</span>
    </div>
  </li>
  {% endfor %}
</ul>

