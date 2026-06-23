# Hosting civicresilience.net Overlay — DNS + Deploy Guide

## Architecture

```
OBS Browser Source
    └── https://overlay.civicresilience.net
            └── GitHub Pages (free) or Netlify (free)
                    └── civic-overlay/index.html
                            └── reads scenes.json (update = instant live change)
```

---

## Option A — GitHub Pages (Recommended, Free)

### 1. Create the repo
1. Go to github.com → New repository
2. Name it: `civic-overlay`
3. Set to **Public** (required for free Pages)
4. Upload: `index.html`, `scenes.json`, `img/` folder

### 2. Enable GitHub Pages
1. Repo → Settings → Pages
2. Source: **Deploy from a branch** → `main` → `/ (root)`
3. Save — site goes live at: `https://yourusername.github.io/civic-overlay`

### 3. Add custom domain
1. Repo → Settings → Pages → Custom domain
2. Enter: `overlay.civicresilience.net`
3. Check "Enforce HTTPS"

### 4. DNS record (at your domain registrar)
Go to your DNS settings for civicresilience.net and add:

```
Type   Host                         Value
────   ───────────────────────────  ──────────────────────────────
CNAME  overlay.civicresilience.net  yourusername.github.io
```

⏱ DNS propagation: 5 min – 2 hours

---

## Option B — Netlify (Free, Instant Deploy)

### 1. Deploy
1. Go to netlify.com → Add new site → Deploy manually
2. Drag your `civic-overlay/` folder into the deploy box
3. Site goes live immediately at a random URL

### 2. Add custom domain
1. Site settings → Domain management → Add custom domain
2. Enter: `overlay.civicresilience.net`

### 3. DNS record
```
Type   Host                         Value
────   ───────────────────────────  ─────────────────────────────
CNAME  overlay.civicresilience.net  your-site-name.netlify.app
```

---

## OBS Browser Source Setup

1. In OBS → Sources → + → Browser Source
2. Settings:
   ```
   URL:    https://overlay.civicresilience.net
   Width:  1920
   Height: 1080
   ✅ Shutdown source when not visible
   ✅ Refresh browser when scene becomes active
   Custom CSS: body { background: transparent !important; }
   ```
3. Set source to **always on top** in your scene stack

---

## Updating Scenes Live (without restarting OBS)

Edit `scenes.json` on GitHub → commit → overlay auto-refreshes in ~30 seconds
(the overlay polls for config changes every 30s).

For **instant** updates: right-click Browser Source in OBS → Refresh.

---

## Keyboard Controls (when overlay has focus)

| Key        | Action                         |
|------------|-------------------------------|
| →  / Space | Next scene                     |
| ←          | Previous scene                 |
| 1–9        | Jump to scene by number        |
| L          | Show lower third queue         |
| R          | Hot-reload scenes.json         |

## URL Hash Controls (remote trigger)

```
# Jump to scene 3:
https://overlay.civicresilience.net#scene=3

# Show lower third:
https://overlay.civicresilience.net#lt=James%20Flanagano|Civic%20Resilience%20Network

# Reload config:
https://overlay.civicresilience.net#reload=1
```

---

## Civicresilience.net Main Domain

To point the root domain to a landing page or newsletter:

```
Type  Host                  Value
────  ────────────────────  ──────────────────────────────
A     civicresilience.net   185.199.108.153   (GitHub Pages IP)
A     civicresilience.net   185.199.109.153
A     civicresilience.net   185.199.110.153
A     civicresilience.net   185.199.111.153
CNAME www.civicresilience.net  yourusername.github.io
```

Or for a Substack redirect:
```
CNAME  www.civicresilience.net  custom.substack.com
```
(Then set the custom domain in Substack settings)
