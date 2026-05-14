# handsdownsocialclub.com

Marketing site, privacy policy, terms of service, and support page for **HandsDown Social Club** (iOS app, Dallas–Fort Worth event discovery).

Live at: **https://handsdownsocialclub.com** (after Cloudflare Pages setup — see Deploy below).

---

## Stack

- **Static HTML + plain CSS.** No build step, no framework, no JS dependencies.
- Inter (Google Fonts) for body, system Georgia for serif headlines.
- Brand palette mirrors the iOS app: Deep Onyx `#0A0A0F` background, Club Gold `#D4A843` accent, white text on dark.
- Designed to be readable as raw HTML by a future hand-off dev.

## File layout

```
handsdownsocialclub-web/
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

## Deploy (Cloudflare Pages)

Jose owns `handsdownsocialclub.com` on Cloudflare Registrar, so DNS + hosting unify on one account.

### One-time setup

1. **Push this repo to GitHub** (already done if you cloned from there; otherwise see "Git setup" below).
2. **Create a Cloudflare Pages project**:
   - Go to **dash.cloudflare.com → Workers & Pages → Create → Pages → Connect to Git**.
   - Authorize GitHub and pick the `handsdownsocialclub-web` repo.
   - **Build settings**:
     - Framework preset: **None**
     - Build command: *(leave blank — pure static)*
     - Build output directory: `/` (root)
     - Root directory: `/` (root)
   - Click **Save and Deploy**. First deploy completes in ~30 seconds and gives you a `*.pages.dev` preview URL.
3. **Attach the custom domain**:
   - In the Pages project → **Custom domains → Set up a custom domain**.
   - Add `handsdownsocialclub.com`. Cloudflare detects the registrar match and adds the DNS automatically.
   - Add `www.handsdownsocialclub.com` as a second custom domain — the `_redirects` file in this repo will 301 it to the apex.
   - SSL is automatic — wait ~2 minutes for the certificate.
4. **Done.** Every `git push` to `main` triggers an auto-deploy.

### Verify it's live

```bash
curl -I https://handsdownsocialclub.com/        # → HTTP/2 200
curl -I https://handsdownsocialclub.com/privacy # → HTTP/2 301 → /privacy.html
curl -I https://www.handsdownsocialclub.com/    # → HTTP/2 301 → apex
```

## Git setup (if starting fresh)

```bash
cd ~/Desktop/Apps/handsdownsocialclub-web
git init
git add .
git commit -m "Initial site"
gh repo create handsdownsocialclub-web --public --source=. --remote=origin --push
```

## TODOs

- [ ] Real TestFlight public link — currently the "Join the TestFlight" CTA emails Jose for an invite. Once a public TF link exists, swap the `mailto:` in `index.html` line ~31 for the `testflight.apple.com/join/...` URL.
- [ ] Open Graph image — `images/og.png` is referenced but not yet created. Make a 1200×630 social card (gold "HANDS DOWN" wordmark on onyx) and drop it at `images/og.png`.
- [ ] Email forwarding — Jose may want `hello@handsdownsocialclub.com` to forward to his Gmail. Set that up in Cloudflare → Email Routing once the domain is on Pages, then update the `mailto:` links sitewide (find/replace `josenegrete0910@gmail.com` → `hello@handsdownsocialclub.com`).
- [ ] App Store badge — once HandsDown launches publicly (Q4 2026 per roadmap), add the official "Download on the App Store" SVG badge next to the TestFlight CTA in the hero and the `#download` section.
- [ ] Founding-member waitlist — Phase 1 of the roadmap targets 500 founding members. If we want to collect emails before the App Store launch, drop in a Cloudflare Worker + KV (or Tally embed) on the `#download` section.
