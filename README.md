# homeSIEM — Website

Marketing site for [homeSIEM](https://github.com/hibikipr/homeSIEM), a
self-hosted syslog collector and security console for a homelab.

- `index.html` — landing page
- `styles.css` — shared foundation: the Townsville.cc theme tokens (night
  ground with the three saber glows, Michroma / Chakra Petch / JetBrains Mono)
  and homeSIEM's red-saber identity
- `assets/` — icon files (from the main repo's
  `design_handoff_homesiem/icons/`) and screenshots captured from the live
  console
- No `CNAME` — `homesiem.townsville.cc` is already in use by the live
  siem-web console via reverse proxy, so this site doesn't claim it. It
  currently serves at whatever GitHub Pages URL this repo is given
  (`https://hibikipr.github.io/homesiem-site/` once Pages is enabled). A
  distinct subdomain can be added as its own DNS record later if wanted.

Plain static HTML/CSS, no build step. The look follows the Townsville.cc
design system (night ground, saber glows, Michroma / Chakra Petch /
JetBrains Mono), with homeSIEM as the red-saber app. Deploys automatically on push to
`main` via GitHub Pages (once enabled in the repo's Settings → Pages).

## Updating

Edit `index.html` directly and push to `main` — Pages rebuilds in under a
minute. To refresh the icon, re-export it from the main repo's
`design_handoff_homesiem/icons/` into `assets/` here. The hero is a live, simulated incident demo (inline
script at the bottom of `index.html`). To refresh a
screenshot, capture it from the live console and save it into
`assets/` (the Wall section uses `wall.jpg`; the pillar cards use `sources.jpg`,
`insights.jpg`, and `search.jpg`).

## If the HTTPS certificate ever gets stuck

If a custom domain is added later: GitHub Pages certificate provisioning
can occasionally stall on the generic `*.github.io` wildcard cert instead
of issuing one for the custom domain. Fix: remove the `CNAME` file
(temporarily detaching the custom domain), let Pages rebuild, then add it
back — this re-triggers certificate issuance.

## Related repos

- [homeSIEM](https://github.com/hibikipr/homeSIEM) — the app itself
