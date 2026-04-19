# 影 Kage — Landing Page

> _the sovereign shadow of you_

Live: **https://dshakes.github.io/kage-landing/**

A personal AI that lives on your device, runs agents on your terms, and shares
nothing with anyone but you. This repo is the public landing page only —
static HTML/CSS/JS, no backend, no tracking.

## What's in here

This repo contains the **compiled static export** of the landing page.
Source lives in the private `kage` monorepo (`app/` at repo root).

```
.
├── index.html       # compiled from app/page.tsx
├── 404.html
├── _next/           # static chunks (JS, CSS, fonts)
├── .nojekyll        # tells GH Pages to serve _next/ unmolested
└── README.md        # you are here
```

## How it's hosted

GitHub Pages → Settings → Pages → `main` branch, root path.
Served at `https://dshakes.github.io/kage-landing/`.

The Next.js config in the source repo prefixes asset paths with
`/kage-landing` when `GH_PAGES=1` is set — so `_next/static/...` resolves
under the project-site subpath.

## Updating the landing

From the private monorepo (`kage/`):

```bash
# 1. Edit app/page.tsx or app/globals.css
# 2. Build with GH Pages basePath
GH_PAGES=1 npm run build

# 3. Sync build output into this repo's checkout and push
cd path/to/kage-landing
rm -rf ./* && rsync -a ../kage/out/ ./
touch .nojekyll
git add -A && git commit -m "Update landing" && git push
```

The Pages build runs automatically on push to `main`; live in ~30–60s.

## Moving to a custom domain

When the primary domain is finalized (likely `mykage.ai` or `kage.computer`):

1. Settings → Pages → Custom domain → enter the domain
2. Point DNS:
   - Apex `A` → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - `www` CNAME → `dshakes.github.io`
3. Wait for HTTPS cert to provision (usually <10 min)
4. In the source repo, **remove** `GH_PAGES=1` from the build (basePath
   goes away) — custom domains serve from root

Or migrate to Cloudflare Pages and keep this repo as the source — same build,
same output dir, global CDN.

## Design

- **Aesthetic:** warm sumi (charcoal-black) ink, crimson seal red (`#c8302e`),
  lacquer gold (`#d4a24c`). Negative space (`ma`) and wabi-sabi restraint.
- **Type:** Instrument Serif (display), Inter (body), JetBrains Mono (code).
- **Animation:** framer-motion reveals; respects `prefers-reduced-motion`.
- **Icons:** lucide-react.
- **Accessibility:** semantic landmarks (`<nav>`, `<main>`, `<footer>`),
  `aria-hidden` on decorative glyphs, visible focus rings, keyboard-reachable
  CTAs, `sr-only` label on the waitlist input.

## License

Source © 2026 Kage. Landing content is public; the implementation repo is
private during alpha.
