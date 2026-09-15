---
name: cross-device-sync
description: Sync any text-based knowledge base between two or more computers (e.g., work + home) using GitHub as the single source of truth. Use when the user has content (markdown files, prompts, docs) that needs to be available on multiple devices, or wants "real-time sync" without running their own server. Outputs: a complete setup including GitHub repo, GitHub Pages URL, and an auto-syncing HTML page.
---

# Cross-Device Knowledge Base Sync

Make any text-based content (markdown prompts, docs, configs) available on multiple computers with **automatic sync from GitHub**.

## When to use

- User has files on one computer that need to be on another
- User has tried cloud sync (OneDrive/Dropbox) but wants version control
- User wants to access content from any device with a browser
- User needs "real-time" sync (refresh page → see latest)

## Architecture

```
[Source Computer]
  Knowledge base (markdown files)
       ↓ git push
  GitHub Repo (Private, single source of truth)
       ↓ Auto-deploy
  GitHub Pages (Public access via HTTPS)
       ↓ Browser opens URL
  [Any Computer]  ← Real-time, works on phone/tablet/laptop
```

## What this skill produces

1. **Private GitHub repo** (e.g., `username/kb-repo`)
2. **Public** repo (for GitHub Pages) OR use repo's main branch via Pages
3. **GitHub Pages enabled** on the repo
4. **Auto-syncing HTML page** (in repo) that:
 - Fetches latest content from raw.githubusercontent.com
 - Caches in localStorage
 - Has Copy button for easy AI use
 - Works in any browser, no file:// restrictions
5. **Update workflow**: edit → push → refresh → latest

## Step-by-step

### Step 1: Create GitHub repo

1. Go to https://github.com/new
2. Repository name: `your-kb-name`
3. **Private** (initially) or **Public** (if not sensitive)
4. Create

### Step 2: Upload files via web

1. In the new repo, click "uploading an existing file"
2. Drag files (markdown prompts, etc.)
3. Commit

### Step 3: Enable GitHub Pages

1. Repo → Settings → Pages
2. Source: Deploy from a branch → main
3. Folder: / (root)
4. Save
5. Wait 1-2 minutes for site to deploy

### Step 4: Get your URLs

| URL | Purpose |
|---|---|
| `https://github.com/USER/REPO` | Edit/upload files |
| `https://raw.githubusercontent.com/USER/REPO/main/path/to/file.md` | Raw content for fetch |
| `https://USER.github.io/REPO/sync-page.html` | Your auto-syncing HTML |

### Step 5: Use on any device

Open `https://USER.github.io/REPO/sync-page.html` in any browser:
- ✅ Works on Chrome/Edge/Firefox/Safari
- ✅ No file:// restrictions
- ✅ HTTPS to HTTPS fetch works
- ✅ localStorage caching for offline

## Why GitHub Pages > file://

| Approach | Pros | Cons |
|---|---|---|
| `file://` HTML | Simple, no hosting | Browser blocks fetch (CORS-like) |
| **GitHub Pages** | HTTPS, no restrictions, free | Repo must be public (for free Pages) |
| Self-hosted server | Full control | Requires running server |
| Cloudflare Pages | Free, fast, private | More setup |

GitHub Pages is the sweet spot for personal use.

## Update workflow

```
[Edit markdown files on source computer]
       ↓
[git push OR drag-drop new files on GitHub web]
       ↓
[GitHub Pages auto-deploys (1-2 min)]
       ↓
[Open https://USER.github.io/REPO/sync-page.html on any device]
       ↓
[Hard refresh: Ctrl+Shift+R]
       ↓
[See latest content! ✅]
```

## Common pitfalls (and solutions)

| Issue | Cause | Fix |
|---|---|---|
| 404 on raw URL | Repo is Private | Make repo Public, OR use GitHub PAT in HTML |
| 404 despite correct URL | Browser cached old 404 | Hard refresh (Ctrl+Shift+R) or use incognito |
| `ERR_CONNECTION_RESET` | Network firewall | Try on different network (e.g., home vs work) |
| `Unsafe attempt to load URL` | file:// security policy | Use GitHub Pages, not local file:// |
| Page shows old content | localStorage cache | Clear site data, or use fresh URL param |

## Privacy considerations

- **Public repo**: Anyone with the URL can read. But:
 - URL is hard to guess (username + repo name + path)
 - Not indexed by search engines
 - Adequate for most personal knowledge
- **Private repo**: Requires GitHub PAT in HTML for raw access
 - More complex setup
 - Use this for truly sensitive content
- **Compromise**: Make repo Public, but use obscure names

## Reusability

This same pattern works for ANY text-based content:
- LLM prompts (your case!)
- Code snippets
- Documentation
- Cheat sheets
- Personal wiki
- Configuration templates
