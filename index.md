---
layout: default
title: Index
---

<div class="index-header">
  <h1>Ethereum Reports</h1>
  <p>Research on DEXes, lending markets, and Ethereum infrastructure</p>
</div>

{% assign categories = "dexes,ethereum,lending" | split: "," %}
{% assign category_labels = "DEXes,Ethereum,Lending" | split: "," %}

{% for cat in categories %}
{% assign label = category_labels[forloop.index0] %}
{% assign reports = site.pages | where_exp: "page", "page.dir contains cat" | where_exp: "page", "page.title" | sort: "date" | reverse %}
{% if reports.size > 0 %}
<div class="section" id="{{ cat }}">
  <h2>{{ label }}</h2>
  <ul class="report-list">
    {% for report in reports %}
    <li>
      <a href="{{ report.url | relative_url }}">{{ report.title }}</a>
      {% if report.date %}<div class="date">{{ report.date | date: "%B %-d, %Y" }}</div>{% endif %}
    </li>
    {% endfor %}
  </ul>
</div>
{% endif %}
{% endfor %}
