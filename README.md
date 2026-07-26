# Keisar family trips dashboard

A single-page Hebrew (RTL) dashboard of the Keisar family's travel history —
timeline, flight costs, total costs, and per-person shared-travel days — from
2010 to today. Hosted at a private URL for the family only.

> **This repository holds private household spending figures (in shekels).**
> It must stay **private** and must **never** be moved to, forked into, or
> mirrored on a public repository. Do not publish the numbers anywhere public
> without Ronen's explicit say-so.

## What it is

`index.html` is the entire application. There is **no build step** — it is
self-contained vanilla HTML, CSS, inline SVG charts, and vanilla JavaScript.
Open the file in any browser and it works. The only external request is the
Google Fonts `<link>` in the `<head>`; the page degrades gracefully without it.

Do not add a bundler, a framework, or a CDN-loaded chart library. Chart.js from
a CDN was tried in an earlier iteration and failed to load; both charts are
hand-rolled inline SVG and must stay that way.

## Where the data lives

All content is in the `DATA` block near the top of the `<script>` at the bottom
of `index.html`:

- `pre`, `trips`, `solo` — family trips (couple era / full family / partial family)
- `personal` — Ronen's personal trips
- `biz` — business trips
- `F` — flight-cost chart
- `D` — total-cost chart and the cost table
- `countries` — the country chips and the countries KPI

**Every displayed number is derived from this data at render time** — the trip
counts, the four KPI tiles, the four shared-days tiles, the country chips, the
Boston/business tiles, the two chart notes, the cost table, and the day counts
(parsed from the `D/M עד D/M` range at the start of each `meta`). To update the
dashboard, edit the data and the numbers recompute themselves. See the comments
in the `DATA` and `DERIVED VALUES` sections.

- `who` on each family trip (`D`=Dana, `R`=Ronen, `G`=Geffen, `A`=Almog,
  `M`=Agam) feeds the shared-days tiles.
- `paxPaid` on a flight is the count of fare-paying tickets when a child flew as
  a free lap infant; the per-passenger figure divides by it.

## Hosting and access

The site is served by **Cloudflare Pages** (auto-deploys from this repo's `main`
branch) and gated by **Cloudflare Access**, which shows a login wall to everyone
except a small allow-list of family email addresses. Visitors get a one-time PIN
by email; no account or password is needed.

**To add or remove a family member:** Cloudflare dashboard → Zero Trust →
Access → Applications → *Keisar Trips* → the *Family* policy → edit the Emails
list. No redeploy is needed. (Remember there is a second Access application, or
disabled previews, covering the `*.pages.dev` preview URLs — keep those gated
too.)

## Updating

```bash
cd ~/Projects/keisar-trips
# edit index.html
git add -A && git commit -m "Describe the change"
git push
```

Cloudflare rebuilds automatically within about a minute. Nothing else to run.

The next substantive update is due mid-August 2026, when the Romania trip
(31 Jul – 15 Aug) finishes and its actual costs land. It currently renders as a
future row (white outline) and appears in the flights chart at ₪8,115 booked.
After the trip, add its costs to the `D` array and it becomes a normal row.

## Conventions

- No em-dashes anywhere in the file or in commit messages.
- No analytics.
- Do not restructure the three-palette visual design: Italian green/white/red
  for family, Scottish blue/white for Ronen's personal trips, US navy/red with a
  striped rule for business. It went through sixteen iterations.

## Layout

```
index.html          the whole dashboard (edit this)
archive/            previous versions, for reference only
README.md           this file
```
