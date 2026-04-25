# HSE Pulse - Pitch site

Two static files. Zero build step. Deployed via GitHub Pages, fronting `hse.harishankar.info`.

```
pitch/
├── index.html    # Single-page sales pitch (SPA)
├── deck.html     # Reveal.js 14-slide presentation
├── CNAME         # tells GitHub Pages to serve at hse.harishankar.info
├── .nojekyll     # tells GitHub Pages to skip Jekyll processing
└── README.md     # this file
```

All HTML loads its dependencies (Tailwind, Reveal.js, Inter / JetBrains Mono fonts) from
public CDNs - no `npm install`, no bundling, nothing to compile.

---

## Local preview

```bash
# from D:\project-demo\cancer\pitch
python -m http.server 8000        # → http://localhost:8000
```

---

## Deploy - three commands + one DNS record

### 1. Create an empty public repo on GitHub

Recommended: **`HSE-Pulse/pitch`** (matches the org the rest of the platform lives in).
Alternative: `HarishankarSomasundaram/hse-pulse-pitch` (personal portfolio).

**Don't initialize it with README / .gitignore / license.** Empty.

### 2. Push from this directory

```bash
# from D:\project-demo\cancer\pitch
git remote add origin https://github.com/HSE-Pulse/pitch.git
git push -u origin main
```

### 3. Enable GitHub Pages

In the new repo's UI: **Settings → Pages** → Source: **Deploy from a branch** →
Branch: **main** → Folder: **/ (root)** → Save.

GitHub will tell you "Your site is live at https://hse.harishankar.info/" once DNS is in place.

### 4. Add the DNS record

In your DNS panel for `harishankar.info`:

| Type   | Host  | Value                              | TTL  |
|--------|-------|------------------------------------|------|
| CNAME  | `hse` | `hse-pulse.github.io.`             | 3600 |

(or `harishankarsomasundaram.github.io.` if you used the personal-account repo. Trailing dot
matters on most DNS providers.)

HTTPS cert is auto-issued by GitHub via Let's Encrypt within a few minutes of DNS propagation.

---

## Updating the pitch

Edit `index.html` or `deck.html` directly (no build step). Then:

```bash
git add . && git commit -m "Update pitch" && git push
```

GitHub Pages picks up the change in ~30 seconds.

Numbers in the SPA come from `docs/BRD.md` Section 11 (Business Impact and Value to
Ireland) and `docs/irish_hospital_ai_strategy_report.md`. If those change, mirror the
changes here.

---

## What I cannot do for you

The repo creation on GitHub and the DNS record on your registrar both require credentials
I don't have. Both are 2-minute steps once you're at the keyboard.
