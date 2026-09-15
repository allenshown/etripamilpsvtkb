# PUSH-GUIDE · 把本地知识库推送到 GitHub

> 让家里 HTML 页面能实时拉取最新 prompt。

## 目标

    公司电脑 experts/*.md
       ↓ git push
    GitHub 私有仓（云端唯一真相源）
       ↓ raw.githubusercontent.com
    家里电脑 HTML 页面（自动拉取）

## 一次性配置

### 1. 注册 GitHub 账号

打开 https://github.com 注册。

### 2. 创建私有仓库

1. 打开 https://github.com/new
2. Repository name: etripamil-psvt-kb
3. 选 Private（私有）
4. 不要勾选 Initialize with README
5. 点 Create repository

### 3. 在公司电脑配置 git

打开 PowerShell，依次执行：

    git config --global user.name "Your Name"
    git config --global user.email "your@email.com"
    cd "C:\Users\ying.xiong\Documents\ChatGPT\etripamil-psvt-kb"
    git init
    git add .
    git commit -m "v1.3: 艾曲帕米专家团初始提交"
    git branch -M main
    git remote add origin https://github.com/你的用户名/etripamil-psvt-kb.git
    git push -u origin main

### 4. 配置家里 HTML 的 GITHUB_RAW_URL

打开家里电脑的 sync-page.html，找到：

    const GITHUB_RAW_URL = 'https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/experts/psvt-clinical-advisor.md';

改成你的 URL：

    const GITHUB_RAW_URL = 'https://raw.githubusercontent.com/你的用户名/etripamil-psvt-kb/main/experts/psvt-clinical-advisor.md';

## 日常更新流程

### 公司电脑：修改 → 推送

    cd "C:\Users\ying.xiong\Documents\ChatGPT\etripamil-psvt-kb"
    git add .
    git commit -m "v1.4: 增量说明"
    git push

### 家里电脑：自动拉取

打开 sync-page.html → 看到「已从 GitHub 拉到最新版本」→ 完事。

## 高级：自动推送（可选）

在知识库目录创建 auto-push.ps1：

    $ErrorActionPreference = "Stop"
    Set-Location $PSScriptRoot
    git add .
    $status = git status --porcelain
    if ($status) {
        git commit -m "auto: $(Get-Date -Format "yyyy-MM-dd HH:mm")"
        git push
    }

## 故障排查

| 问题 | 解决 |
|---|---|
| 家里 HTML 显示拉取失败 | 检查 URL 是否正确；网络是否能访问 raw.githubusercontent.com |
| 推送时报 permission denied | 确认 git remote URL 正确；或配置 Personal Access Token |

## Personal Access Token（避免密码输入）

1. GitHub → Settings → Developer settings → Personal access tokens
2. 勾选 repo 权限，生成 token
3. 推送时用 token 代替密码

或用 SSH key（更安全）：

    ssh-keygen -t ed25519
    # 复制公钥到 GitHub
    git remote set-url origin git@github.com:你的用户名/etripamil-psvt-kb.git

## 推送清单

- [ ] GitHub 仓库创建（Private）
- [ ] 公司电脑 git 配置 + 首次 push
- [ ] 家里 HTML 修改 GITHUB_RAW_URL
- [ ] 打开 HTML 验证「已从 GitHub 拉到最新版本」
- [ ] （可选）配置自动推送任务