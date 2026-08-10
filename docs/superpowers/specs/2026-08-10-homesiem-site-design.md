# homesiem-site — Design

## Purpose

A static marketing/landing page for [homeSIEM](https://github.com/hibikipr/homeSIEM),
a self-hosted syslog collector and security console for a homelab. The site's
job is to explain what homeSIEM is and send visitors to the GitHub repo — it
is not a product with an app-store listing, so there's no "download" flow to
build, no privacy policy page (the site itself collects nothing), and no
login.

Modeled on the sibling repo `listnudge-site` (plain static HTML/CSS, GitHub
Pages, no build step, no JS framework) but re-themed: homeSIEM is a dark,
technical, self-hosted ops tool, not a light consumer app, so the site
borrows its palette directly from the real siem-web console rather than
ListNudge's warm-cream editorial look.

## Repo

New public GitHub repo: `hibikipr/homesiem-site`.

- `index.html` — the landing page (single page)
- `styles.css` — shared tokens/reset/base type
- `assets/` — homeSIEM icon files, copied from
  `homeSIEM/design_handoff_homesiem/icons/` (transparent + light variants)
- `README.md` — mirrors listnudge-site's README: what the repo is, how to
  update it, the GitHub Pages HTTPS-cert-stuck fix, a link back to the main
  `hibikipr/homeSIEM` repo
- `.gitignore` — same as listnudge-site (macOS + editor cruft)
- No `CNAME` file — homesiem.townsville.cc is already in use by the live
  siem-web console via reverse proxy; repointing it to GitHub Pages would
  break that. The site ships at `hibikipr.github.io/homesiem-site` for now.
  A distinct subdomain (e.g. `homesiem-site.townsville.cc`) can be added
  later as a separate DNS record without touching the existing one.

## Visual theme

Dark theme, pulled directly from `siem-web/src/lib/styles/tokens.css`'s real
values so the site reads as a preview of the actual console:

- `--bg: #131523`, `--surface: #232532` (cards), `--line: #292b31`
- `--text: #e9e9ed`, `--muted: #9397ab`
- `--accent: #968ae0`, `--accent-light: #b5abfc`, `--accent-deep: #423a6a`
  (used as an icon-badge fill, matching the design comp)

Typography: Inter (matches the design comp's font choice, and fits a
technical/ops tool better than ListNudge's Karla/Bricolage Grotesque
pairing). System-ui fallback stack, same as listnudge-site's approach.

No dark/light toggle and no `prefers-color-scheme` branching — the console
itself is dark-only today (`tokens.css` has no light variant), so a light
mode on the marketing site would be inventing a mode the product doesn't
have.

## Page structure (single page, `index.html`)

1. **Hero** — icon (badge-style, matching the design comp's nav icon
   treatment) + "homeSIEM" wordmark, tagline "Security triage for your
   homelab — not another metrics dashboard," a short paragraph pulled from
   the main README's framing (syslog in, correlation rules, alerts you
   triage in a browser), primary button "View on GitHub" linking to
   `https://github.com/hibikipr/homeSIEM`, secondary text link "Quickstart"
   anchored to section 4.

2. **Three pillars** — one card per real service, mirroring the README's
   architecture table:
   - **Ingest & enrich** (siem-ingest / Vector) — syslog sources, geo/threat-intel
     enrichment, fast path + full stream to Loki.
   - **Correlate & alert** (siem-api) — threshold/first-seen/absence rules,
     alert lifecycle, ntfy delivery.
   - **Triage console** (siem-web) — Wall, Search, Live tail, Alerts,
     Sources, Settings, OIDC login.

   Each card: kicker label, heading, 1–2 sentence description. No
   screenshots yet (none exist that reflect real running data — see
   "Known gaps" below); cards use an icon/kicker treatment only, matching
   the pillar-card layout pattern from listnudge-site but re-themed dark.

3. **Architecture strip** — a lightweight inline SVG diagram (no external
   image files): syslog sources → siem-ingest → {Loki, siem-api} →
   siem-web, with ntfy shown as a side output from siem-api. Simple boxes
   and arrows in the accent palette, not a literal reproduction of the
   design comp's architecture doc.

4. **Quickstart** — a code block with the real quickstart sequence from the
   main README:
   ```
   git clone https://github.com/hibikipr/homeSIEM.git
   cd homeSIEM
   cp .env.example .env
   docker compose up -d --build
   ```
   Below it, one line noting OIDC setup and the role-mapping bootstrap step
   are required before first login, with a link to the full README for
   details — the snippet should not overstate what "quickstart" gets you.

5. **Footer** — link to the GitHub repo, a one-line "self-hosted, open
   source" note, no privacy-policy link (nothing on this site collects
   data — no forms, no analytics, no accounts).

## Accessibility / quality bar

Same baseline as listnudge-site: single `:focus-visible` treatment via a
`--focus-ring` custom property, `prefers-reduced-motion` handling, real
`alt` text on the icon, semantic heading order, no color-only distinctions.

## Known gaps

- No real product screenshots — the only screenshot-shaped asset that
  exists (`design_handoff_homesiem/designs/*.dc.html`) is a design-comp
  export missing its supporting JS/CSS bundle, so its `{{ }}` template
  placeholders never render real data. Getting genuine screenshots means
  running the full stack (Loki, ntfy, siem-api, siem-web) plus an external
  OIDC provider, logging in, and capturing each screen — out of scope
  here. Screenshots can be dropped into `assets/` and wired into the
  pillar cards / hero as a follow-up once they exist.
- No custom domain yet (see "Repo" above).
