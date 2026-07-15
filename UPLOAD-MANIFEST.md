# Upload manifest — MediaHelm docs & repos

What to put where. Everything below is **image-only** (no source, no private-repo
links) so it's safe for public docs while the code repo stays private.

## 1. `MunerisUK/mediahelm` (private code repo) — nothing to upload

All code changes (Tunaar → MediaHelm Live-TV rename, the Lemon Squeezy variant-ID
fix, UI fixes, feedback pipeline) were pushed straight to `main` via git. There is
nothing to hand-upload here.

## 2. `MunerisUK/mediahelm-docs` (public) — upload all of these

Use **Add file → Upload files** and overwrite/add each:

| File | State | Notes |
| --- | --- | --- |
| `README.md` | **updated** | Live-TV rename, variant-ID setup guidance, image-only install (no build-from-source / clone / run-without-Docker), fixed links |
| `index.html` | **updated** | Source→private-repo link removed; brand + Acceptable-Use links repointed to public; Live-TV link added |
| `mediahelm-comparison-3col.html` | **updated** | Live-TV rename + all glyphs (✓ — → " ') converted to HTML entities (pure ASCII) |
| `mediahelminstallguide.md` | **updated + split** | Now customer install steps only. Internal Muneris content (ops checklist, store-copy templates, publishing strategy, accuracy notes) was moved OUT to `INTERNAL-distribution-notes.md`, which is **kept private, not in this repo** |
| `compose.yaml` | **NEW** | Image-based Docker Compose (no build) — referenced by the docs |
| `unraid-mediahelm.xml` | **NEW** | Unraid Community Applications template, public links only |
| `.env.example` | **NEW** | Referenced by the Configuration section |
| `icon-192.png` | **NEW** | Used by the Unraid template `<Icon>` |
| `ACCEPTABLE_USE.md` | unchanged | Already present — no need to re-upload |

## 3. `MunerisUK/mediahelm-live-docs` (public) — not done yet

Not touched in this pass. It likely has its own naming to sort out. Ask and it can
be cleaned up the same way.

## Verified
- No "Tunaar" or "MCHammer65" anywhere.
- No links to the private `github.com/MunerisUK/mediahelm` repo (bare, `.git`,
  `/issues`, `/blob`, `/pkgs`, or raw). Only the public GHCR **package** page and
  the two **docs** repos are linked.
- No build-from-source / `git clone` / run-without-Docker instructions.
- All referenced files (`compose.yaml`, `unraid-mediahelm.xml`, `.env.example`,
  `icon-192.png`) are included.
