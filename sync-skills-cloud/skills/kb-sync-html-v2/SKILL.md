---
name: kb-sync-html-v2
description: Generate a self-syncing HTML page that mirrors a GitHub-hosted knowledge base via HTTPS (not file://). Use when the user has a knowledge base on GitHub and wants a portable web page that always shows the latest version, accessible from any device, that auto-copies to clipboard for use in any AI chat. Inputs: GitHub URL of the markdown file, optional title. Outputs: a self-syncing HTML file deployable to GitHub Pages.
---

# KB Sync HTML v2 (GitHub Pages Edition)

Self-syncing HTML page that fetches from GitHub via HTTPS — no file:// limitations.

## Why v2 (vs v1)

| v1 (file://) | v2 (GitHub Pages) |
|---|---|
| Only works locally | Works on any device |
| Browser blocks fetch | HTTPS allowed |
| CORS errors | No CORS issues |
| Cache issues | Reliable |
| Limited sharing | Shareable URL |

## When to use

- User has markdown files on GitHub
- Wants a portable HTML that always shows latest
- Wants to use content with any AI (DeepSeek, Kimi, etc.)
- Doesn't want to install or run a server

## Architecture

```
[GitHub Repo (private or public)]
  └─ experts/psvt-clinical-advisor.md
       ↓ (raw URL, no auth needed if public)
[GitHub Pages serves sync-page.html]
  sync-page.html fetches: raw.githubusercontent.com/...
       ↓ (HTTPS to HTTPS = no CORS)
[Any browser anywhere]
  Shows latest content + Copy button
```

## What the HTML does

1. On page load: `fetch()` from raw.githubusercontent.com
2. Display content in code block
3. Save to localStorage (cache for offline)
4. Show status: ✅ Live / ⚠️ Cached / ❌ Error
5. Auto-refresh every 30 min
6. Copy to clipboard button
7. "Copy + Open DeepSeek/Kimi" buttons for one-click workflow

## How to create one

### Step 1: Generate HTML

In Codex:
```
Generate a sync HTML page for my KB at:
https://raw.githubusercontent.com/USERNAME/REPO/main/experts/my-prompt.md
Title: My KB Sync
```

### Step 2: Save as `sync-page.html` in your GitHub repo

In your KB repo (the one containing the markdown), add a new file `sync-page.html` with the generated content.

### Step 3: Enable GitHub Pages

Repo → Settings → Pages → Source: main → Save

### Step 4: Use

Open: `https://USERNAME.github.io/REPO/sync-page.html`

## Configuration constants (in the HTML)

```javascript
const GITHUB_RAW_URL = 'https://raw.githubusercontent.com/USERNAME/REPO/main/path/to/file.md';
const STORAGE_KEY = 'unique_cache_key';
const REFRESH_INTERVAL_MIN = 30;  // auto-refresh interval
```

## Critical: file path is exact

If your file is at `experts/psvt-advisor.md` in the repo, the URL is:
```
https://raw.githubusercontent.com/USERNAME/REPO/main/experts/psvt-advisor.md
```

(NOT `main/file.md` — must include the full path)

## Updating the KB

```
1. Edit file in repo (web UI or git push)
2. Wait 1-2 min for GitHub Pages to redeploy
3. Hard refresh sync-page.html (Ctrl+Shift+R)
4. See latest content
```

## Common pitfalls

| Issue | Cause | Fix |
|---|---|---|
| 404 on raw URL | Repo is Private | Make Public OR add GitHub PAT |
| ERR_CONNECTION_RESET | Network firewall | Try on different network |
| Page shows old content | Browser cache | Hard refresh / use incognito |
| CORS error in console | Mixed content or file:// | Use GitHub Pages URL (https), not file:// |
| Auto-refresh not working | Tab in background | Browsers throttle background tabs |

## Security note

If you make the repo Public, anyone with the URL can read the content. For sensitive data, use GitHub PAT (more complex) or keep repo Private + use a service that supports auth.

## Compared to alternatives

| Approach | Cost | Setup | Auto-update | Works everywhere |
|---|---|---|---|---|
| **GitHub Pages (this)** | Free | Easy | ✅ | ✅ |
| Vercel | Free | Medium | ✅ | ✅ |
| Netlify | Free | Medium | ✅ | ✅ |
| Self-hosted (Raspberry Pi) | $35 + always-on | Hard | ✅ | ✅ (LAN only) |
| Cloudflare Pages | Free | Medium | ✅ | ✅ |
| Dropbox/OneDrive direct link | Free | Easy | Manual upload | Limited |
