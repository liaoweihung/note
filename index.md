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
<div id="original-list">
  <ul>
  {% for post in sorted_meds %}
    <li style="margin-bottom: 15px; list-style-type: none; display: flex; align-items: center;">
      <strong><a href="{{ post.url | relative_url }}" style="font-size: 1.2em; text-decoration: none; color: #3182ce;">{{ post.title }}</a></strong>
      <span style="color: #718096; font-size: 0.85em; background: #edf2f7; padding: 3px 8px; border-radius: 6px; margin-left: 12px; font-weight: normal;">
        {{ post.category }}
      </span>
    </li>
  {% endfor %}
  </ul>
</div>
{% for post in sorted_meds %}
  <li style="margin-bottom: 15px; list-style-type: none; display: flex; align-items: center;">
    <strong><a href="{{ post.url | append: tag | relative_url }}" style="font-size: 1.2em; text-decoration: none; color: #3182ce;">{{ post.title }}</a></strong>
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
    <div style="margin: 30px 0;">
  <input type="text" id="search-input" placeholder="🔍 輸入關鍵字搜尋標題、分類或標籤..." style="width: 100%; padding: 15px 20px; font-size: 1.1em; border: 2px solid #cbd5e0; border-radius: 8px; outline: none; box-shadow: 0 2px 4px rgba(0,0,0,0.05); transition: border-color 0.2s;">
</div>

<ul id="search-results" style="padding: 0; display: none;"></ul>

<script>
  // 1. 讓 Jekyll 幫我們把所有文章資料打包成 JavaScript 陣列
  const posts = [
    {% for post in site.meds %}
    {
      title: "{{ post.title | escape }}",
      url: "{{ post.url | relative_url }}",
      category: "{{ post.category | escape }}",
      tags: "{{ post.tags | join: ' ' | escape }}",
      date: "{{ post.date | date: '%Y-%m-%d' }}"
    }{% unless forloop.last %},{% endunless %}
    {% endfor %}
  ];

  const searchInput = document.getElementById('search-input');
  const searchResults = document.getElementById('search-results');
  const originalList = document.getElementById('original-list');

  // 2. 監聽輸入框的打字動作
  searchInput.addEventListener('input', function() {
    const query = this.value.toLowerCase().trim();

    // 如果搜尋框是空的，顯示原本的列表，隱藏搜尋結果
    if (query === '') {
      searchResults.style.display = 'none';
      if (originalList) originalList.style.display = 'block';
      return;
    }

    // 開始搜尋：隱藏原本的列表，打開搜尋結果區塊
    searchResults.style.display = 'block';
    if (originalList) originalList.style.display = 'none';

    // 3. 過濾資料：比對標題、標籤或分類
    const filtered = posts.filter(post => {
      return post.title.toLowerCase().includes(query) ||
             post.tags.toLowerCase().includes(query) ||
             post.category.toLowerCase().includes(query);
    });

    // 4. 將結果印到畫面上
    if (filtered.length === 0) {
      searchResults.innerHTML = '<li style="list-style: none; text-align: center; color: #a0aec0; padding: 20px; background: #fdfdfd; border-radius: 8px;">找不到相關內容 😢</li>';
    } else {
      searchResults.innerHTML = filtered.map(post => `
        <li style="margin-bottom: 15px; list-style-type: none; display: flex; align-items: center; padding: 8px 0; border-bottom: 1px dashed #edf2f7;">
          <strong><a href="${post.url}" style="font-size: 1.2em; text-decoration: none; color: #3182ce;">${post.title}</a></strong>
          <span style="color: #718096; font-size: 0.85em; background: #edf2f7; padding: 3px 8px; border-radius: 6px; margin-left: 12px;">${post.category}</span>
        </li>
      `).join('');
    }
  });

  // 點擊搜尋框時有變色效果，增加質感
  searchInput.addEventListener('focus', () => searchInput.style.borderColor = '#3182ce');
  searchInput.addEventListener('blur', () => searchInput.style.borderColor = '#cbd5e0');
</script>
  {% endif %}
  {% endfor %}
</ul>
