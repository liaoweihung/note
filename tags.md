---
layout: default
title: 🏷️ 全站標籤總覽
---

# 🏷️ 全站標籤總覽

{% for tag in site.tags %}
  <h2 id="{{ tag[0] }}" style="color: #2c7a7b; border-bottom: 2px solid #e2e8f0; padding-bottom: 10px; margin-top: 40px;">
    📌 {{ tag[0] }} <span style="font-size: 0.6em; color: #718096; font-weight: normal;">({{ tag[1].size }} 篇)</span>
  </h2>
  
  <ul>
    {% for post in tag[1] %}
      <li style="margin-bottom: 10px; list-style-type: none;">
        <a href="{{ post.url | relative_url }}" style="text-decoration: none; color: #3182ce; font-size: 1.1em; font-weight: bold;">
          {{ post.title }}
        </a>
        <span style="color: #a0aec0; font-size: 0.85em; margin-left: 10px;">{{ post.date | date: "%Y-%m-%d" }}</span>
      </li>
    {% endfor %}
  </ul>
{% endfor %}
