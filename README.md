# MediaHelm 📺

[![License: Proprietary](https://img.shields.io/badge/License-Proprietary-red.svg)](LICENSE)

**Your Plex command center — and a full Live TV control plane — in one
self-hosted app.**

MediaHelm http://ghcr.io/munerisuk/mediahelm:sha-1f06c70 sits beside Plex (and Emby/Jellyfin) and gives you the cockpit they
never shipped: see who's watching, fix *"why won't this play?"*, manage IPTV
channels, and run live TV through your media server — all from one clean, fast
dashboard you own. It doesn't replace Plex; it makes running it effortless.

```bash
docker run -d --name mediahelm -p 5005:5005 -v ./data:/data \
  ghcr.io/munerisuk/mediahelm:latest
```

Open `http://your-server:5005`, follow the guided setup, and you're live.

---

## Acceptable use — MediaHelm supplies no content

MediaHelm is administration & diagnostics **software**. It ships with **no media,
channels, streams, playlists or subscriptions** — every source is provided, owned
and controlled by you, on your own hardware. Its tuner uses the open **HDHomeRun**
standard (like SiliconDust, Plex DVR and Channels DVR) to present *your own*
sources to *your own* media server; MediaHelm originates nothing.

You may only use MediaHelm with content and sources you are lawfully entitled to
use. The full policy is **built into the app** — you must agree to it to finish
setup, it gates the in-app Help, it lives at `/legal/acceptable-use`, and it's
linked in every page's footer — and is reproduced in
[`ACCEPTABLE_USE.md`](ACCEPTABLE_USE.md).

---

## Why people install it

- **🎬 Know your server at a glance** — live streams (who's watching, direct play
  vs. transcode), users, and remote-access health. No SSH required.
- **🛠️ Fix problems faster** — a *"why did this fail?"* playback compatibility
  checker, a library scan for missing artwork/metadata, **one-click metadata &
  poster rollback**, and an open issue portal feeding an admin Kanban.
- **📡 Turn IPTV into Live TV** — import M3U + XMLTV, appear to Plex/Emby/Jellyfin
  as an **auto-discovered HDHomeRun tuner**, share **one upstream across many
  viewers**, fail over between providers, and transcode with FFmpeg profiles.
- **🔒 Secure & robust by default** — generated secrets, CSRF + SSRF protection,
  security headers, owner-locked sign-in, DB migrations. (See
  [`docs/SECURITY_AUDIT.md`](docs/SECURITY_AUDIT.md).)
- **📱 Runs and looks great everywhere** — one multi-arch Docker container for
  QNAP / Synology / Unraid / Linux (amd64 + **arm64**), dark/light themes, a
  native-style mobile UI, and **installable as an offline PWA**.

---

## How MediaHelm compares

**vs. just Plex** — Plex is great at playing your media; it's thin on *operating*
your server. MediaHelm adds the admin layer around it:

| You want to… | Plex alone | With MediaHelm |
|---|---|---|
| See live streams + transcode reasons | basic | dashboard + diagnostics |
| Understand *why* a file won't direct play | ✗ | compatibility checker |
| Roll back a bad metadata/poster refresh | ✗ | one-click snapshots |
| Let users report problems & triage them | ✗ | portal + Kanban |
| Run IPTV channels as Live TV | needs a separate tuner | built-in HDHomeRun + proxy |

**Alongside [Dispatcharr](https://github.com/Dispatcharr/Dispatcharr)** —
Dispatcharr is an excellent, dedicated IPTV stream manager and proxy, and it
inspired MediaHelm's Live TV module (credit where it's due 🙏). The two have
different centers of gravity:

- **Dispatcharr** is the specialist — if your focus is deep, large-scale IPTV
  management and proxying, it's a fantastic, mature, purpose-built tool.
- **MediaHelm** is the generalist Plex companion — stream monitoring, playback
  diagnostics, metadata rollback and issue triage, *with* IPTV/Live TV as one
  part of a broader control plane.

They're not mutually exclusive: you can happily run Dispatcharr for heavy IPTV
duty and point Plex at it, while using MediaHelm for day-to-day Plex
administration and diagnostics. Pick the tool whose focus matches yours — or use
both.

### Tunaar or MediaHelm — which fits?

Both are ours and built for different jobs, so this isn't a better/worse call —
it's about what you want to run.

**[Tunaar](https://github.com/MCHammer65/Tunaar) is the IPTV specialist.** If all
you need is to get IPTV M3U playlists into Plex, Emby or Jellyfin as reliable Live
TV channels, Tunaar does exactly that: one Docker container, real tuner slots,
ffmpeg remuxing so streams don't drop, automatic EPG merging and channel
auto-discovery. It's lean, focused and quick to stand up.

**MediaHelm is the all-in-one media-server companion.** It includes the same kind
of IPTV/HDHomeRun tuner module, but that's one part of a much wider tool: it's
really there to *run and troubleshoot your whole server* — stream and bandwidth
monitoring, playback diagnostics, one-click metadata/poster rollback, a user
problem-report portal with triage, and multi-server support across
Plex/Jellyfin/Emby.

At a glance:

| Capability | Tunaar | MediaHelm |
|------------|:------:|:---------:|
| IPTV M3U → Plex/Emby/Jellyfin (HDHomeRun tuner) | ✓ | ✓ |
| Real tuner slots (concurrency cap + release) | ✓ | ✓ |
| Reliable streaming (ffmpeg remux / proxy) | ✓ | ✓ |
| EPG auto-detect & merge | ✓ | ✓ |
| Merge multiple IPTV sources, collision-free numbering | ✓ | ✓ |
| Channel-group control | ✓ | ✓ |
| Tuner auto-discovery (UDP 65001) | ✓ | ✓ |
| Web dashboard + live activity logs | ✓ | ✓ |
| Stream failover between sources | — | ✓ |
| Server admin & user management | — | ✓ |
| Stream / bandwidth monitoring | — | ✓ |
| Playback diagnostics | — | ✓ |
| One-click metadata / poster rollback | — | ✓ |
| User problem-report portal + triage | — | ✓ |
| Notifications (email / Discord / Telegram) | — | ✓ |
| Multi-server (Plex + Jellyfin + Emby together) | — | ✓ |

(Tunaar focuses entirely on doing the IPTV rows well; the lower rows are
MediaHelm's broader media-server remit.)

> **The complete Tunaar feature set is included in every MediaHelm tier** — the
> entire IPTV/Live TV tuner (M3U/EPG, HDHomeRun emulation + discovery, tuner
> slots, ffmpeg streaming, group control, admin console) is never feature-gated
> behind a higher tier. (MediaHelm itself has no free tier: a Standard or Pro
> licence/trial is required to run it. The cheap IPTV-only option is the separate
> Tunaar product.) Higher tiers only *add* features on top.

Rule of thumb: **choose Tunaar if you want IPTV and little else; choose MediaHelm
if you want a control panel for your entire media server** (with IPTV included).

For current plans and pricing, see the in-app **License** page.

---

## Features

> **Works with Plex, Jellyfin and Emby.** Pick your server type in Settings. The
> companion features below run against all three; *metadata snapshot/rollback*
> is currently Plex-only. **Remote-access help** is server-agnostic — health
> check plus guided Tailscale / Cloudflare-Tunnel / reverse-proxy / port-forward
> setup. Live TV / IPTV works with every server (a standard HDHomeRun tuner).

| Area | What it does |
|------|--------------|
| **Plex sign-in** | Official Plex PIN linking (no password) or a direct server URL + token. |
| **Active streams dashboard** | Live view of who's watching what, with transcode/direct-play status, auto-refreshing every 5s. |
| **Users** | Owner + shared users with access to your server. |
| **Issue reporting portal** | An open, account-free form (`/report`) your users can submit playback problems through. |
| **Admin Kanban** | Triage reports across **New → Investigating → Fixed → Closed**. |
| **Library scan** | Flags obvious problems: missing posters, summaries, years, unmatched items. |
| **Compatibility checker** | The *"why did this fail?"* engine — predicts direct play / direct stream / transcode and explains why, per media file. |
| **Metadata / poster snapshots** | Capture an item's metadata + poster and **roll back** after a bad refresh or edit. |
| **Remote access health** | Checks whether your server is reachable from outside the LAN. |
| **Notifications** | Email (SMTP), Discord and Telegram — fired on new issue reports, plus a test button. |
| **Acceptable Use Policy** | Built in and required — users must agree to it to finish setup; it also gates in-app Help, lives at `/legal/acceptable-use`, and is linked in every footer. MediaHelm supplies no content. |

---

## Live TV / IPTV (Dispatcharr-style)

MediaHelm includes an IPTV stream manager + proxy that sits between your IPTV
sources and your media server, and appears to Plex/Emby/Jellyfin as an
**HDHomeRun tuner**.

| Feature | What it does |
|---------|--------------|
| **Playlist management** | Import & curate **M3U/M3U8** playlists; channels parsed with `tvg-id`, logos, groups and channel numbers. Re-import is idempotent. |
| **EPG automation** | Import **XMLTV** guide data (`.xml` / `.xml.gz`), matched to channels by `tvg-id`; now/next lookup. |
| **HDHomeRun emulation** | `/discover.json`, `/lineup.json`, `/lineup_status.json`, `/device.xml` — add MediaHelm as a DVR tuner in Plex/Emby/Jellyfin. |
| **M3U export** | `/playlist.m3u` — point any plain-M3U player (TiviMate, IPTV Smarters, OTT Navigator, Kodi, VLC, Channels DVR) straight at MediaHelm; guide included via `url-tvg`. |
| **Xtream Codes API** | `player_api.php` / `xmltv.php` / `get.php` / `/live/…` — connect any IPTV player with one URL + username + password; categories, channels & EPG in a single login. |
| **Stream proxy** | One upstream connection fan-out to multiple clients via `/proxy/{id}`, reducing connections to your provider. |
| **Output profiles** | Per-channel **passthrough / remux / transcode** via FFmpeg (bundled in the image). |
| **Guide output** | `/epg.xml` regenerates XMLTV for the curated lineup. |
| **Real-time monitoring** | Live dashboard of who's watching what, profile, duration and throughput. |
| **Auto-discovery** | HDHomeRun UDP responder (port 65001) — Plex finds the tuner with no URL typing (needs host networking). |
| **Auto-refresh** | Background scheduler re-imports playlists & EPG on a per-source interval. |
| **Stream failover** | Multiple sources per channel (auto-merged by `tvg-id` across providers); the proxy fails over if one drops. |
| **Auto-reconnect** | The proxy restarts an upstream that drops mid-watch (EOF / token rotation / blip) so the player never sees the gap; capped retries stop a dead channel hot-looping. |
| **VPN support** | Route all IPTV egress through a VPN (see `docker-compose.vpn.yml`). |

**Connect Plex:** Settings → Live TV & DVR → *Set up Plex DVR* → enter
`http://<host>:5005` as the HDHomeRun device, then point the guide at
`http://<host>:5005/epg.xml`.

**VPN:** `docker compose -f docker-compose.yml -f docker-compose.vpn.yml up -d`
routes all egress through a Gluetun container.

> **Status:** the playlist/EPG parsing, channel management, HDHomeRun lineup,
> XMLTV output and the passthrough proxy with live monitoring are implemented and
> tested. Remux/transcode build real FFmpeg pipelines (and run when FFmpeg is
> present — it is, in the Docker image), but heavy-load tuning, SSDP
> auto-discovery broadcast, and per-provider auth quirks are foundational and
> may need hardening for large (10k+ channel) setups.

---

## Quick start (Docker)

Use a pre-built multi-arch image (amd64/arm64) from GHCR — no build needed:

```bash
docker run -d --name mediahelm -p 5005:5005 -v ./data:/data \
  ghcr.io/munerisuk/mediahelm:latest
```

Or build from source with compose:

```bash
git clone https://github.com/MunerisUK/mediahelm.git && cd mediahelm
cp .env.example .env          # optional — a secret is generated on first run
docker compose up -d
```

Then open **http://localhost:5005** and click **Sign in with Plex**.

The SQLite database and metadata snapshots are persisted to `./data` on the host.

### Choosing a different port
The container listens on `5005` internally. To publish it on a different host
port, set `MEDIAHELM_HOST_PORT` in `.env` (e.g. `MEDIAHELM_HOST_PORT=9000`) and
`docker compose up -d` — MediaHelm is then reachable at `http://<host>:9000`.
Port `5005` is safe on a typical Plex host (Plex itself uses `32400`); its only
common conventional use is Java remote debugging (JDWP).

### NAS notes
- **QNAP** (one-line deploy over SSH — Container Station required):
  ```bash
  ssh admin@<qnap-ip>
  git clone https://github.com/MunerisUK/mediahelm.git
  cd mediahelm
  sh deploy/qnap-deploy.sh            # add -p 9000 to pick a host port
  ```
  The script finds Container Station's Docker/Compose, generates a secret key,
  writes `.env`, builds, launches, and prints the URL. Re-run with `-u` to
  update (git pull + rebuild). See [`deploy/qnap-deploy.sh`](deploy/qnap-deploy.sh)
  (`-h` for all options).
- **Synology**: use Container Manager and import `docker-compose.yml`, or run
  the image directly mapping a volume to `/data` and publishing `<your-port>:5005`.
- **Unraid**: install via the included **Community Applications template**
  ([`deploy/unraid/mediahelm.xml`](deploy/unraid/mediahelm.xml)) — maps `/data`,
  publishes `5005`, and exposes the key env vars. For HDHomeRun auto-discovery,
  set the container's Network Type to **Host**.

---

## Run locally (without Docker)

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
export MEDIAHELM_SECRET_KEY=dev-secret
uvicorn app.main:app --reload --port 5005
```

---

## Configuration

Everything can be set via `.env` (see `.env.example`) **or** from the in-app
**Settings** page. Secrets entered in the UI are stored in the database and
masked on redisplay. Key environment variables:

| Variable | Purpose |
|----------|---------|
| `MEDIAHELM_SECRET_KEY` | Signs session cookies — **change this**. |
| `MEDIAHELM_DATABASE_URL` | SQLite path (default `/data/mediahelm.db` in Docker). |
| `MEDIAHELM_PLEX_BASEURL` / `MEDIAHELM_PLEX_TOKEN` | Optional direct Plex connection. |
| `MEDIAHELM_SMTP_*` | Email notifications. |
| `MEDIAHELM_DISCORD_WEBHOOK` | Discord notifications. |
| `MEDIAHELM_TELEGRAM_BOT_TOKEN` / `_CHAT_ID` | Telegram notifications. |

---

## Architecture

```
app/
  main.py            FastAPI app factory + router wiring
  config.py          Env-based settings
  database.py        SQLAlchemy engine/session
  models.py          Issue, Setting, MetadataSnapshot
  security.py        Signed-cookie admin sessions
  routers/           One module per feature area (thin HTTP layer)
  services/          Business logic: Plex client, scans, diagnostics, notifications
  templates/         Server-rendered Jinja2 UI (no build step)
  static/            CSS + a little vanilla JS
```

Design choices: server-rendered HTML (zero front-end build), a defensive Plex
wrapper that degrades gracefully when Plex is unconfigured/unreachable, and a
key/value settings store so the app is usable with no `.env` at all.

### Mobile / iOS

The UI is responsive and optimised for iPhone and iPad. On compact screens a
**native-style bottom tab bar** (Home / Issues / Library / Checks / More) replaces
the desktop nav, with the full menu in a slide-up "More" sheet. The Kanban becomes
a swipeable row, tables scroll, forms use 16px fields (no focus-zoom), and
safe-area insets keep content clear of the notch / home indicator.

It's an installable, **offline-capable PWA**: a service worker (`/sw.js`)
precaches the app shell and serves a friendly offline page when the network
drops. On iOS Safari tap **Share → Add to Home Screen** to launch MediaHelm
full-screen like a native app (manifest + Apple touch icons included).

### Design language

MediaHelm follows the **[Tunaar](https://github.com/MCHammer65/Tunaar) design
philosophy — "robust and effortless / just works."** A dark-first UI with a
light-theme toggle, Tunaar's signature orange gradient accent
(`#ff7a18 → #ffb347`), a radial background glow, a sticky topbar header with
horizontal nav, and clean bordered panels. The colour tokens mirror Tunaar's
own `style.css`, so MediaHelm sits visually alongside it in a homelab stack.

---

## Robustness / production notes

- **Database migrations** — schema is managed by **Alembic** and brought to head
  automatically on startup; existing `create_all` databases are adopted (stamped)
  so upgrades are clean. Run manually with `alembic upgrade head`.
- **Secure by default** — if no `MEDIAHELM_SECRET_KEY` is provided, a strong
  session secret is generated and persisted to `data/secret.key` on first run.
- **CSRF protection** — admin state-changing requests are guarded by a
  double-submit-cookie token (machine endpoints and the public report portal are
  exempt); session cookies are `SameSite=Lax`.
- **Resilient fetches** — playlist/EPG downloads retry with exponential backoff
  and a configurable User-Agent; the stream proxy sends a player UA and answers
  Plex's `HEAD` probes.

## Tests

```bash
pip install -r requirements-dev.txt
pytest
```

Tests cover health/readiness, the issue + Kanban service, the public report
flow, and the compatibility engine — none require a live Plex server.

---

## Health endpoints

- `GET /healthz` — liveness (used by the Docker `HEALTHCHECK`).
- `GET /readyz` — readiness (verifies the database).

---

## Roadmap

Aligned to the tiered product plan:

- **Standard**: the full media-server companion suite plus metadata rollback
  history and advanced notification routing, on top of the IPTV tuner.
- **Pro**: offline downloads, automated monitoring & alerts, DVR / deep
  monitoring, multi-user portals, multi-server (Jellyfin/Emby) bridge, bandwidth
  policies, API/webhooks.

---

## Legal / scope

MediaHelm is for personal media management, diagnostics and lawful libraries.
It deliberately avoids anything that facilitates acquiring copyrighted media.

## License

MediaHelm is **proprietary software**. © 2026 Muneris Management Ltd. All rights
reserved — see [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE).

**What that means for you:**

- 🔒 The Software is proprietary and confidential. No licence is granted by
  access to this repository.
- 📄 Use is governed solely by a written agreement with Muneris Management Ltd
  (subscription, lifetime purchase licence, or End User Licence Agreement).

**💼 Licensing.** MediaHelm is licensed commercially by the copyright holder,
**Muneris Management Ltd** (company no. 09096411). For subscriptions, lifetime
licences, or other commercial terms — including **Standard / Pro** features —
**contact [info@muneris.co.uk](mailto:info@muneris.co.uk).**

Contributions are accepted under the DCO + inbound license grant in
[`CONTRIBUTING.md`](CONTRIBUTING.md), which assigns commercial licensing rights
to the maintainer.

> Not legal advice — have the CLA and commercial terms reviewed by a lawyer
> before taking revenue.

### Pricing

There's **no free tier** — MediaHelm is **Standard** or **Pro**, each with a
**30-day free trial** (a Lemon Squeezy free-trial subscription, no card required):
a new user clicks *Start free trial*, signs up at Lemon Squeezy with just an
email, and gets a licence key that unlocks the full feature set for 30 days.
Lemon Squeezy expires the key if the trial isn't paid. After that, an install
with no active licence is *unlicensed*: it is **nagged for 14 days, then the
admin app soft-locks** — the dashboard and companion features redirect to the
License page, but **already-configured players keep streaming** (the tuner, guide,
M3U and stream proxy stay live) so a household isn't cut off mid-show. The cheap
**IPTV-only** option is the separate **Tunaar** product (£16.99/yr or £49.99
lifetime), not a free MediaHelm tier. Note: within MediaHelm the IPTV tuner is
never *feature-gated* behind a higher tier — Standard and Pro both include it.

Two paid tiers, billed **annually** (no lifetime — the app tracks evolving
Plex/Jellyfin/Emby and IPTV APIs, so paid support is ongoing). Both include the
30-day free trial:

| Tier | Billing | Adds (on top of the always-included IPTV/Live TV tuner) |
|------|---------|--------------------------------------------------------|
| **Standard** | annual · 30-day free trial | The full media-server companion suite — playback diagnostics, stream & bandwidth monitoring, metadata rollback + history, library audit, remote-access helper, issue portal, advanced notifications |
| **Pro** | annual · 30-day free trial | Everything in Standard **+** offline downloads, automated monitoring & alerts, DVR / deep monitoring, multi-user portals, bandwidth policies, API / webhooks, multi-server bridge |

> Current prices are shown on the in-app **License** page and should match your
> Lemon Squeezy variant prices.

Set up a free-trial period on a Lemon Squeezy subscription variant (turn **off**
"require card" for a no-friction taster) and put its checkout URL in
`MEDIAHELM_LEMONSQUEEZY_CHECKOUT_TRIAL`; `MEDIAHELM_TRIAL_DAYS` (display only)
should match it. The Standard/Pro price labels come from the `MEDIAHELM_PRICE_*`
settings — note the internal keys are historical, so **`MEDIAHELM_PRICE_PRO` =
Standard** and **`MEDIAHELM_PRICE_POWER` = Pro**; keep them in sync with your LS
variant prices.

### Standard / Pro licensing (via Lemon Squeezy)

Paid tiers are gated behind `gating.ensure_feature(...)`. **Lemon Squeezy
generates, emails and manages the license keys** — MediaHelm validates them
against the LS License API rather than minting its own.

Vendor setup:

1. In Lemon Squeezy, enable **License keys** on your Standard and Pro product
   variants (Lemon Squeezy emails the key to the buyer automatically on
   purchase).
2. Copy each **Variant ID** into the variant settings so a validated key
   resolves to the right tier, and paste the hosted **checkout URLs** to show
   "Buy" buttons on the License page. Note the internal setting keys are
   historical, so **`…_VARIANT_PRO` / `…_CHECKOUT_PRO` = Standard** and
   **`…_VARIANT_POWER` / `…_CHECKOUT_POWER` = Pro**.
3. (Optional) Set per-variant **activation limits** in Lemon Squeezy to cap how
   many installs one key can run on.

That's it — no private signing key, webhook endpoint or SMTP needed for
delivery.

How activation works for the customer:

- They paste the key into **Settings → License** and click Activate. MediaHelm
  calls the LS `activate` endpoint, records the returned instance id, and unlocks
  the tier mapped from the variant.
- The license is **re-validated online** at most weekly. The result is cached, so
  a self-hosted/air-gapped box keeps working through a **14-day offline grace
  window** if it can't reach Lemon Squeezy; beyond that it falls back to the free
  tier until connectivity returns (the key is retained and recovers
  automatically). Refunds/cancellations take effect at the next successful
  re-validation.
- **Remove license** deactivates the instance with LS (freeing an activation
  slot) and clears local state.

The validation client lives in `app/pro/lemonsqueezy.py`; the cache + grace
logic in `app/pro/licensing.py`.
