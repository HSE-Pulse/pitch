# HSE Pulse — Pitch site

Two static files. Zero build step. Deployed to Firebase Hosting on the existing
`hse-pulse-ireland` GCP project, fronting `hse.harishankar.info`.

```
pitch/
├── index.html       # Single-page sales pitch (SPA)
├── deck.html        # Reveal.js 14-slide presentation
├── firebase.json    # Hosting config (cache headers, security headers, rewrites)
├── .firebaserc      # Pins to GCP project hse-pulse-ireland
└── README.md        # this file
```

All HTML loads its dependencies (Tailwind, Reveal.js, Inter / JetBrains Mono fonts) from
public CDNs — no `npm install`, no bundling, nothing to compile.

---

## Local preview

```bash
# from D:\project-demo\cancer\pitch
firebase emulators:start --only hosting       # http://localhost:5000
# OR plain HTTP (no firebase install needed):
python -m http.server 8000                    # http://localhost:8000
```

---

## One-time setup (you only do this once)

You need: a Google account that has Owner / Firebase Admin on the `hse-pulse-ireland`
project, and the Firebase CLI installed.

```bash
# Install the CLI (Windows: PowerShell or Git Bash)
npm install -g firebase-tools

# Authenticate — opens a browser tab
firebase login

# (sanity check) confirm the project pin matches your access
firebase projects:list                        # should show hse-pulse-ireland
firebase use hse-pulse-ireland                # already pinned in .firebaserc
```

If `hse-pulse-ireland` shows up but Firebase is not enabled on it, click "Add Firebase to
this Google Cloud project" in the Firebase console once. No new project, no migration —
Firebase Hosting just sits on top of the existing GCP project.

---

## Deploy (every time you want to ship a change)

```bash
# from D:\project-demo\cancer\pitch
firebase deploy --only hosting
```

The CLI prints a `*.web.app` URL once it's done — the site is live there immediately.
Custom-domain mapping below.

---

## Map `hse.harishankar.info` to it

In the Firebase console:

1. **Hosting → Add custom domain → `hse.harishankar.info`**
2. Firebase shows you either an A record or a TXT verification record. Add it to your DNS
   panel for `harishankar.info`.
3. Verification typically takes 5-30 minutes; HTTPS cert is provisioned automatically once
   verification completes.

Typical DNS records Firebase will ask for:

| Type | Host | Value (Firebase will give you the exact one) | TTL |
|------|------|-----------------------------------------------|------|
| TXT  | `hse` | `firebase=hse-pulse-ireland` (verification)  | 3600 |
| A    | `hse` | one or two Firebase Hosting IPs               | 3600 |

That's it — fully provisioned, automatic Let's Encrypt cert, global CDN included.

---

## What firebase.json does

- `cleanUrls`: serves `/deck` from `deck.html` (no extension in the URL)
- Cache headers: HTML revalidates every 5 minutes (so a deploy goes live quickly)
- Security headers: `X-Frame-Options`, `Referrer-Policy`, `Permissions-Policy` set sensibly
- `rewrites`: `/deck` → `/deck.html` (in case anyone shares the short URL)

---

## Free-tier headroom

Firebase Hosting free tier includes 10 GB storage and 360 MB/day egress. This site is ~80
KB total. You'd need ~4,500 pageviews per day to exceed free tier — which would be a great
problem to have.

---

## Updating the pitch

Edit `index.html` or `deck.html` directly (no build step). Then:

```bash
firebase deploy --only hosting
```

Numbers in the SPA come from `docs/BRD.md` Section 11 (Business Impact and Value to
Ireland) and `docs/irish_hospital_ai_strategy_report.md`. If those change, mirror the
changes here.

---

## Why not GitHub Pages / GCS+LB / GKE

- **GitHub Pages** — pulls in a separate Google-account-external dependency. Skipped.
- **GCS bucket + HTTPS LB** — works, but always-on LB is ~€18/mo for nothing. Skipped.
- **GKE** — your cluster is for the platform itself, not for serving 2 HTML files. Skipped.

Firebase Hosting is the right-sized tool for a static SPA on an existing GCP project.

---

## What I cannot do for you

`firebase login` is interactive and requires your Google credentials. The DNS change must
happen in your registrar's panel (whichever provider is authoritative for `harishankar.info`).
Both are 5-minute steps once you're at the keyboard.
