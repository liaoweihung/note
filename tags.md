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
{% assign unique_tags = raw_tags | split: "|||" | uniq %}

{% assign count_tag_pairs = "" %}
{% for tag in unique_tags %}
{% if tag != "" %}
{% assign count = 0 %}
{% for post in site.meds %}
{% if post.tags contains tag %}
{% assign count = count | plus: 1 %}
{% endif %}
{% endfor %}
{% assign padded_count = count | prepend: '000' | slice: -3, 3 %}
{% assign pair = padded_count | append: ":::" | append: tag %}
{% assign count_tag_pairs = count_tag_pairs | append: pair | append: "|||" %}
{% endif %}
{% endfor %}

{% assign sorted_pairs = count_tag_pairs | split: "|||" | sort | reverse %}

<div style="margin-bottom: 40px; padding: 15px; background: #fdfdfd; border: 1px solid #eee; border-radius: 8px;">
<strong>熱門索引：</strong>
{% for pair in sorted_pairs %}
{% if pair != "" %}
{% assign parts = pair | split: ":::" %}
{% assign tag_name = parts[1] %}
{% assign tag_count = parts[0] | plus: 0 %}
<a href="#{{ tag_name }}" style="margin-right: 10px; text-decoration: none; color: #3182ce; display: inline-block; margin-bottom: 5px;">#{{ tag_name }}({{ tag_count }})</a>
{% endif %}
{% endfor %}
</div>

{% for pair in sorted_pairs %}
{% if pair != "" %}
{% assign parts = pair | split: ":::" %}
{% assign tag_name = parts[1] %}
{% assign tag_count = parts[0] | plus: 0 %}

<h2 id="{{ tag_name }}" style="color: #2c7a7b; border-bottom: 2px solid #e2e8f0; padding-bottom: 10px; margin-top: 50px;">
📌 {{ tag_name }} <span style="font-size: 0.6em; color: #718096; font-weight: normal;">({{ tag_count }} 篇)</span>
</h2>

<ul>
{% for post in site.meds %}
{% if post.tags contains tag_name %}
<li style="margin-bottom: 12px; list-style-type: none;">
<a href="{{ post.url | relative_url }}" style="text-decoration: none; color: #3182ce; font-size: 1.1em; font-weight: bold;">{{ post.title }}</a>
<span style="color: #a0aec0; font-size: 0.85em; margin-left: 10px;">{{ post.date | date: "%Y-%m-%d" }}</span>
</li>
{% endif %}
{% endfor %}
</ul>

{% endif %}
{% endfor %}
