# Portfolio site — setup

Plain HTML and CSS. No build step, no npm, no framework. Edit a file, save, refresh the browser.

## Files

```
index.html      home
projects.html   project writeups
writing.html    blog index
style.css       all styling — edit the :root block at the top to change colors/fonts
resume.pdf      drop your resume PDF here (this exact filename)
img/            architecture diagrams go here
posts/          one HTML file per blog post
```

## Before you deploy — find and replace

Search across all three HTML files for these and replace:

| Placeholder | Replace with |
|---|---|
| `YOUR NAME` | your name |
| `YOURUSERNAME` | your GitHub username |
| `YOURPROFILE` | your LinkedIn profile slug |
| `YOU@YOURDOMAIN` | your email |
| `YOURDOMAIN` | your domain, e.g. `yourname.dev` |
| `TODO` | the tradeoffs sections on projects.html — write these, don't ship them empty |

Also drop `resume.pdf` in the root folder.

## Preview locally

Double-click `index.html`. It opens in your browser. That's it.

## Deploy — GitHub Pages

```bash
cd portfolio
git init
git add .
git commit -m "portfolio site"
git branch -M main
git remote add origin https://github.com/YOURUSERNAME/portfolio.git
git push -u origin main
```

Then on github.com, in the repo: **Settings → Pages → Source: Deploy from a branch → main → / (root) → Save**.

About 60 seconds later it's live at `https://YOURUSERNAME.github.io/portfolio`.

## Point your domain at it

At your domain registrar's DNS settings, add five records:

```
A      @      185.199.108.153
A      @      185.199.109.153
A      @      185.199.110.153
A      @      185.199.111.153
CNAME  www    YOURUSERNAME.github.io
```

Then back in GitHub: **Settings → Pages → Custom domain →** enter your domain → Save. Once the
certificate is issued (minutes to an hour), tick **Enforce HTTPS**.

DNS takes anywhere from 10 minutes to a few hours to propagate.

## Updating

```bash
git add .
git commit -m "add writeup"
git push
```

Live in ~30 seconds.

## Diagrams

Use [Excalidraw](https://excalidraw.com) — free, no account needed. Draw the flow, export as PNG,
save into `img/` with the filenames already referenced in `projects.html`:

- `img/risk-pipeline-architecture.png`
- `img/redteam-injection-path.png`
- `img/securelab-architecture.png`

The diagram is the single highest-value element on a project page. Do these before you polish copy.

## Adding a blog post

1. Copy `writing.html` to `posts/your-post-name.html`
2. Change the `<link rel="stylesheet" href="style.css">` to `href="../style.css"`, and fix the nav
   links to point up one level (`../index.html`, etc.)
3. Replace the `<main>` contents with your article
4. Link it from `writing.html`

## Optional: security headers

GitHub Pages can't set custom response headers. If you want an A+ on
[securityheaders.com](https://securityheaders.com) — a reasonable thing to have as a security
candidate, and a decent interview talking point — deploy to **Cloudflare Pages** instead. Same
process (connect the GitHub repo, it auto-deploys), but you can add a `_headers` file:

```
/*
  Content-Security-Policy: default-src 'self'; style-src 'self' https://fonts.googleapis.com; font-src https://fonts.gstatic.com; img-src 'self' data:; script-src 'none'; frame-ancestors 'none'; base-uri 'self'
  Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
  X-Content-Type-Options: nosniff
  X-Frame-Options: DENY
  Referrer-Policy: strict-origin-when-cross-origin
  Permissions-Policy: geolocation=(), microphone=(), camera=()
```

Don't let this block launch. Ship on GitHub Pages first, move later if you want.

## Optional: self-host the fonts

The pages currently load three families from Google Fonts, which sends visitor IPs to Google.
If that bothers you (it's a fair thing for a security portfolio to care about, and a nice detail
to mention in an interview), download the woff2 files, drop them in `fonts/`, replace the
`<link>` tags with `@font-face` rules in `style.css`, and tighten the CSP accordingly.
