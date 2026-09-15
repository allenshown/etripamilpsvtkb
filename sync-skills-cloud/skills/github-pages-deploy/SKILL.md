---
name: github-pages-deploy
description: Deploy a static HTML page to GitHub Pages so it can be accessed via HTTPS URL (avoiding file:// browser restrictions). Use when the user has a static HTML file that needs to be served from a public URL, or wants to bypass local-file browser fetch limitations. Inputs: the HTML file path. Outputs: a working GitHub Pages URL.
---

# GitHub Pages Static Deploy

Deploy a static HTML file to GitHub Pages for HTTPS-based access.

## When to use

- User has a static HTML file (e.g., `sync-page.html`)
- Wants to access it via `https://...` URL (not `file://`)
- Needs to bypass browser file:// security policies
- Wants a free, simple hosting solution

## Why GitHub Pages

- ✅ Free for public repos
- ✅ Auto-deploys on git push
- ✅ HTTPS by default (no SSL setup)
- ✅ Custom domain support
- ✅ Reliable CDN

## How to enable

### Step 1: Have a GitHub repo

If you don't have one, create at https://github.com/new

### Step 2: Upload your HTML file

In the repo, click "Add file" → "Upload files" → drag your HTML

### Step 3: Enable Pages

1. Repo → **Settings** → **Pages** (left sidebar)
2. **Source**: "Deploy from a branch"
3. **Branch**: `main`
4. **Folder**: `/ (root)`
5. Click **Save**

### Step 4: Wait 1-2 minutes

GitHub builds and deploys. You'll see:
```
Your site is live at https://USERNAME.github.io/REPO/
```

### Step 5: Access your HTML

```
https://USERNAME.github.io/REPO/your-file.html
```

## Common pitfalls

| Problem | Solution |
|---|---|
| 404 on the URL | Wait 2 more minutes (deployment delay) |
| URL shows README | Add `.html` to filename: `sync-page.html` |
| Changes don't appear | Hard refresh (Ctrl+Shift+R); check Actions tab for build status |
| "There isn't a GitHub Pages site here" | Verify branch + folder in Settings → Pages |

## Custom domains (advanced)

1. Buy a domain (e.g., from Namecheap)
2. Repo → Settings → Pages → Custom domain
3. Add `CNAME` record pointing to `USERNAME.github.io`
4. Enable "Enforce HTTPS"

## When NOT to use

- Private/sensitive content (use Cloudflare Pages with access control instead)
- Dynamic server-side logic (use Vercel/Netlify/Functions)
- High-traffic production apps (use Cloudflare Pages or Vercel)
