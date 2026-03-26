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
  <li style="margin-bottom: 15px; list-style-type: none;">
    <strong><a href="{{ post.url | relative_url }}" style="font-size: 1.2em; text-decoration: none; color: #3182ce;">{{ post.title }}</a></strong>
    <br>
  <span style="color: #718096; font-size: 0.9em; background: #edf2f7; padding: 3px 8px; border-radius: 6px;">
      {{ post.category }}
    </span>
  </li>
{% endfor %}
</ul>
