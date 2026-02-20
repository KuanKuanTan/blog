# MkDocs 進階技巧：打造專業級文件網站

在熟悉 MkDocs 基礎後，這篇文章會帶你深入進階配置、自訂主題、插件系統、效能優化等技巧，讓你的文件網站更專業、更強大。

---

## 目錄

- [進階配置選項](#進階配置選項)
- [自訂主題與樣式](#自訂主題與樣式)
- [插件系統](#插件系統)
- [進階 Markdown 擴充功能](#進階-markdown-擴充功能)
- [自訂 HTML/CSS/JavaScript](#自訂-htmlcssjavascript)
- [效能優化](#效能優化)
- [多語言支援](#多語言支援)
- [進階部署策略](#進階部署策略)
- [自動化工作流程](#自動化工作流程)

---

## 進階配置選項

### 1. 完整的 `mkdocs.yml` 結構

一個完整的 MkDocs 配置檔可以包含以下區塊：

```yaml
site_name: My Blog
site_description: 個人部落格 - 分享技術、生活與思考
site_author: Your Name
site_url: https://yourusername.github.io/blog
repo_url: https://github.com/yourusername/blog
repo_name: yourusername/blog
edit_uri: edit/main/docs/

# 嚴格模式：檢查所有連結是否有效
strict: true

# 開發模式設定
dev_addr: 127.0.0.1:8000
```

### 2. 進階導航配置

```yaml
nav:
  - 首頁: index.md
  - 文章:
    - 文章列表: posts/index.md
    - 入門: posts/mkdocs-intro.md
    - 進階: posts/mkdocs-advanced.md
  - 關於: about.md

# 自動生成導航（不建議用於大型專案）
# nav:
#   - Home: index.md
#   - User Guide:
#     - Introduction: getting-started.md
#     - Advanced: advanced.md
```

### 3. 多環境配置

使用環境變數來區分開發與生產環境：

```yaml
site_url: ${SITE_URL:-https://yourusername.github.io/blog}
```

或在 Python 中動態設定：

```python
# build.py
import os
import yaml

env = os.getenv('ENV', 'dev')
config = yaml.safe_load(open('mkdocs.yml'))

if env == 'production':
    config['site_url'] = 'https://yourusername.github.io/blog'
else:
    config['site_url'] = 'http://localhost:8000'

with open('mkdocs.yml', 'w') as f:
    yaml.dump(config, f)
```

---

## 自訂主題與樣式

### 1. Material 主題進階設定

```yaml
theme:
  name: material
  language: zh-TW
  
  # 自訂圖示
  icon:
    repo: fontawesome/brands/github
    logo: material/library
  
  # 進階調色板設定
  palette:
    - scheme: default
      primary: indigo
      accent: indigo
      toggle:
        icon: material/brightness-7
        name: 切換到深色模式
    - scheme: slate
      primary: indigo
      accent: indigo
      toggle:
        icon: material/brightness-4
        name: 切換到淺色模式
  
  # 啟用進階功能
  features:
    - navigation.tabs
    - navigation.sections
    - navigation.expand
    - navigation.top
    - search.suggest
    - search.highlight
    - content.code.annotate
    - content.code.copy
    - content.tooltips
    - toc.integrate
  
  # 自訂字體
  font:
    text: Roboto
    code: Roboto Mono
```

### 2. 自訂 CSS

建立 `docs/assets/css/custom.css`：

```css
/* 自訂樣式 */
:root {
  --md-primary-fg-color: #6366f1;
  --md-accent-fg-color: #8b5cf6;
}

/* 自訂程式碼區塊樣式 */
.highlight .hll {
  background-color: #ffffcc;
}

/* 自訂表格樣式 */
.md-typeset table:not([class]) {
  border-collapse: collapse;
  border: 1px solid var(--md-default-fg-color--lighter);
}
```

在 `mkdocs.yml` 中引入：

```yaml
theme:
  name: material
  custom_dir: docs/overrides
  
extra_css:
  - assets/css/custom.css
```

### 3. 自訂 HTML 模板

建立 `docs/overrides/main.html`：

```html
{% extends "base.html" %}

{% block content %}
  {{ super() }}
  
  <!-- 自訂內容 -->
  <div class="custom-footer">
    <p>© 2026 Your Name. All rights reserved.</p>
  </div>
{% endblock %}
```

---

## 插件系統

### 1. 內建插件配置

```yaml
plugins:
  - search:
      lang: zh-TW
      separator: '[\s\-,:!=\[\]()"`/]+|\.(?!\d)|&[lg]t;|&amp;'
  
  - minify:
      minify_html: true
      minify_js: true
      minify_css: true
  
  - git-revision-date-localized:
      enable_creation_date: true
      type: timeago
      locale: zh_TW
```

### 2. 第三方插件推薦

安裝方式：

```powershell
py -3.12 -m pip install mkdocs-git-revision-date-localized-plugin
py -3.12 -m pip install mkdocs-minify-plugin
py -3.12 -m pip install mkdocs-mermaid2-plugin
```

使用範例：

```yaml
plugins:
  - mermaid2:
      arguments:
        theme: default
        themeVariables:
          primaryColor: '#6366f1'
  
  - git-revision-date-localized:
      enable_creation_date: true
      type: timeago
      locale: zh_TW
  
  - i18n:
      default_language: zh-TW
      languages:
        zh-TW: 繁體中文
        en: English
```

### 3. 自訂插件開發

建立 `plugins/custom_plugin.py`：

```python
from mkdocs.plugins import BasePlugin
from mkdocs.config import config_options

class CustomPlugin(BasePlugin):
    config_scheme = (
        ('enabled', config_options.Type(bool, default=True)),
    )

    def on_page_markdown(self, markdown, page, config, files):
        if self.config['enabled']:
            # 處理 Markdown 內容
            markdown = markdown.replace('{{date}}', '2026-02-20')
        return markdown
```

在 `mkdocs.yml` 中使用：

```yaml
plugins:
  - custom_plugin:
      enabled: true
```

---

## 進階 Markdown 擴充功能

### 1. 程式碼高亮與註解

```yaml
markdown_extensions:
  - pymdownx.highlight:
      anchor_linenums: true
      line_spans: __span
      pygments_lang_class: true
  
  - pymdownx.superfences:
      custom_fences:
        - name: mermaid
          class: mermaid
          format: !!python/name:pymdownx.superfences.fence_code_format
  
  - pymdownx.inlinehilite
  - pymdownx.snippets
```

### 2. 表格與數學公式

```yaml
markdown_extensions:
  - tables
  - pymdownx.tabbed:
      alternate_style: true
      combine_header_slug: true
  
  - pymdownx.arithmatex:
      generic: true
  
  - pymdownx.tasklist:
      custom_checkbox: true
```

使用範例：

````markdown
```python
def hello():
    print("Hello, World!")  # (1)
```

1. 這是一個註解
````

### 3. 提示框與摺疊區塊

```yaml
markdown_extensions:
  - admonition
  - pymdownx.details:
      details: true
```

使用範例：

```markdown
!!! note "提示"
    這是一個提示框。

!!! tip "小技巧"
    這是一個小技巧。

!!! warning "警告"
    這是一個警告訊息。

!!! danger "危險"
    這是一個危險警告。

??? note "可摺疊的內容"
    點擊標題可以展開或收合這個區塊。
```

### 4. 標籤與徽章

```yaml
markdown_extensions:
  - attr_list
  - pymdownx.betterem:
      smart_enable: all
```

使用範例：

```markdown
這是一個 {.highlight} 高亮文字。

這是一個 {.warning} 警告文字。
```

---

## 自訂 HTML/CSS/JavaScript

### 1. 自訂 JavaScript

建立 `docs/assets/js/custom.js`：

```javascript
// 自訂 JavaScript
document.addEventListener('DOMContentLoaded', function() {
  // 添加自訂功能
  console.log('MkDocs 網站已載入');
  
  // 例如：添加複製按鈕
  document.querySelectorAll('pre code').forEach(function(block) {
    var button = document.createElement('button');
    button.className = 'copy-button';
    button.textContent = '複製';
    block.parentElement.appendChild(button);
  });
});
```

在 `mkdocs.yml` 中引入：

```yaml
extra_javascript:
  - assets/js/custom.js
```

### 2. 自訂 HTML 區塊

在 Markdown 中使用 HTML：

```html
<div class="custom-box">
  <h3>自訂區塊</h3>
  <p>這裡可以放任何 HTML 內容。</p>
</div>
```

搭配 CSS：

```css
.custom-box {
  border: 2px solid var(--md-primary-fg-color);
  border-radius: 8px;
  padding: 1rem;
  margin: 1rem 0;
}
```

---

## 效能優化

### 1. 啟用 Minify 插件

```yaml
plugins:
  - minify:
      minify_html: true
      minify_js: true
      minify_css: true
      htmlmin_opts:
        remove_comments: true
```

### 2. 圖片優化

使用 WebP 格式並設定懶載入：

```yaml
markdown_extensions:
  - attr_list
  - pymdownx.superfences
```

在 Markdown 中：

```markdown
![圖片](image.webp){ loading=lazy }
```

### 3. 快取策略

在 `docs/overrides/main.html` 中添加：

```html
{% block extrahead %}
  <meta http-equiv="Cache-Control" content="max-age=3600">
{% endblock %}
```

### 4. CDN 加速

使用 CDN 載入外部資源：

```yaml
theme:
  name: material
  custom_dir: docs/overrides
```

在模板中：

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/material-icons@latest/iconfont/material-icons.min.css">
```

---

## 多語言支援

### 1. 使用 i18n 插件

```yaml
plugins:
  - i18n:
      default_language: zh-TW
      languages:
        zh-TW: 繁體中文
        en: English
        ja: 日本語
```

### 2. 多語言內容結構

```
docs/
├── zh-TW/
│   ├── index.md
│   └── posts/
└── en/
    ├── index.md
    └── posts/
```

### 3. 語言切換器

Material 主題內建語言切換功能，只需在配置中設定即可。

---

## 進階部署策略

### 1. GitHub Actions 自動部署

建立 `.github/workflows/deploy.yml`：

```yaml
name: Deploy MkDocs

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      
      - name: Install dependencies
        run: |
          pip install mkdocs-material
          pip install mkdocs-minify-plugin
      
      - name: Build site
        run: mkdocs build --strict
      
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site
```

### 2. 多環境部署

```yaml
# .github/workflows/deploy.yml
jobs:
  deploy-dev:
    if: github.ref == 'refs/heads/develop'
    # 部署到開發環境
  
  deploy-prod:
    if: github.ref == 'refs/heads/main'
    # 部署到生產環境
```

### 3. 預覽部署

使用 GitHub Actions 的預覽功能：

```yaml
- name: Deploy Preview
  if: github.event_name == 'pull_request'
  # 部署到預覽環境
```

---

## 自動化工作流程

### 1. 自動檢查連結

建立 `.github/workflows/check-links.yml`：

```yaml
name: Check Links

on: [push, pull_request]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Check links
        run: |
          pip install mkdocs-material
          mkdocs build --strict
```

### 2. 自動生成 Sitemap

使用插件自動生成：

```yaml
plugins:
  - sitemap:
      filename: sitemap.xml
```

### 3. 自動更新日期

使用 git-revision-date-localized 插件自動顯示最後更新日期。

---

## 結語

這些進階技巧可以讓你的 MkDocs 網站：

- ✨ **更專業**：自訂主題與樣式
- 🚀 **更快速**：效能優化與快取
- 🌍 **更國際化**：多語言支援
- 🔧 **更自動化**：CI/CD 整合
- 📊 **更豐富**：進階 Markdown 功能

持續探索 MkDocs 的各種可能性，打造屬於你的專業文件網站！

---

**相關資源：**

- [MkDocs 官方文件](https://www.mkdocs.org/)
- [Material for MkDocs 文件](https://squidfunk.github.io/mkdocs-material/)
- [MkDocs 插件列表](https://github.com/mkdocs/mkdocs/wiki/MkDocs-Plugins)
