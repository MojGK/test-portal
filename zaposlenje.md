---
layout: default
title: Zaposlenje
permalink: /zaposlenje/
sidebar: true
description: Oglasi za posao u Gorskom kotaru – izdvojena radna mjesta i svi aktualni oglasi.
---
{%- comment -%}
Stranica Zaposlenje: svi članci s type: Zaposlenje (ili "Zaposlenje" u categories), najnoviji prvi.
Najnoviji tjedni pregled je istaknut na vrhu. Predložak za nove preglede: _drafts/predlozak-zaposlenje.md
{%- endcomment -%}
{%- assign zap_posts = "" | split: "" -%}
{%- for p in site.posts -%}{%- if p.type == "Zaposlenje" or p.categories contains "Zaposlenje" -%}{%- assign zap_posts = zap_posts | push: p -%}{%- endif -%}{%- endfor -%}
<div class="history-page zaposlenje-page">
  <h1><span aria-hidden="true">💼</span> Zaposlenje</h1>
  <p class="intro">Tjedni pregled aktualnih oglasa za posao u Gorskom kotaru: izdvojena radna mjesta i popis svih otvorenih oglasa s rokovima i poveznicama na izvor. Istekli oglasi se uklanjaju.</p>
  {% if zap_posts.size > 0 %}
  {%- assign zl = zap_posts.first %}
  <article class="zap-latest">
    <a href="{{ zl.url | relative_url }}" class="zap-latest-link">
      <span class="zap-latest-img"><img src="{{ zl.image | default: '/assets/images/zaposlenje-naslovna.jpg' | relative_url }}" alt="{{ zl.image_alt | default: zl.title | escape }}"></span>
      <span class="zap-latest-body">
        <span class="card-badges"><span class="card-badge udarno">Aktualno</span><span class="card-badge t-zaposlenje">Zaposlenje</span></span>
        <span class="zap-latest-title">{{ zl.title }}</span>
        <span class="zap-latest-date">{% include datum.html date=zl.date show_time=zl.show_time %}</span>
        {%- assign zl_ex = zl.excerpt | strip_html | strip -%}
        {% if zl_ex != "" %}<span class="zap-latest-ex">{{ zl_ex | truncate: 200 }}</span>{% endif %}
        <span class="lead-cta">{% if zl.broj_oglasa %}Svih {{ zl.broj_oglasa }} oglasa{% else %}Pogledaj oglase{% endif %} <span aria-hidden="true">→</span></span>
      </span>
    </a>
  </article>
  {% if zl.istaknuti_oglasi %}{% include istaknuti-oglasi.html post=zl naslov="Izdvojeni oglasi" %}{% endif %}
  {% if zap_posts.size > 1 %}
  <h2 class="section-title">Ranije objave</h2>
  <ul class="zap-older">
    {%- for post in zap_posts offset: 1 %}
    <li><a href="{{ post.url | relative_url }}">{{ post.title }}</a> <span class="meta">{% include datum.html date=post.date show_time=false %}</span></li>
    {%- endfor %}
  </ul>
  {% endif %}
  {% else %}
  <div class="zap-empty">
    <span class="zap-empty-icon" aria-hidden="true">💼</span>
    <p><strong>Prvi tjedni pregled uskoro.</strong></p>
    <p>Svaki tjedan ovdje ćemo skupiti aktualne oglase za posao iz Delnica, Vrbovskog, Čabra, Fužina, Lokava, Mrkoplja, Ravne Gore i Skrada – na jednom mjestu, s rokovima prijave i poveznicama na izvor.</p>
  </div>
  {% endif %}
</div>
