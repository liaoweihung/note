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
   <span style="color: #718096; font-size: 0.9em;">
      分類：{{ post.category }} | 
      標籤：
      {% for tag in post.tags %}
        <span class="tag">{{ tag }}</span>
      {% endfor %}
    </span>
  </li>
{% endfor %}
</ul>
