---
layout: default
title: Početna
---
{%- comment -%}
TEST PORTAL – mobilna naslovnica: udarna (najnoviji featured), Najnovije (5), kategorijski blokovi (1 velika + 3 male).
Svaki članak ima sliku; ako je nema, koristi se zeleni placeholder.
{%- endcomment -%}
{%- assign tp_ph = '/assets/images/placeholder-suma.svg' -%}
{%- assign vijesti = site.posts | where_exp: "p", "p.type != 'Zaposlenje'" -%}
{%- assign featured = vijesti | where: "featured", true | first -%}
{%- if featured -%}{%- assign lead = featured -%}{%- else -%}{%- assign lead = vijesti.first -%}{%- endif -%}

<section class="tp-hero">
  <a href="{{ lead.url | relative_url }}" class="tp-hero-link">
    <img src="{{ lead.image | default: tp_ph | relative_url }}" alt="{{ lead.image_alt | default: lead.title | escape }}" fetchpriority="high">
    <div class="tp-hero-text">
      <span class="tp-kicker">{% if featured %}Udarna vijest{% else %}Najnovije{% endif %}{% if lead.location %} · {{ lead.location }}{% endif %}</span>
      <h1>{{ lead.title }}</h1>
      <p>{{ lead.excerpt | strip_html | strip | truncate: 180 }}</p>
    </div>
  </a>
  {% if lead.image_credit %}<p class="tp-credit">{{ lead.image_credit | markdownify | remove: '<p>' | remove: '</p>' | strip }}</p>{% endif %}
</section>

<section class="tp-block tp-najnovije" id="najnovije">
  <div class="tp-block-head"><h2>Najnovije</h2><a href="{{ '/kategorije/' | relative_url }}">Sve →</a></div>
  <ul class="tp-list">
  {%- assign tp_n = 0 -%}
  {%- for post in vijesti -%}
    {%- if post.url == lead.url -%}{%- continue -%}{%- endif -%}
    {%- if tp_n >= 5 -%}{%- break -%}{%- endif -%}
    {%- assign tp_n = tp_n | plus: 1 %}
    <li><a href="{{ post.url | relative_url }}" class="tp-row">
      <img src="{{ post.image | default: tp_ph | relative_url }}" alt="" loading="lazy">
      <span class="tp-row-text"><strong>{{ post.title }}</strong><small>{{ post.location }} · {% include datum.html date=post.date show_time=post.show_time short=true %}</small></span>
    </a></li>
  {%- endfor %}
  </ul>
</section>

{% include oglas-banner.html %}

{%- assign tp_blokovi = "vijesti|Vijesti|Aktualno,Promet,Priroda,Kultura,Društvo,Pregled tjedna|/vijesti/;poslovi-opcine|Poslovi općine|Općinske vijesti|/kategorije/opcinske-vijesti/;zanimljivosti|Zanimljivosti|Zanimljivosti|/kategorije/zanimljivosti/;dogadanja|Događanja|Događanja|/kategorije/dogadanja/;sport|Sport|Sport|/kategorije/sport/;crna-kronika|Crna kronika|Crna kronika|/kategorije/crna-kronika/;povijest-gk|Povijest GK|Povijest|/povijest/" | split: ";" -%}
{%- for b in tp_blokovi -%}
  {%- assign bp = b | split: "|" -%}
  {%- assign b_vrste = bp[2] | split: "," -%}
  {%- assign b_posts = "" | split: "" -%}
  {%- for post in vijesti -%}
    {%- if post.url == lead.url -%}{%- continue -%}{%- endif -%}
    {%- if b_vrste contains post.type -%}{%- assign b_posts = b_posts | push: post -%}{%- endif -%}
    {%- if b_posts.size >= 4 -%}{%- break -%}{%- endif -%}
  {%- endfor -%}
  {%- if b_posts.size == 0 -%}{%- continue -%}{%- endif %}
<section class="tp-block" id="blok-{{ bp[0] }}">
  <div class="tp-block-head"><h2>{{ bp[1] }}</h2><a href="{{ bp[3] | relative_url }}">Više →</a></div>
  {%- assign big = b_posts[0] %}
  <a href="{{ big.url | relative_url }}" class="tp-big">
    <img src="{{ big.image | default: tp_ph | relative_url }}" alt="{{ big.image_alt | default: big.title | escape }}" loading="lazy">
    <h3>{{ big.title }}</h3>
    <small>{{ big.location }} · {% include datum.html date=big.date show_time=big.show_time short=true %}</small>
  </a>
  {%- if b_posts.size > 1 %}
  <ul class="tp-list tp-list-small">
    {%- for post in b_posts offset: 1 %}
    <li><a href="{{ post.url | relative_url }}" class="tp-row">
      <img src="{{ post.image | default: tp_ph | relative_url }}" alt="" loading="lazy">
      <span class="tp-row-text"><strong>{{ post.title }}</strong><small>{{ post.location }}</small></span>
    </a></li>
    {%- endfor %}
  </ul>
  {%- endif %}
</section>
{%- endfor %}

{% include zaposlenje-blok.html %}
