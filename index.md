---
layout: default
title: 首頁
---

## 📚 藥學自修資料總覽

> 點擊標題可查看詳細資料

---

### 🔖 最新收錄列表

<ul>
{% for post in site.meds %}
  <li style="margin-bottom: 15px; list-style-type: none; display: flex; align-items: center;">
    <strong><a href="{{ post.url | relative_url }}" style="font-size: 1.2em; text-decoration: none; color: #3182ce;">{{ post.title }}</a></strong>
    <span style="color: #718096; font-size: 0.85em; background: #edf2f7; padding: 3px 8px; border-radius: 6px; margin-left: 12px; font-weight: normal;">
      {{ post.category }}
    </span>
  </li>

  {% if forloop.index == 8 %}
    <div style="margin: 40px 0; padding: 20px; background: #f7fafc; border-radius: 12px; box-shadow: 0 2px 4px rgba(0,0,0,0.02);">
      <h3 style="margin-top: 0; color: #4a5568; font-size: 1.1em;">🏷️ 探索更多主題標籤</h3>
      <div style="line-height: 2.2;">
        {% for tag in site.tags %}
          <a href="{{ '/tags/' | relative_url }}#{{ tag[0] }}" style="display: inline-block; background: white; border: 1px solid #cbd5e0; color: #2d3748; padding: 4px 12px; border-radius: 20px; text-decoration: none; font-size: 0.9em; margin-right: 8px; box-shadow: 0 1px 2px rgba(0,0,0,0.05); transition: background 0.2s;">
            {{ tag[0] }} <span style="color: #a0aec0; font-size: 0.85em;">({{ tag[1].size }})</span>
          </a>
        {% endfor %}
      </div>
    </div>
  {% endif %}
  {% endfor %}
</ul>
