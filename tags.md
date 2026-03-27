---
layout: default
title: 🏷️ 全站標籤總覽
permalink: /tags/
---

# 🏷️ 全站標籤總覽

{% assign raw_tags = "" %}
{% for post in site.meds %}
  {% for tag in post.tags %}
    {% assign raw_tags = raw_tags | append: tag | append: "|||" %}
  {% endfor %}
{% endfor %}
{% assign unique_tags = raw_tags | split: "|||" | uniq | sort %}

<div style="margin-bottom: 40px; padding: 15px; background: #fdfdfd; border: 1px solid #eee; border-radius: 8px;">
  <strong>快速索引：</strong>
  {% for tag in unique_tags %}
    {% if tag != "" %}
      <a href="#{{ tag }}" style="margin-right: 10px; text-decoration: none; color: #3182ce;">#{{ tag }}</a>
    {% endif %}
  {% endfor %}
</div>

{% for tag in unique_tags %}
  {% if tag != "" %}
    {% assign tag_count = 0 %}
    {% for p in site.meds %}
      {% if p.tags contains tag %}
        {% assign tag_count = tag_count | plus: 1 %}
      {% endif %}
    {% endfor %}
    
    <h2 id="{{ tag }}" style="color: #2c7a7b; border-bottom: 2px solid #e2e8f0; padding-bottom: 10px; margin-top: 50px;">
      📌 {{ tag }} <span style="font-size: 0.6em; color: #718096; font-weight: normal;">({{ tag_count }} 篇)</span>
    </h2>
    
    <ul>
      {% for post in site.meds %}
        {% if post.tags contains tag %}
          <li style="margin-bottom: 12px; list-style-type: none;">
            <a href="{{ post.url | relative_url }}" style="text-decoration: none; color: #3182ce; font-size: 1.1em; font-weight: bold;">
              {{ post.title }}
            </a>
            <span style="color: #a0aec0; font-size: 0.85em; margin-left: 10px;">{{ post.date | date: "%Y-%m-%d" }}</span>
          </li>
        {% endif %}
      {% endfor %}
    </ul>
  {% endif %}
{% endfor %}
