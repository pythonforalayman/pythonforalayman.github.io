---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
title: "Main Repository"
---

{% for post in site.categories.python3 %}
<li><span>{{ post.date | date_to_string }}</span> » <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a></li>
{% endfor %}
<ul class="posts">
{% assign sorted_categories = site.categories | sort {|left, right| left[0] <=> right[0]} %}
{% for tag in sorted_categories %}
  {% if tag[1] != null %}
  <h2 class='tag' id="{{ tag[0] }}">{{ tag[0] | camelize }}</h2>
  <ul class="post-list">
    {% assign pages_list = tag[1] %}
    {% for post in pages_list reversed %}
      {% if post.title != null and post.hidden != true and post.published != false %}
      {% if group == null or group == post.group %}
      <li><a href="{{ site.url }}{{ post.url }}">
          <!--<span class="entry-date"><time datetime="{{ post.date | date_to_xmlschema }}" itemprop="datePublished">{{ post.date | date: "%B %d, %Y" }}</time></span>
          &bull;-->
          {{ post.title }}
        </a></li>
      {% endif %}
      {% endif %}
    {% endfor %}
    {% assign pages_list = nil %}
    {% assign group = nil %}
  </ul>
  {% endif %}
{% endfor %}
</ul>


