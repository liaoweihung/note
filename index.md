---
layout: default
title: 首頁
---

## 📚 藥學自修資料總覽

> 點擊標題可查看詳細資料

---

<h3 style="color: #4a5568; margin-bottom: 20px;">🔖 最新收錄列表</h3>

<ul style="padding-left: 0; margin-top: 0;">

  {% assign sorted_meds = site.meds | sort: 'date' | reverse %}
  
  {% assign raw_tags = "" %}
  {% for p in site.meds %}
    {% for t in p.tags %}
      {% assign raw_tags = raw_tags | append: t | append: "|||" %}
    {% endfor %}
  {% endfor %}
  {% assign unique_tags = raw_tags | split: "|||" | uniq %}

  {% for post in sorted_meds %}
  
    <li class="post-item" style="margin-bottom: 15px; list-style-type: none; display: flex; align-items: center;">
      <strong><a href="{{ post.url | relative_url }}" style="font-size: 1.2em; text-decoration: none; color: #3182ce;">{{ post.title }}</a></strong>
      <span style="color: #718096; font-size: 0.85em; background: #edf2f7; padding: 3px 8px; border-radius: 6px; margin-left: 12px; font-weight: normal;">
        {{ post.category }}
      </span>
    </li>

    {% if forloop.index == 7 %}
      <li style="list-style-type: none; margin: 40px 0;">
        <div style="padding: 20px; background: #f7fafc; border-radius: 12px; box-shadow: 0 2px 4px rgba(0,0,0,0.02);">
          <div style="display: flex; justify-content: space-between; align-items: center;">
            <h3 style="margin: 0; color: #4a5568; font-size: 1.1em;">🏷️ 更多主題標籤或搜尋本站</h3>
            <button onclick="toggleTags()" id="tagToggleBtn" style="background: white; border: 1px solid #cbd5e0; color: #4a5568; padding: 6px 12px; border-radius: 8px; cursor: pointer; font-size: 0.9em; font-weight: bold; transition: background 0.2s;">
              展開 ▼
            </button>
          </div>

          <div id="tagList" style="margin-top: 20px; display: none;">
            
            <div style="margin-bottom: 20px;">
              <input type="text" id="search-input" placeholder="🔍 輸入關鍵字搜尋標題、分類或標籤..." style="width: 100%; padding: 12px 15px; font-size: 1em; border: 2px solid #cbd5e0; border-radius: 8px; outline: none; box-shadow: inset 0 1px 3px rgba(0,0,0,0.05); transition: border-color 0.2s; margin-bottom: 10px;">
              
              <label style="font-size: 0.9em; color: #4a5568; display: flex; align-items: center; cursor: pointer; width: fit-content;">
                <input type="checkbox" id="search-content-cb" style="margin-right: 8px; transform: scale(1.2); cursor: pointer;">
                🔲 包含「內文筆記」深度搜尋 (勾選後可搜尋細節)
              </label>
            </div>

            <ul id="search-results" style="padding: 0; display: none; margin-bottom: 20px;"></ul>

            <div id="tags-container" style="line-height: 2.2;">
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
        </div>
      </li>
    {% endif %}

  {% endfor %}
</ul>

<script>
  // --- 收合按鈕功能 ---
  function toggleTags() {
    var list = document.getElementById("tagList");
    var btn = document.getElementById("tagToggleBtn");
    var searchInput = document.getElementById('search-input');
    var searchCb = document.getElementById('search-content-cb');

    if (list.style.display === "none") {
      list.style.display = "block";
      btn.innerHTML = "收起 ▲";
      btn.style.backgroundColor = "#edf2f7"; 
    } else {
      list.style.display = "none";
      btn.innerHTML = "展開 ▼";
      btn.style.backgroundColor = "white"; 
      
      // 收起時，清空搜尋框並取消內文搜尋的打勾，讓一切恢復預設狀態
      if(searchInput) {
        searchInput.value = "";
        searchCb.checked = false; 
        searchInput.dispatchEvent(new Event('input'));
      }
    }
  }

  // --- 即時搜尋引擎功能 ---
  const posts = [
    {% for post in site.meds %}
    {
      title: "{{ post.title | escape }}",
      url: "{{ post.url | relative_url }}",
      category: "{{ post.category | escape }}",
      tags: "{{ post.tags | join: ' ' | escape }}",
      // 將內文的 HTML 標籤與換行濾除後打包進來
      content: {{ post.content | strip_html | strip_newlines | jsonify }}
    }{% unless forloop.last %},{% endunless %}
    {% endfor %}
  ];

  const searchInput = document.getElementById('search-input');
  const searchResults = document.getElementById('search-results');
  const postItems = document.querySelectorAll('.post-item');
  const tagsContainer = document.getElementById('tags-container');
  const searchContentCb = document.getElementById('search-content-cb');

  if(searchInput) {
    // 獨立出一個「執行搜尋」的函數，讓打字跟打勾都可以呼叫它
    function performSearch() {
      const query = searchInput.value.toLowerCase().trim();
      const includeContent = searchContentCb.checked;

      if (query === '') {
        searchResults.style.display = 'none';
        postItems.forEach(item => item.style.display = 'flex');
        if (tagsContainer) tagsContainer.style.display = 'block';
        return;
      }

      searchResults.style.display = 'block';
      postItems.forEach(item => item.style.display = 'none');
      if (tagsContainer) tagsContainer.style.display = 'none';

      const filtered = posts.filter(post => {
        // 基本比對：標題、標籤、分類
        const matchBasic = post.title.toLowerCase().includes(query) ||
                           post.tags.toLowerCase().includes(query) ||
                           post.category.toLowerCase().includes(query);
        
        // 如果有打勾，就額外比對內文
        if (includeContent) {
          return matchBasic || (post.content && post.content.toLowerCase().includes(query));
        } else {
          return matchBasic;
        }
      });

      if (filtered.length === 0) {
        searchResults.innerHTML = '<li style="list-style: none; text-align: center; color: #a0aec0; padding: 20px; background: #fff; border: 1px dashed #cbd5e0; border-radius: 8px;">找不到相關內容 😢</li>';
      } else {
        searchResults.innerHTML = filtered.map(post => `
          <li style="margin-bottom: 10px; list-style-type: none; display: flex; align-items: center; padding: 12px; background: #fff; border: 1px solid #e2e8f0; border-radius: 8px;">
            <strong><a href="${post.url}" style="font-size: 1.1em; text-decoration: none; color: #3182ce;">${post.title}</a></strong>
            <span style="color: #718096; font-size: 0.8em; background: #edf2f7; padding: 3px 8px; border-radius: 6px; margin-left: auto;">${post.category}</span>
          </li>
        `).join('');
      }
    }

    // 當輸入框打字時，觸發搜尋
    searchInput.addEventListener('input', performSearch);

    // 當打勾或取消打勾時，如果搜尋框裡面有字，就瞬間重新搜尋
    searchContentCb.addEventListener('change', function() {
      if (searchInput.value.trim() !== '') {
        performSearch();
      }
    });

    searchInput.addEventListener('focus', () => searchInput.style.borderColor = '#3182ce');
    searchInput.addEventListener('blur', () => searchInput.style.borderColor = '#cbd5e0');
  }
</script>
