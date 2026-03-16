---
layout: default
title: Index
---

<div class="index-header">
  <h1>Ethereum Reports</h1>
  <p>Research on DEXes, DeFi, lending markets, Ethereum infrastructure, and TradFi</p>
</div>

<div class="tabs" id="tabs">
  <button class="tab active" data-filter="all">All</button>
  <button class="tab" data-filter="defi">DeFi</button>
  <button class="tab" data-filter="dexes">DEXes</button>
  <button class="tab" data-filter="ethereum">Ethereum</button>
  <button class="tab" data-filter="lending">Lending</button>
  <button class="tab" data-filter="tradfi">TradFi</button>
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

<script>
  (function() {
    var tabs = document.querySelectorAll('.tab');
    var items = document.querySelectorAll('#report-list li');
    var navLinks = document.querySelectorAll('.nav a[data-tab]');

    function setFilter(filter) {
      tabs.forEach(function(t) { t.classList.toggle('active', t.dataset.filter === filter); });
      items.forEach(function(li) {
        li.style.display = (filter === 'all' || li.dataset.category === filter) ? '' : 'none';
      });
      navLinks.forEach(function(a) { a.classList.toggle('active', a.dataset.tab === filter); });
      if (filter !== 'all') {
        history.replaceState(null, '', '#' + filter);
      } else {
        history.replaceState(null, '', window.location.pathname);
      }
    }

    tabs.forEach(function(tab) {
      tab.addEventListener('click', function() { setFilter(tab.dataset.filter); });
    });

    navLinks.forEach(function(link) {
      link.addEventListener('click', function(e) {
        if (window.location.pathname === '/' || window.location.pathname === '/index.html') {
          e.preventDefault();
          setFilter(link.dataset.tab);
        } else {
          link.href = '/' + (link.dataset.tab === 'all' ? '' : '#' + link.dataset.tab);
        }
      });
    });

    var hash = window.location.hash.replace('#', '');
    if (hash && ['defi','dexes','ethereum','lending','tradfi'].indexOf(hash) !== -1) {
      setFilter(hash);
    }
  })();
</script>
