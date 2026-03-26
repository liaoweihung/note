---
layout: default
title: 🏷️ 全站標籤總覽
---

# 🏷️ 全站標籤總覽

{% assign all_tags = "" | split: "" %}
{% for post in site.meds %}
  {% if post.tags %}
    {% assign all_tags = all_tags | concat: post.tags %}
  {% endif %}
{% endfor %}
{% assign unique_tags = all_tags | uniq %}

{% for tag in unique_tags %}
  {% assign tag_count = 0 %}
  {% for post in site.meds %}
    {% if post.tags contains tag %}
      {% assign tag_count = tag_count | plus: 1 %}
    {% endif %}
  {% endfor %}
  
  <h2 id="{{ tag }}" style="color: #2c7a7b; border-bottom: 2px solid #e2e8f0; padding-bottom: 10px; margin-top: 40px;">
    📌 {{ tag }} <span style="font-size: 0.6em; color: #718096; font-weight: normal;">({{ tag_count }} 篇)</span>
  </h2>
  
  <ul>
    {% for post in site.meds %}
      {% if post.tags contains tag %}
        <li style="margin-bottom: 10px; list-style-type: none;">
          <a href="{{ post.url | relative_url }}" style="text-decoration: none; color: #3182ce; font-size: 1.1em; font-weight: bold;">
            {{ post.title }}
          </a>
        </li>
      {% endif %}
    {% endfor %}
  </ul>
{% endfor %}
