# MkDocs 入門：用簡單方式打造文件與部落格

MkDocs 是一個專門用來寫「文件網站」的靜態網站產生器，非常適合技術文件與個人部落格。  
這篇文章會帶你快速了解 MkDocs 的概念，並示範如何在本機開發、以及部署到 GitHub Pages。

---

## 什麼是 MkDocs？

- **定位**：專為「技術文件」與「說明文件」設計的靜態網站工具  
- **語法**：全部使用 Markdown 撰寫內容  
- **設定**：透過 `mkdocs.yml` 控制網站標題、導航列、主題等  
- **輸出**：將 Markdown 轉成一個純靜態網站（HTML + CSS + JS）

只要會寫 Markdown，就可以很快做出一個漂亮又實用的文件或部落格。

---

## 專案結構長什麼樣子？

以這個部落格為例，主要結構大概是：

```text
blog/
├── docs/              # 所有頁面與文章
│   ├── index.md       # 首頁
│   ├── about.md       # 關於我
│   └── posts/         # 文章目錄
│       ├── index.md   # 文章列表
│       └── mkdocs-intro.md  # 本文
└── mkdocs.yml         # MkDocs 設定檔
```

- `docs/` 底下的每個 `.md` 檔，最後都會變成一個網頁  
- `mkdocs.yml` 負責設定網站名稱、主題、導覽列等

---

## 本機開發：即時預覽

確保已安裝好 Python 3.12，然後在專案根目錄執行：

```powershell
py -3.12 -m pip install mkdocs mkdocs-material
py -3.12 -m mkdocs serve
```

接著打開瀏覽器，前往：

- `http://127.0.0.1:8000/`

每次你修改 `docs/` 裡的 Markdown 檔案並存檔，頁面會自動重新載入，非常適合一邊寫一邊看效果。

---

## 部署到 GitHub Pages

這個部落格採用 MkDocs 內建的 `gh-deploy` 指令，直接幫你：

1. 產生網站到暫存資料夾
2. 建立或更新 `gh-pages` 分支
3. 推送到 GitHub

在專案根目錄執行：

```powershell
py -3.12 -m mkdocs gh-deploy --force
```

第一次執行時，MkDocs 會自動幫你建立 `gh-pages` 分支。  
然後到 GitHub 專案的：

- `Settings → Pages`
  - **Source**：Deploy from a branch  
  - **Branch**：`gh-pages` / `(root)`

設定完成後，網站就會出現在：

- `https://<你的使用者名稱>.github.io/<你的 Repo 名稱>/`

在這個專案裡，就是：

- `https://kuankuantan.github.io/blog/`

之後每次更新內容，只要再跑一次 `gh-deploy` 就會重新發布。

---

## MkDocs 設定檔簡介

專案根目錄的 `mkdocs.yml` 是整個網站的大腦。  
這個部落格的設定包含：

- `site_name`：網站名稱
- `theme`：使用 `material` 主題，並設定語言、顏色、功能等
- `nav`：定義上方／側邊導航列要顯示哪些頁面

例如（節錄）：

```yaml
site_name: My Blog
theme:
  name: material
  language: zh-TW

nav:
  - 首頁: index.md
  - 文章:
    - 文章列表: posts/index.md
    - MkDocs 入門：用簡單方式打造文件與部落格: posts/mkdocs-intro.md
  - 關於: about.md
```

只要在 `nav` 裡加上一行，新的文章就會自動出現在導航列中。

---

## 寫文章的建議流程

1. 在 `docs/posts/` 底下新增一個 `.md` 檔  
2. 在 `docs/posts/index.md` 加入該文章的連結  
3. 視需要在 `mkdocs.yml` 的 `nav` 裡也加一條  
4. 本機用 `mkdocs serve` 預覽  
5. 沒問題後：

   ```powershell
   git add .
   git commit -m "新增文章：MkDocs 入門"
   git push
   py -3.12 -m mkdocs gh-deploy --force
   ```

---

## 結語

MkDocs 搭配 Material 主題，讓「寫文件」變得像「寫筆記」一樣簡單，  
卻又能輸出設計感很好的網站，非常適合作為技術文件、學習紀錄和個人部落格的基礎。

之後你可以：

- 多善用標題、目錄與程式碼區塊整理內容  
- 把常用的開發指令、筆記、教學都寫進來  
- 透過 GitHub Pages 免費公開給全世界閱讀

