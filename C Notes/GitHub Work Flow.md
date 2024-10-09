# GitHub

# 五人團隊使用GitHub協作的完整指南

## 1. 項目初始化

### 1.1 創建GitHub倉庫

- 團隊負責人登錄GitHub，點擊右上角的"+"，選擇"New repository"
- 填寫倉庫名稱，例如"team-online-shop"
- 選擇公開或私有
- 初始化時添加README文件
- 點擊"Create repository"

### 1.2 設置分支保護規則

- 在倉庫頁面，點擊"Settings" > "Branches"
- 在"Branch protection rules"下，點擊"Add rule"
- 在"Branch name pattern"中輸入"main"
- 選擇"Require pull request reviews before merging"
- 選擇"Require status checks to pass before merging"
- 保存規則

### 1.3 創建develop分支

```bash
git clone <https://github.com/your-username/team-online-shop.git>
cd team-online-shop
git checkout -b develop
git push -u origin develop

```

## 2. 團隊成員設置

每個團隊成員都應該執行以下步驟：

### 2.1 克隆倉庫

```bash
git clone <https://github.com/your-username/team-online-shop.git>
cd team-online-shop

```

### 2.2 設置Git配置

```bash
git config user.name "Your Name"
git config user.email "your.email@example.com"

```

## 3. 開發工作流程

### 3.1 創建功能分支

每個新功能都應該在一個單獨的分支上開發：

```bash
git checkout develop
git pull origin develop
git checkout -b feature/user-authentication

```

### 3.2 開發和提交

在功能分支上進行開發，並定期提交更改：

```bash
# 做一些更改
git add .
git commit -m "實現用戶註冊功能"

```

### 3.3 推送分支到GitHub

```bash
git push -u origin feature/user-authentication

```

### 3.4 創建Pull Request

- 在GitHub上，切換到feature/user-authentication分支
- 進行各自功能的開發（不同功能）
- 點擊"Compare & pull request"
- 填寫PR的標題和描述
- 選擇將分支合併到develop
- 點擊"Create pull request"

### 3.5 代碼審查

- 其他團隊成員審查代碼
- 在PR頁面上提供評論和建議
- 開發者根據反饋進行修改

### 3.6 合併Pull Request

- 當PR獲得批准後，點擊"Merge pull request"
- 選擇"Squash and merge"選項
- 確認合併

## 4. 同步和更新

### 4.1 更新本地develop分支

```bash
git checkout develop
git pull origin develop

```

### 4.2 刪除已合併的功能分支

```bash
git branch -d feature/user-authentication

```

## 5. 發布版本

### 5.1 準備發布

- 確保所有計劃的功能都已合併到develop分支
- 在develop分支上進行最後的測試和bug修復

### 5.2 合併到main分支

```bash
git checkout main
git merge develop

```

### 5.3 創建版本標籤

```bash
git tag -a v1.0.0 -m "版本 1.0.0 - 初始發布，包含用戶認證、商品目錄和購物車功能"

```

### 5.4 推送更改和標籤

```bash
git push origin main
git push origin v1.0.0

```

### 5.5 在GitHub上創建Release

- 在GitHub倉庫頁面，點擊"Releases"
- 點擊"Draft a new release"
- 選擇剛才創建的標籤
- 填寫發布說明
- 點擊"Publish release"

## 6. 最佳實踐

- 經常從develop分支更新你的功能分支
- 寫清晰、描述性強的提交消息
- 保持PR小而精準
- 使用有意義的分支名稱，如feature/，bugfix/，hotfix/等
- 定期進行程式碼審查，培養團隊協作文化
- 使用issues跟踪任務和bug
- 保持文檔更新，包括README和其他項目文檔

通過遵循這個工作流程，你的五人團隊可以有效地使用GitHub進行協作，確保代碼質量，並順利管理項目的開發和發布過程。