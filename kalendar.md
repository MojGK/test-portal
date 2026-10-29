---
layout: default
title: Kalendar događanja
permalink: /kalendar/
sidebar: true
---
{%- comment -%}
Kalendar: svi članci s event_date, po datumu, grupirani po mjesecima.
Prošli događaji (event_end, inače +3 h, a za 00:00 kraj dana) skrivaju se JS-om u pregledniku,
jer GitHub Pages gradi stranicu samo pri pushu. Gumb „Prošla događanja“ ih prikazuje.
{%- endcomment -%}
{%- assign k_mj = "siječanj,veljača,ožujak,travanj,svibanj,lipanj,srpanj,kolovoz,rujan,listopad,studeni,prosinac" | split: "," -%}
{%- assign k_dani = "ned,pon,uto,sri,čet,pet,sub" | split: "," -%}
{%- assign k_posts = site.posts | where_exp: "p", "p.event_date" | sort: "event_date" -%}
<div class="events-page kalendar-page">
  <h1>Kalendar događanja</h1>
  <p class="intro">Svi najavljeni događaji u Gorskom kotaru po datumu. Popis s karticama je na stranici
  <a href="{{ '/dogadaji/' | relative_url }}">Događaji</a>.</p>
  <p><button type="button" class="filter-btn" id="kal-prosli" aria-pressed="false">Prikaži prošla događanja</button></p>
  <div class="kalendar" id="kalendar">
  {%- assign k_cur = "" -%}
  {%- for post in k_posts -%}
    {%- assign k_key = post.event_date | date: "%Y-%m" -%}
    {%- assign k_start = post.event_date | date: "%s" | plus: 0 -%}
    {%- assign k_hm = post.event_date | date: "%H:%M" -%}
    {%- if post.event_end -%}{%- assign k_end = post.event_end | date: "%s" | plus: 0 -%}{%- elsif k_hm == "00:00" -%}{%- assign k_end = k_start | plus: 86400 -%}{%- else -%}{%- assign k_end = k_start | plus: 10800 -%}{%- endif -%}
    {%- if k_key != k_cur -%}
      {%- unless k_cur == "" %}</ul></section>{% endunless %}
      {%- assign k_mi = post.event_date | date: "%-m" | minus: 1 %}
    <section class="kal-mjesec" data-kal-mjesec>
      <h2>{{ k_mj[k_mi] | capitalize }} {{ post.event_date | date: "%Y." }}</h2>
      <ul class="kal-lista">
      {%- assign k_cur = k_key -%}
    {%- endif -%}
    {%- assign k_wi = post.event_date | date: "%w" | plus: 0 %}
        <li class="kal-stavka" data-kal-end="{{ k_end }}">
          <div class="kal-datum"><span class="kal-dan">{{ k_dani[k_wi] }}</span><span class="kal-broj">{{ post.event_date | date: "%-d." }}</span></div>
          <div class="kal-opis">
            <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
            <span class="meta">🕔 {% if k_hm == "00:00" %}vrijeme nije objavljeno{% else %}{{ k_hm }}{% endif %}{% if post.event_place %} · 📍 {{ post.event_place }}{% endif %}{% if post.location %} · {{ post.location }}{% endif %}</span>
          </div>
        </li>
  {%- endfor -%}
  {%- unless k_cur == "" %}</ul></section>{% endunless %}
  </div>
  <p class="no-results" id="kal-prazno" hidden>Trenutno nema najavljenih događaja.</p>
</div>
<script>
(function () {
  var now = Date.now() / 1000, btn = document.getElementById('kal-prosli'), show = false;
  function apply() {
    var any = 0;
    document.querySelectorAll('[data-kal-mjesec]').forEach(function (sec) {
      var vis = 0;
      sec.querySelectorAll('[data-kal-end]').forEach(function (li) {
        var past = Number(li.getAttribute('data-kal-end')) < now;
        li.classList.toggle('is-past', past);
        li.hidden = past && !show;
        if (!li.hidden) vis++;
      });
      sec.hidden = vis === 0; any += vis;
    });
    document.getElementById('kal-prazno').hidden = any > 0;
    btn.textContent = show ? 'Sakrij prošla događanja' : 'Prikaži prošla događanja';
    btn.setAttribute('aria-pressed', show);
  }
  btn.addEventListener('click', function () { show = !show; apply(); });
  apply();
})();
</script>
