# socialseasonclub.com

> **⚠️ ARCHIVED 2026-07-14.** This static-HTML repo never reached prod. The canonical marketing site is the **WordPress build on DreamHost** at https://www.socialseasonclub.com. Kept here for git history + potential future ref (copy blocks, brand tokens, screenshots). Do not deploy this repo — a fresh push would fight the WP site. See `archive/2026-07-14` tag on `main` for the last-good pre-archive commit.

Marketing site, privacy policy, terms of service, and support page for **Social Season Club** (iOS app, Dallas–Fort Worth event discovery). Formerly HandsDown Social Club — rebranded 2026-07.

Live at: **https://www.socialseasonclub.com** (WordPress on DreamHost, not this repo).

---

## Stack

- **Static HTML + plain CSS.** No build step, no framework, no JS dependencies.
- Inter (Google Fonts) for body, system Georgia for serif headlines.
- Brand palette mirrors the iOS app: Deep Onyx `#0A0A0F` background, Club Gold `#D4A843` accent, white text on dark.
- Designed to be readable as raw HTML by a future hand-off dev.

## File layout

```
socialseasonclub-web/
├── index.html          # Landing page — hero, pillars, Club perks, FAQ, download CTA
├── privacy.html        # Privacy Policy (required for App Store submission)
├── terms.html          # Terms of Service
├── support.html        # FAQ + contact + bug reporting
├── 404.html            # Custom 404 page
├── assets/
│   └── styles.css      # All site styles, one file, brand-aligned
├── images/
│   ├── app-icon.png    # Favicon + apple-touch-icon
│   ├── discover.png    # Hero phone mockup (events list)
│   ├── rsvp.png        # Section 2 phone mockup (RSVP)
│   └── friends.png     # Section 4 phone mockup (friend activity)
├── _headers            # Cloudflare Pages — security headers + cache control
├── _redirects          # Cloudflare Pages — pretty URLs, www→apex
├── robots.txt
├── sitemap.xml
└── .gitignore
```

## Local preview

This is a static site — open `index.html` directly, or run a one-liner server from the repo root:

```bash
# Python 3
python3 -m http.server 8080
# Or Node
npx serve .
```

Then open `http://localhost:8080/`.

## Editing the site

- **Content edits** — open the relevant `.html` file. The structure is documented inline with HTML comments where it matters; everything else is named clearly enough to find by reading.
- **Style edits** — `assets/styles.css` is one file, sectioned with comment banners. Brand colors are CSS custom properties at the top (`:root { --gold: #D4A843; ... }`). Change those once to re-theme.
- **Privacy/Terms updates** — bump the "Last updated" date at the top of the page when you change anything material. The privacy policy is also referenced from the iOS App Store listing — keep them in sync.
- **Club perks** — listed in `index.html` under `<section class="section-club">`. Edit the nine `.perk` blocks.
- **FAQ** — in `index.html` under `<section class="section-faq">`; deeper troubleshooting lives in `support.html`.

## Domains

Both domains are registered at **DreamHost** (not Cloudflare Registrar):

- `socialseasonclub.com` — primary (registered 2026-07-09, 3-year term)
- `handsdownsocialclub.com` — legacy brand (registered 2026-04-21); should 301 to the new domain

Because DNS is hosted at DreamHost, attaching the apex custom domain to Cloudflare Pages requires either moving the zone to Cloudflare (add site → free plan → update nameservers at DreamHost) or CNAME records at DreamHost for subdomains.

## Deploy (Cloudflare Pages)

GitHub repo: `fuegaux/socialseasonclub-web`. Cloudflare Pages project builds from `main`.

**Build settings** (already configured in the Pages project):
- Framework preset: **None**
- Build command: *(blank — pure static)*
- Build output directory: `/` (root)

Every `git push` to `main` triggers an auto-deploy to `*.pages.dev`; custom domains follow once DNS points at Cloudflare.

### Verify it's live

```bash
curl -I https://socialseasonclub.com/          # → HTTP/2 200
curl -I https://socialseasonclub.com/privacy   # → HTTP/2 301 → /privacy.html
curl -I https://www.socialseasonclub.com/      # → HTTP/2 301 → apex
```

## TODOs

- [ ] New SSC Open Graph image — the old `images/og.png` was HandsDown-branded, so the `og:image` tag was removed. Make a 1200×630 social card (gold "SOCIAL SEASON CLUB" wordmark on onyx), drop it at `images/og.png`, and restore the `og:image` + `twitter:card summary_large_image` tags in `index.html`.
- [ ] Real TestFlight public link — currently the "Join the TestFlight" CTA emails Jose for an invite. Once a public TF link exists, swap the `mailto:` in `index.html` for the `testflight.apple.com/join/...` URL.
- [ ] Email forwarding — Jose may want `hello@socialseasonclub.com` to forward to his Gmail. Set that up in Cloudflare → Email Routing once the domain is on Cloudflare, then update the `mailto:` links sitewide (find/replace `josenegrete0910@gmail.com` → `hello@socialseasonclub.com`).
- [ ] App Store badge — once Social Season Club launches publicly, add the official "Download on the App Store" SVG badge next to the TestFlight CTA in the hero and the `#download` section.
- [ ] Founding-member waitlist — if we want to collect emails before the App Store launch, drop in a Cloudflare Worker + KV (or Tally embed) on the `#download` section.
