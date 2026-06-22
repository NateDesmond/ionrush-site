# Bloomfall - promo + policy site

A polished, fully static website for **Bloomfall**, a free one-thumb iOS arcade
catcher by Urban Algorithm LLC. It's the game's promo landing page plus the two
pages the App Store listing needs: a **Privacy Policy** and a **Support** page.

Lives at: **https://bloomfall.urbanalgorithm.com**

Plain HTML + CSS only - no frameworks, no build step (the Fraunces + Inter
webfonts load from Google Fonts). A tiny bit of optional vanilla JS (the mobile
nav toggle) lives inline in each page and degrades gracefully if JS is disabled.

## Pages

| URL | File |
| --- | --- |
| `/`        | `index.html` - promo landing page |
| `/privacy` | `privacy/index.html` - Privacy Policy |
| `/support` | `support/index.html` - Support / Contact |

GitHub Pages serves `privacy/index.html` at the clean URL `/privacy` (and
likewise for `/support`), so the App Store "Privacy Policy URL" and "Support URL"
resolve to those addresses.

## Folder contents

```
bloomfall-site/
├── index.html              # promo landing page
├── privacy/index.html      # Privacy Policy  → /privacy
├── support/index.html      # Support/Contact → /support
├── assets/
│   ├── css/styles.css      # shared styles
│   └── img/                # Remi + palm art for the hero / screens
├── CNAME                   # custom domain for GitHub Pages
└── README.md               # this file
```

The images in `assets/img/` were copied from the game project
(`Bloomfall/www/assets/`): `remi_hero.png`, `remi_plant.png`, `palm.png`,
`sprout.png`, and `share_celebrate.png`. Every `<img>` has an
`onerror="this.style.display='none'"` fallback, so a missing asset never breaks
the layout.

## Preview locally

Open `index.html` in a browser - it works straight from disk. To get the clean
`/privacy` and `/support` URLs (closer to production), serve it:

```bash
cd bloomfall-site
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploy to GitHub Pages

This site hosts on **GitHub Pages** at the custom subdomain
`bloomfall.urbanalgorithm.com` (the included `CNAME` file wires that up).

1. Create a GitHub repository (suggested name: `bloomfall-site`). Put these files
   at the repo **root**: `index.html`, the `privacy/`, `support/` and `assets/`
   folders, `CNAME`, and this `README.md`.
2. Commit and push to the `main` branch.
3. In the repo: **Settings → Pages → "Build and deployment" → Source:
   _Deploy from a branch_ → Branch: `main`, folder: `/ (root)` → Save.**
4. Still on **Settings → Pages → "Custom domain"**: enter
   `bloomfall.urbanalgorithm.com` → Save. (The included `CNAME` file already sets
   this; GitHub will verify it.)
5. Wait for the DNS check to pass, then tick **Enforce HTTPS** (GitHub
   auto-provisions a free Let's Encrypt certificate - this can take a few
   minutes).

### DNS setup

At the DNS host for `urbanalgorithm.com`, add **one** record:

- **Type:** `CNAME`
- **Host / Name:** `bloomfall`
- **Value / Target:** `natedesmond.github.io`
  - include the trailing dot if your provider requires it.
- **TTL:** default / automatic.

This is a subdomain, so a CNAME record is correct. (Only an apex/root domain
would need A records pointing to GitHub's IP addresses - not the case here.)

Propagation usually takes a few minutes to ~an hour. Verify with:

```bash
dig bloomfall.urbanalgorithm.com +short   # should resolve to *.github.io
```

…or a checker like [whatsmydns.net](https://www.whatsmydns.net/). Once it
resolves, enable **Enforce HTTPS** as in step 5 above.

## TODO

- [x] **App Store URL added (live).** The "Coming soon to the App Store" buttons on
      the landing page are placeholders. Search `index.html` for
      `<!-- TODO: App Store URL -->` and swap the `href` (and styling) once the
      listing is live.
