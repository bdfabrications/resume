# Brandon DeWinne — Digital Business Card

Fast, mobile-first landing page for a QR code on a physical business card.
Fully static. No build step. No backend.

**Stack:** plain HTML + CSS  
**Repo:** https://github.com/bdfabrications/resume  
**Host:** Nginx in Docker + Cloudflare Tunnel on ZimaBlade (same pattern as scprivateer.org)

---

## Repo contents (what gets deployed)

```
index.html              ← digital card (QR destination)
resume/index.html       ← full résumé
assets/css/main.css     ← shared styles
docker-compose.yml      ← nginx:alpine on host port 4322
nginx.conf              ← container Nginx config
README.md
.gitignore
```

Local-only (gitignored — stay on your PC): Cursor prompt/rules, roadmap, QR PNG.

Edit copy in the HTML files. Colors/fonts: `:root` in `assets/css/main.css`.

---

## Preview locally

```bash
python -m http.server 8080
# or: docker compose up -d
```

- Python: `http://localhost:8080`
- Docker: `http://localhost:4322`

---

## Deploy on ZimaBlade

Same idea as scprivateer.org: `nginx:alpine` → host port → mount static files.

| Site | Container | Host port | Mount |
|------|-----------|-----------|--------|
| scprivateer.org | `blackbox_storefront` | `4321` | `./dist` (Astro build) |
| this card | `brandon_card` | `4322` | project root (no build) |

### 1. Clone or pull

```bash
git clone https://github.com/bdfabrications/resume.git landing
cd landing
```

Later updates:

```bash
cd landing
git pull
docker compose restart   # only if the page looks stale
```

### 2. Start the container

```bash
docker compose up -d
docker ps | grep brandon_card
```

LAN check: `http://<zimablade-ip>:4322/` and `.../resume/`

### 3. Cloudflare Tunnel

Add a Public Hostname on the same tunnel as scprivateer.org:

- **Hostname:** e.g. `resume.crustyserver.us`
- **Service:** HTTP → `localhost:4322`

Cloudflare terminates HTTPS.

### 4. QR code

Generate after the public HTTPS URL works on cellular. Point the QR at the site root. Keep the PNG out of git (print asset only).

---

## Content you’ll likely edit

| What | Where |
|------|--------|
| Phone / email | `index.html` + `resume/index.html` |
| Alaska / availability | `.identity__location` in `index.html` |
| Skills | Capabilities section in `index.html` |
| Work history | `resume/index.html` |
| Colors | `:root` in `assets/css/main.css` |

---

## Notes

- Dark charcoal + copper theme, no JavaScript
- Fonts from Google Fonts (can self-host later)
- Optional later: headshot, PDF résumé, analytics
