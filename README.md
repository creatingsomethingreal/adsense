# The Quiet Hours — an AdSense website for Vercel

A complete, content-rich static website wired for **Google AdSense**, ready to
deploy on **Vercel**. This is the *web* equivalent of what you asked for:
AdMob does not work on websites (it's an Android/iOS-only SDK), so for a site
that earns from Google ads, **AdSense** is the correct product. Same Google,
same payouts — just the web version.

```
quiet-hours-site/
├── index.html                 # homepage (hero, article grid, ad slots)
├── about.html
├── privacy.html               # required by AdSense (cookie/ads disclosure)
├── styles.css
├── ads.txt                    # required by AdSense — authorizes your account
├── articles/
│   ├── timeboxing.html
│   ├── two-minute-rule.html
│   ├── shutdown-ritual.html
│   └── inconvenient-distraction.html
└── README.md
```

---

## ⚠️ Read this first: how AdSense actually starts paying

Unlike a code library you just `import`, AdSense has a **gate**: ads only appear
**after Google reviews and approves your live site**, which can take anywhere
from a day to a few weeks. Until then, ad slots stay blank — that's normal, not
a bug. To get approved you need:

1. The site **live on a real URL** (your Vercel domain works).
2. **Genuine, original content** (this site ships with 4 real essays — keep them
   or, better, replace them with your own writing; thin/empty sites get rejected).
3. A **privacy policy** (included) and **ads.txt** (included).
4. An **approved AdSense account** tied to your domain.

Two more rules that protect your account:
- **Ads never show on `localhost`** — only on the deployed site.
- **Never click your own ads.** Google permanently bans accounts for it.

---

## Step 1 — Deploy to Vercel (no build step; it's static)

**Easiest — drag & drop:** go to [vercel.com](https://vercel.com), New Project →
deploy, and drop the `quiet-hours-site` folder in. Done.

**Or via Git:** push this folder to a GitHub repo, then "Import Project" in
Vercel. Framework preset: **Other**. Build command: **(leave empty)**.
Output directory: **(leave empty / `.`)** — the files are served as-is.

**Or via CLI:**
```bash
npm i -g vercel
cd quiet-hours-site
vercel            # preview
vercel --prod     # production
```

You'll get a live URL like `https://quiet-hours-site.vercel.app`. Open it — the
site works fully right now; only the ad boxes are empty (expected pre-approval).

> Tip: a custom domain (Vercel → Project → Settings → Domains) tends to fare
> better in AdSense review than a default `*.vercel.app` subdomain.

## Step 2 — Get your AdSense IDs

1. Sign up / sign in at [adsense.google.com](https://adsense.google.com).
2. **Add your site** (your Vercel URL). AdSense gives you a code snippet — it's
   the same loader already in every page's `<head>` of this project, so once you
   do Step 3 you've satisfied the "connect your site" requirement.
3. Note your **publisher ID**: `ca-pub-` followed by 16 digits.
4. Create **display ad units** (Ads → By ad unit → Display). Each one gives a
   **10-digit slot ID**. Create one per placement (6 used here).

## Step 3 — Plug your IDs in

**a) Publisher ID (one find-and-replace does the whole site):**
search every file for `ca-pub-XXXXXXXXXXXXXXXX` and replace with your real
`ca-pub-…`. This updates every loader script and every `data-ad-client`.

**b) Slot IDs:** replace each placeholder `data-ad-slot` with the real slot ID
from the matching ad unit:

| Placeholder slot | Location |
|---|---|
| `0000000001` | Home — leaderboard under the hero |
| `0000000002` | Home — in-feed (inside the article grid) |
| `0000000003` | Article — in-content (timeboxing) |
| `0000000004` | Article — in-content (two-minute-rule) |
| `0000000005` | Article — in-content (shutdown-ritual) |
| `0000000006` | Article — in-content (inconvenient-distraction) |

**c) `ads.txt`:** replace `pub-XXXXXXXXXXXXXXXX` with your publisher ID digits
(written as `pub-…`, no `ca-`). After deploy, confirm it loads at
`https://yourdomain/ads.txt`.

### Shortcut: Auto ads (skip the slot IDs entirely)
If you'd rather not manage individual units, just do step (a), then in AdSense
turn on **Auto ads** for your site. Google then places ads automatically using
the loader script that's already in every `<head>`. You can leave the manual
`<ins>` units in place or delete them. Manual units give you control over
placement (the "strategy"); Auto ads is the one-click option.

## Step 4 — Submit for review & wait
In AdSense, request review of your site. Keep it live and unchanged while Google
checks it. When approved, ads begin filling the slots automatically — no
redeploy needed.

---

## Customizing
- **Content:** edit the `.html` files in `articles/` or add new ones (copy an
  existing file, change the text, link it from `index.html`). More original
  content improves both approval odds and revenue.
- **Look:** all styling is in `styles.css` (colors are CSS variables at the top).
- **Contact/identity:** fill in the placeholders in `about.html` and
  `privacy.html` before going live.

## Compliance checklist before launch
- [ ] Real `ca-pub-…` everywhere (`grep -r ca-pub- .` shows no `XXXX`).
- [ ] `ads.txt` updated and reachable at `/ads.txt`.
- [ ] Privacy policy contact + date filled in.
- [ ] Approved AdSense account, site submitted for review.
- [ ] You never click your own ads.

## Docs
- AdSense get-started: https://support.google.com/adsense/answer/9724
- Where to place ad code: https://support.google.com/adsense/answer/9190028
- Vercel static deploys: https://vercel.com/docs
