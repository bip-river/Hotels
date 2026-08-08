# Chain Index

A single-page reference for 102 North American hotel brands — J.D. Power 2026 satisfaction
scores, tier rankings, amenities, breakfast quality and footprint — plus a recommender that
suggests where to book for a given trip. Built with per-diem and official travel in mind,
where the practical question is usually "what's the best flag I'll actually find here?"

The whole thing is one self-contained `index.html`: inline CSS, inline JavaScript, and the
brand data hardcoded in the page. No build step, no dependencies, no backend.

## Viewing it

Open `index.html` in a browser — that works straight off the filesystem.

To serve it locally instead:

```sh
python3 -m http.server 8000
# then open http://localhost:8000
```

To publish it: **Settings → Pages → Source: Deploy from a branch**, pick the branch and the
`/` (root) folder. Once Pages is enabled it will serve at
`https://bip-river.github.io/Hotels/`. The `.nojekyll` file in the root tells Pages to
publish the files as-is rather than running them through Jekyll.

## What's in it

**Browse** — every brand grouped by tier, from Luxury down to Economy Extended Stay. Each
row shows the J.D. Power score, rank within its tier, parent company, typical nightly rate
band and a score gauge. Tap a row for the detail sheet: segment rank, tier rate range,
breakfast quality rated 1–5 with a note on what you actually get, an editorial read on the
brand, an amenity checklist, footprint, and best-for tags.

**Search and filters** — free text matches brand or parent company. Chip filters cover
tier, parent, footprint, amenities (all selected must be present), minimum breakfast
quality, best-for, and sort order.

**What should I book?** — answer trip type, destination, length of stay, budget ceiling and
any must-have amenities, and it returns a ranked top five with a plain-language reason for
each. Some answers hard-filter the pool: 4+ nights requires an in-room kitchen or an
extended-stay format, and official travel drops the Luxury and Upper Upscale tiers.

## How the recommender ranks

Each qualifying brand gets a blended score from two normalized inputs: its J.D. Power score
and its estimated footprint. The destination answer sets the weighting — a major city
weights score against footprint 80/20, a small town 60/40, and rural or field destinations
35/65. The further out you go, the more "one exists here" outweighs "it scored well."

Small bonuses then nudge brands tagged per-diem (official travel), field work (rural), or
long stay (4+ nights). When a brand makes the top five mostly on availability — it sits in
the bottom third of its own segment — the result says so explicitly rather than letting the
ranking imply quality.

## Data and caveats

**Scores and ranks** come from the J.D. Power 2026 North America Hotel Guest Satisfaction
Index Study: 104 brands benchmarked, 102 published in the ranked results, 44,787 stays
surveyed May 2025–May 2026, on a 1,000-point scale spanning check-in/out, connectivity,
facilities, food & beverage, guest rooms, staff service and value.

**Typical rate ranges** are tier-level, derived from STR/CoStar chain-scale benchmarks (US
ADR roughly $161 in early 2026). They are not brand-specific, and market and date move rate
far more than the flag does.

**Everything else is editorial** — breakfast quality, amenities, footprint estimates,
per-diem notes and best-for tags were assembled from brand standards and judgment, not from
J.D. Power. Amenities are brand standards and vary property to property; confirm before
booking. Footprint is a rule of thumb about US presence, not verified property counts.
Rankings shift annually.

This page is not affiliated with, endorsed by, or sponsored by J.D. Power or any hotel
company named in it.

## Editing the data

Everything lives in the inline `<script>` in `index.html`:

- `DATA` — one object per brand: name, parent, tier, score, rank, tier size, amenities,
  footprint, best-for tags and the editorial note.
- `A` and `U` — the amenity and use-case label constants referenced by each record.
- `BREAKFAST` — per-brand `[rating 1–5, description]`.
- `TIER_PRICE`, `TIER_ORDER`, `BUDGET_TIERS`, `DEST_WEIGHTS` — tier rate bands, display
  order, budget-to-tier mapping, and the recommender's weighting.

Edit and reload. There is nothing to compile.

## License

MIT — see [LICENSE](LICENSE). That covers this page's code and presentation. It does not
cover the underlying J.D. Power study data, which belongs to J.D. Power.
