# HSE Pulse — Pitch site

Two static files. Zero build step. Works on any HTTP host.

```
pitch/
├── index.html   # Single-page sales pitch (SPA)
├── deck.html    # Reveal.js 14-slide presentation
└── README.md    # this file
```

Both files load all dependencies (Tailwind, Reveal.js, Inter / JetBrains Mono fonts) from
public CDNs — no `npm install`, no bundling, nothing to compile. Open `index.html` in a
browser to verify locally before deploying.

---

## Local preview

Pick whichever you have:

```bash
# Python (already installed)
python -m http.server -d D:/project-demo/cancer/pitch 8000
# → http://localhost:8000

# Node
npx serve D:/project-demo/cancer/pitch

# Or just double-click index.html in Explorer
```

---

## Deploy to a subdomain of `harishankar.info`

Pick the path that matches your existing setup. **All four of these are zero-cost.**

### Option A — GitHub Pages (recommended if `harishankar.info` already lives on GitHub Pages)

```bash
# From the project root
git init -b main pitch_deploy           # or use an existing repo
cp -r pitch/* pitch_deploy/
cd pitch_deploy
echo "hse.harishankar.info" > CNAME    # subdomain you want
git add . && git commit -m "Initial pitch site"
git remote add origin git@github.com:HarishankarSomasundaram/<repo>.git
git push -u origin main
```

Then in your DNS panel for `harishankar.info`:

```
hse.harishankar.info  CNAME  HarishankarSomasundaram.github.io.
```

In the GitHub repo: Settings → Pages → Source = main branch, root. SSL is automatic.

### Option B — Vercel

```bash
cd pitch
npx vercel                    # follow the prompts; framework: "Other"
npx vercel --prod
# Then in Vercel dashboard: Settings → Domains → add hse.harishankar.info
```

DNS:
```
hse.harishankar.info  CNAME  cname.vercel-dns.com.
```

### Option C — Netlify (drag-and-drop, no CLI)

1. Open https://app.netlify.com/drop
2. Drag the `pitch/` folder onto the page
3. Site → Domain settings → Add custom domain → `hse.harishankar.info`
4. Add the CNAME Netlify shows you to your DNS panel

### Option D — VPS / cPanel / shared hosting

Upload the contents of `pitch/` to a directory served at `hse.harishankar.info`. The
files are static; no PHP or Node runtime needed. Configure HTTPS via Let's Encrypt
(`certbot --nginx -d hse.harishankar.info`).

---

## Suggested subdomains

Pick whichever you prefer; they all work the same way:

- `hse.harishankar.info` — explicit audience
- `pulse.harishankar.info` — matches the platform name "HSE Pulse"
- `medai.harishankar.info` — generic platform branding
- `pitch.harishankar.info` — explicit purpose

---

## Updating the pitch

Both files are hand-edited HTML. There is intentionally no build step — every section is
clearly labelled with an HTML comment (`<!-- HERO -->`, `<!-- IMPACT -->` etc). Edit and
re-deploy by re-uploading the changed file.

The pitch numbers come from `docs/BRD.md` Section 11 (Business Impact and Value to
Ireland) and `docs/irish_hospital_ai_strategy_report.md`. If those change, mirror the
changes here.

---

## What I cannot do for you

DNS, hosting credentials, and any operation against your `harishankar.info` domain
require access I don't have. The four options above each take 5-15 minutes start to
finish; tell me which path you want and I'll give you the exact commands for that one.
