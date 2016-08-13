---
layout: layout
title: Tags
description: List of articles and posts by tags.
---

{% for item in (0..site.tags.size) %}
{% unless forloop.last %}

{% capture this_word %}{{ tag_words[item] }}{% endcapture %}
<div class="grid-container" id="{{this_word | cgi_escape }}">
  <div class="grid archive">
    <div class="grid-item one-quarter archive-border-top">
      <h1 class="archive-year">{{ this_word }}</h1>
    </div>
    
    <div class="grid-item three-quarters archive-border-top">
      <ul class="archive-list">
        {% for post in site.tags[this_word] %}{% if post.title != null %}
        <li>
          <a href="{{ post.url }}" class="archive-list-link">
            <strong class="archive-list-title">{{ post.title }}</strong>
            <em class="archive-list-date">{{ post.date | date: '%B %Y' }}</em>
          </a>
        </li>
        {% endif %}{% endfor %}
      </ul>
    </div>
  </div>
</div>
{% endunless %}
{% endfor %}