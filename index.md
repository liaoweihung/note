---
layout: default
title: 首頁
---

## 📚 藥學自修資料總覽

> 點擊標題可查看詳細資料

---

### 🔖 最新收錄列表

<ul>
{% assign sorted_meds = site.meds | sort: 'date' | reverse %}

{% for post in sorted_meds %}
  <li style="margin-bottom: 15px; list-style-type: none; display: flex; align-items: center;">
    <strong><a href="{{ '/tags/# ' | append: tag | relative_url }}" style="font-size: 1.2em; text-decoration: none; color: #3182ce;">{{ post.title }}</a></strong>
    <span style="color: #718096; font-size: 0.85em; background: #edf2f7; padding: 3px 8px; border-radius: 6px; margin-left: 12px; font-weight: normal;">
      {{ post.category }}
    </span>
  </li>

  {% if forloop.index == 8 %}
    {% assign raw_tags = "" %}
    {% for p in site.meds %}
      {% for t in p.tags %}
        {% assign raw_tags = raw_tags | append: t | append: "|||" %}
      {% endfor %}
    {% endfor %}
    {% assign unique_tags = raw_tags | split: "|||" | uniq %}

    <div style="margin: 40px 0; padding: 20px; background: #f7fafc; border-radius: 12px; box-shadow: 0 2px 4px rgba(0,0,0,0.02);">
      <div style="display: flex; justify-content: space-between; align-items: center;">
        <h3 style="margin: 0; color: #4a5568; font-size: 1.1em;">🏷️ 探索更多主題標籤</h3>
        <button onclick="toggleTags()" id="tagToggleBtn" style="background: white; border: 1px solid #cbd5e0; color: #4a5568; padding: 6px 12px; border-radius: 8px; cursor: pointer; font-size: 0.9em; font-weight: bold; transition: background 0.2s;">
          展開標籤 ▼
        </button>
      </div>

      <div id="tagList" style="line-height: 2.2; margin-top: 20px; display: none;">
        {% for tag in unique_tags %}
          {% if tag != "" %}
            {% assign tag_count = 0 %}
            {% for p2 in site.meds %}
              {% if p2.tags contains tag %}
                {% assign tag_count = tag_count | plus: 1 %}
              {% endif %}
            {% endfor %}
            <a href="{{ '/tags/' | relative_url }}#{{ tag }}" style="display: inline-block; background: white; border: 1px solid #cbd5e0; color: #2d3748; padding: 4px 12px; border-radius: 20px; text-decoration: none; font-size: 0.9em; margin-right: 8px; margin-bottom: 8px; box-shadow: 0 1px 2px rgba(0,0,0,0.05); transition: background 0.2s;">
              {{ tag }} <span style="color: #a0aec0; font-size: 0.85em;">({{ tag_count }})</span>
            </a>
          {% endif %}
        {% endfor %}
      </div>
    </div>

    <script>
      function toggleTags() {
        var list = document.getElementById("tagList");
        var btn = document.getElementById("tagToggleBtn");
        if (list.style.display === "none") {
          list.style.display = "block";
          btn.innerHTML = "收起標籤 ▲";
          btn.style.backgroundColor = "#edf2f7"; // 展開時按鈕變灰底
        } else {
          list.style.display = "none";
          btn.innerHTML = "展開標籤 ▼";
          btn.style.backgroundColor = "white"; // 收起時恢復白底
        }
      }
    </script>
  {% endif %}
  {% endfor %}
</ul>
