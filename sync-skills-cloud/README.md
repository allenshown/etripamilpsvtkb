# Sync Skills Cloud

3 个 Codex Skills，教你**如何把内容同步到任何设备**。

## 🎯 这里有什么

```
sync-skills-cloud/
├── skills/
│   ├── cross-device-sync/          ← Skill 1: 整体方法论
│   ├── github-pages-deploy/       ← Skill 2: 静态部署
│   └── kb-sync-html-v2/           ← Skill 3: 升级版同步页
├── examples/
│   └── psvt-setup/                ← 你的实际案例 (PSVT)
│       └── html/                   ← sync-page.html 实例
└── README.md                      ← 本文件
```

## 🚀 快速上手（PSVT 案例）

1. 创建 GitHub 仓库 `etripamilpsvtkb`
2. 上传 `experts/*.md`
3. 启用 GitHub Pages
4. 上传 `sync-page.html`（已带正确 URL）
5. Edge 打开 `https://USERNAME.github.io/REPO/sync-page.html`
6. ✅ 实时拉取 + 复制 + 粘到任何 AI

## 📋 适用场景

- LLM prompts / 专家 prompt
- 团队 wiki
- 个人笔记
- 代码片段
- 任何需要"实时同步 + 任何设备"的文本

## 💡 核心洞察

**单点真相源** = 1 个 GitHub 仓库  
**分发** = GitHub Pages（自动 HTTPS）  
**消费** = 任何浏览器、任何设备  

→ 零服务器、零配置、自动同步。

## 🔄 升级

每次想加新内容：
1. 改文件（公司电脑本地或 GitHub web）
2. git push 或 web 直接拖
3. GitHub Pages 自动重部署
4. 家里/任何设备刷新就看到

## 📦 使用 Skills

每个 skill 都是 **Codex SKILL.md** 格式，可装到任何 Codex：
- `~/.codex/skills/.system/` (个人永久)
- `~/.agents/skills/` (个人永久)

## 📄 许可

内部使用。v1.0 (2026-09-15)。
