# cutatlas.io redesign — drop-in files (2026-06)

Folder: `_redesign-2026-06/`

## What's here

| File | Replaces |
|---|---|
| `styles.css` | `/styles.css` |
| `index.html` | `/index.html` |
| `cutbridge.html` | `/cutbridge.html` |
| `cutmap.html` | `/cutmap.html` |
| `cutcompose.html` | `/cutcompose.html` |
| `about.html` | `/about.html` |
| `faq.html` | `/faq.html` |

Use `assets/`, `favicon-*`, `robots.txt`, `sitemap.xml`, `privacy.html`, `beta-terms.html`, `cutmap-privacy.html`, `cutmap-terms.html`, `cutmap-refund.html`, and `guides/` as-is — these are not touched. All internal links in the new pages point to the same paths as before.

## Drop into repo

```bash
# from repo root, with _redesign-2026-06/ at the top level
cp _redesign-2026-06/styles.css      ./styles.css
cp _redesign-2026-06/index.html      ./index.html
cp _redesign-2026-06/cutbridge.html  ./cutbridge.html
cp _redesign-2026-06/cutmap.html     ./cutmap.html
cp _redesign-2026-06/cutcompose.html ./cutcompose.html
cp _redesign-2026-06/about.html      ./about.html
cp _redesign-2026-06/faq.html        ./faq.html
git add styles.css *.html
git commit -m "Redesign 2026-06 — perspective hero, bento products, handoff-sim boot"
```

The old files are still in the repo's history if you need to roll back.

## What changed visually

**Brand-aligned (Brand Guidelines v0.1 honored):**
- Same palette as before (Deep Navy `#1B2843` / Cream `#F5F2EB` / Warm Black `#1A1815` / Warm Gray `#6B6660` / Light Gray `#A0998E` / Burgundy `#7A1F2F`)
- Same typography (Georgia serif + Courier mono + system sans-serif)
- Same emblem / lockup SVG paths inline
- No gradients, flat surfaces (brand rule)
- Burgundy used ≤5% (CTA underlines, playhead, flagship marker, tick affordances)
- "Atlas plate" editorial ornaments retained as identity (PL. I / EST. 2026 / hairline frames / monospace labels)

**New visual moments:**
- **Boot / handoff-sim screen** on index only — centered `DELIVERABLE/` box with file icons + ticks animated in, vertical scanline, italic "Handoff Complete" stamp. ~3s, sessionStorage-once, ESC/click to skip, respects `prefers-reduced-motion`.
- **Perspective Premiere panel mockup** in index hero and cutbridge hero — CSS-only (no images), `rotateY(-13deg) rotateX(7deg)` at rest, scroll-driven flatten to `-2°/+1°` as user scrolls past, cursor parallax on hover.
- **Glass floating chips** over panel — `Preflight 24/24` (italic Georgia) + `AI Assistant ready for handoff`. Single tasteful glass use, not pervasive.
- **Bento product grid** on index (replaces old `.plate` cards) — CutBridge flagship LG card with source→deliverable flow viz; CutMap MD with version tree; CutCompose SM with script-to-timeline lines. Burgundy accents on flow arrows, "current version", flagship tag.

## What's preserved (no behavior change)

- **All SEO meta**: `<title>`, `<meta description>`, canonical, OG, Twitter cards — character-for-character.
- **All Schema.org**: Organization on `/`, SoftwareApplication on `/cutbridge`, `/cutmap`, `/cutcompose`, FAQPage on `/faq`. The full 20-question FAQPage JSON-LD is retained.
- **CutBridge waitlist form** — same form IDs (`#waitlistForm`, `#wl-email`, `#wl-name`, `#wl-consent`, `#waitlistBtn`, `#waitlistStatus`), same endpoint (`https://cutbridge.fly.dev/waitlist/join`), same payload shape, same Supabase tester counter (`#testerCount`, `#testerCountNum`) — `https://ikpwodnohabtkiweevmw.supabase.co` RPC `beta_tester_count`.
- **CutMap Paddle checkout** — same token `live_07ea7ca3b9a2da9d04e67d08a32`, priceId `pri_01knxsfmnbrwxr0ykxx17t5z0c`, free-trial endpoint `https://cutmap.fly.dev/trial`. Form IDs preserved (`#trialForm`, `#trial-email`, `#trialBtn`, `#trialStatus`, `#buyBtn`).
- **Cloudflare Web Analytics** — beacon script + token `44f85e504acd4048890a82f660ed807d` on every page.
- **Nav structure** — same routes (`/cutbridge`, `/cutmap`, `/cutcompose`, `/guides`, `/about`), same `aria-current="page"` per section. Mobile toggle behaviour preserved (`#siteNav`, `#navToggle`, `#navLinks`).
- **Footer** — same four columns (brand, Products, Company, Legal), same outbound legal links per page (CutMap pages keep CutMap-specific legal).
- **Skip-link, focus-visible, sr-only** — accessibility primitives preserved.
- **Headings hierarchy** — one `<h1>` per page, sequential `h2`/`h3`. SEO good.

## Known gaps (per brand context — flagged, not invented)

These were not in the existing copy and are intentionally not added:
- **Exact CutBridge pricing** — placeholder messaging stays ("Final pricing will be announced after the beta"). No price invented.
- **Real customer testimonials / logos** — none present, none invented.
- **Founder's Edition / Studio per-seat tiers** — described in `brand-and-content-context.md` but not present in current site copy. Not added here; this redesign matches what's already live. Add when you're ready to commit pricing details.
- **Hard performance claims ("saves X hours")** — none invented.
- **CutCompose waitlist link** — page says "a public waitlist will open later"; no form added (matches existing state).

## SEO check

| Page | Title | Meta desc | Canonical | h1 | Schema.org |
|---|---|---|---|---|---|
| `/` | "CutAtlas, Editorial infrastructure for post-production" | ✓ | ✓ | 1 | Organization + WebSite |
| `/cutbridge` | "CutBridge by CutAtlas, Finishing-ready handoff from Premiere Pro" | ✓ | ✓ | 1 | SoftwareApplication |
| `/cutmap` | "CutMap by CutAtlas, Visual versioning for Premiere Pro" | ✓ | ✓ | 1 | SoftwareApplication + Offer $9 |
| `/cutcompose` | "CutCompose by CutAtlas, Automatic roughcut assembly for Premiere Pro" | ✓ | ✓ | 1 | SoftwareApplication |
| `/about` | "About, CutAtlas" | ✓ | ✓ | 1 | — |
| `/faq` | "FAQ, CutAtlas \| Premiere Pro handoff & versioning questions" | ✓ | ✓ | 1 | FAQPage × 20 |

FAQ page detail entries are `<details open>` by default — content is server-rendered, crawler-readable, NOT JS-collapsed. SEO requirement from project brief is met.

## Boot screen — operational notes

- Shows once per browser session (sessionStorage `cutbridge_boot_seen`).
- Direct hits to `/cutbridge`, `/cutmap`, etc. — NO boot. Only `/` shows it.
- `prefers-reduced-motion: reduce` → boot is completely skipped.
- Skip with `Escape` key or click anywhere.
- ~3s total runtime, with 650ms fade-out.
- During boot, `body.boot-active` prevents scroll. Removed when boot dismisses.

## Things to verify after deploy

1. Hit `/` in incognito — boot should show once, then again after closing/reopening browser.
2. Hit `/cutbridge` directly — boot must NOT show, page must render immediately with sticky nav working.
3. Submit waitlist form — confirm it still hits Fly endpoint and returns expected status messages.
4. Tester counter — should populate on `/cutbridge` if Supabase RPC is alive.
5. CutMap "Buy now" — confirm Paddle checkout opens with the expected SKU.
6. Mobile nav — hamburger toggle on viewport ≤880px, links expand below header.
7. Run a Lighthouse pass — expected wins vs Framer site: faster LCP (no Framer runtime), no CLS from animation artifacts, semantic HTML for crawlers.
8. Validate JSON-LD with Google's Rich Results test on `/faq` — should show all 20 Q&A pairs.

## Files for diff review (before merging)

Run a side-by-side diff against the current `main`:

```bash
diff -u index.html      _redesign-2026-06/index.html
diff -u cutbridge.html  _redesign-2026-06/cutbridge.html
diff -u cutmap.html     _redesign-2026-06/cutmap.html
diff -u cutcompose.html _redesign-2026-06/cutcompose.html
diff -u about.html      _redesign-2026-06/about.html
diff -u faq.html        _redesign-2026-06/faq.html
diff -u styles.css      _redesign-2026-06/styles.css
```

styles.css and index.html have the largest deltas. Other pages are surgical — section structure changed but copy, IDs, forms, scripts are intact.

## Not touched in this drop

- `guides/` — separate from this redesign pass.
- Legal pages (`privacy.html`, `beta-terms.html`, `cutmap-privacy.html`, `cutmap-terms.html`, `cutmap-refund.html`) — these have their own content needs and weren't part of the brief.
- `sitemap.xml`, `robots.txt` — unchanged.
- `assets/` and favicons — unchanged.

If you decide to take these into the new visual language too, the patterns are in `styles.css` already — `.stage.framed.page-hero` for a centered top + `.section .statement` or `.faq-list` for body.
