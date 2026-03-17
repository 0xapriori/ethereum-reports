---
layout: default
title: Index
---

<div class="index-header">
  <h1>Ethereum Reports</h1>
  <p>Research on DEXes, DeFi, lending markets, Ethereum infrastructure, and TradFi</p>
</div>

{% assign categories = "defi,dexes,ethereum,lending,tradfi" | split: "," %}
{% assign all_reports = "" | split: "" %}

{% for cat in categories %}
  {% assign cat_reports = site.pages | where_exp: "page", "page.dir contains cat" | where_exp: "page", "page.title" %}
  {% for report in cat_reports %}
    {% assign all_reports = all_reports | push: report %}
  {% endfor %}
{% endfor %}

{% assign all_reports_sorted = all_reports | sort: "date" | reverse %}

<ul class="report-list" id="report-list">
  {% for report in all_reports_sorted %}
  {% assign cat_slug = report.dir | split: "/" | last %}
  <li data-category="{{ cat_slug }}">
    <a href="{{ report.url | relative_url }}">{{ report.title }}</a>
    <div class="report-info">
      {% if report.date %}<span class="date">{{ report.date | date: "%B %-d, %Y" }}</span>{% endif %}
      <span class="category-tag">{{ report.category }}</span>
    </div>
  </li>
  {% endfor %}
</ul>

