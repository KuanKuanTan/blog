# Git 命令指南

這個文件包含管理此部落格專案所需的 Git 命令。

## 🎯 初始設置

### 1. 加入所有檔案並進行首次提交

```bash
# 加入 .gitignore
git add .gitignore

# 加入所有檔案
git add .

# 進行首次提交
git commit -m "Initial commit: 建立個人部落格"

# 查看提交歷史
git log --oneline
```

## 📤 推送到 GitHub

### 2. 設置遠端儲存庫（如果還沒有）

```bash
# 在 GitHub 上創建新儲存庫後，添加遠端
git remote add origin https://github.com/yourusername/blog.git

# 驗證遠端設定
git remote -v
```

### 3. 推送到 GitHub

```bash
# 推送到 main 分支
git push -u origin main

# 之後的推送只需要
git push
```

## 🔄 日常使用命令

### 查看狀態

```bash
# 查看檔案狀態
git status

# 簡潔狀態
git status --short
```

### 加入和提交變更

```bash
# 加入特定檔案
git add docs/posts/new-article.md

# 加入所有變更
git add .

# 提交變更
git commit -m "新增文章：文章標題"

# 加入並提交（跳過暫存區）
git commit -am "更新內容"
```

### 查看變更

```bash
# 查看未暫存的變更
git diff

# 查看已暫存的變更
git diff --staged

# 查看提交歷史
git log --oneline --graph
```

### 分支操作

```bash
# 創建新分支
git checkout -b feature/new-feature

# 切換分支
git checkout main

# 合併分支
git merge feature/new-feature

# 刪除分支
git branch -d feature/new-feature
```

## 🚀 部署流程

### 標準部署流程

```bash
# 1. 確保所有變更已提交
git status

# 2. 加入變更
git add .

# 3. 提交變更
git commit -m "描述你的變更"

# 4. 推送到 GitHub（會觸發自動部署）
git push origin main
```

### 查看部署狀態

1. 前往 GitHub 儲存庫
2. 點擊 "Actions" 標籤
3. 查看最新的 workflow 執行狀態

## 🔧 常用命令速查

```bash
# 初始化 Git（如果還沒初始化）
git init

# 設置使用者資訊（首次使用時）
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# 查看配置
git config --list

# 撤銷未暫存的變更
git checkout -- <file>

# 撤銷已暫存的變更
git reset HEAD <file>

# 修改最後一次提交訊息
git commit --amend -m "新的提交訊息"

# 查看遠端資訊
git remote show origin
```

## 📋 提交訊息規範

建議使用清晰的提交訊息：

```
feat: 新增文章：Python 基礎教學
fix: 修正首頁連結錯誤
docs: 更新 README
style: 調整文章格式
refactor: 重構導航結構
```

## ⚠️ 注意事項

- 推送到 `main` 或 `master` 分支會自動觸發部署
- 確保 `mkdocs.yml` 格式正確，否則建置會失敗
- 建置產生的 `site/` 目錄已在 `.gitignore` 中，不需要提交
