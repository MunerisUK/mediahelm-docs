# MediaHelm Installation and Distribution Instruction

> Prepared for Muneris Management Ltd. British English. Customer-facing sections
> are ready to send; internal notes are marked **_Internal (Muneris)_**.

---

## 1. What customers should be told

MediaHelm is supplied as a **Docker-based application**. It is not a traditional
installer — there is no `.exe`, `.dmg` or `.zip` to download and double-click, and
there is no "click-to-download" button in a web browser.

Instead, MediaHelm is published as a container image to the **GitHub Container
Registry (GHCR)**. You install it by having Docker **pull and run** that image on
the machine that will host MediaHelm (a home server, NAS, or any Linux/Docker
host). Once it is running, you use MediaHelm through your web browser.

You will need Docker installed on the host first (Docker Desktop on
Windows/macOS, or Docker Engine on Linux/NAS).

---

## 2. Docker image reference

```
ghcr.io/munerisuk/mediahelm:latest
```

In Docker terms this is the **install reference** — the address Docker uses to
fetch MediaHelm. It is the equivalent of a "download link", but for containers:
you give it to Docker, not to a web browser.

- `:latest` always resolves to the most recent published release.
- You can pin a specific version by replacing `latest` with a version tag, e.g.
  `ghcr.io/munerisuk/mediahelm:0.2.0`.

---

## 3. Standard Docker install command

```bash
docker run -d --name mediahelm --restart unless-stopped \
  -p 5005:5005 \
  -v mediahelm-data:/data \
  ghcr.io/munerisuk/mediahelm:latest
```

- `-p 5005:5005` publishes MediaHelm's web interface on port **5005**.
- `-v mediahelm-data:/data` persists your configuration, database and metadata
  snapshots in a named Docker volume, so they survive updates and restarts.

Once running, open:

```
http://<host-ip>:5005
```

Replace `<host-ip>` with the address of the machine running MediaHelm (for a
local install this is usually `http://localhost:5005`). Then follow the guided
setup.

---

## 4. Host networking install command

Use host networking if you want your media server (Plex, Emby or Jellyfin) to
**auto-discover MediaHelm as an HDHomeRun tuner**, which relies on a local
network broadcast (UDP 65001) that bridge networking does not pass through.

```bash
docker run -d --name mediahelm --restart unless-stopped \
  --network host \
  -v mediahelm-data:/data \
  ghcr.io/munerisuk/mediahelm:latest
```

With `--network host` you do **not** add `-p 5005:5005` — the container uses the
host's network directly.

Once running, open:

```
http://<host-ip>:5005
```

MediaHelm listens on port **5005** by default. If you have changed the port via
configuration, confirm the final port from MediaHelm's configuration or the
documentation.

---

## 5. Docker Compose example

```yaml
services:
  mediahelm:
    image: ghcr.io/munerisuk/mediahelm:latest
    container_name: mediahelm
    restart: unless-stopped
    ports:
      - "5005:5005"
    volumes:
      - mediahelm-data:/data
    #
    # Alternative: for HDHomeRun auto-discovery (UDP 65001), use host networking
    # INSTEAD of the "ports" mapping above — remove "ports:" and uncomment:
    # network_mode: host

volumes:
  mediahelm-data:
```

Use **either** explicit port mapping (`ports:`) **or** host networking
(`network_mode: host`) — not both. Choose host networking if you want the tuner
to be auto-discovered by your media server; otherwise the port mapping is simpler
and works for the web dashboard and manual tuner setup.

Save the file as `docker-compose.yml` and run `docker compose up -d`.

---

## 6. GHCR package page

```
https://github.com/orgs/MunerisUK/packages/container/package/mediahelm
```

This is the **package / version browsing page** — the closest equivalent to a
web-based "download page" for the image. It shows the published versions and
tags, the total pull count, and the exact `docker pull` reference. It is for
browsing and copying the image reference; the actual install still happens by
pulling and running the image with Docker.

> **_Internal (Muneris):_** If the package is set to **private**, customers must
> authenticate before pulling:
> ```bash
> docker login ghcr.io
> ```
> (using a GitHub username and a personal access token with `read:packages`).
> If the package is **public**, customers can pull it directly with no login.
> For a paid product distributed by licence key, keeping the image **public** is
> usually simplest — the paid features are gated by the in-app licence, not by
> registry access — but that is a commercial decision.

---

## 7. Documentation and public install guide

- **Code repository:** https://github.com/MunerisUK/mediahelm
- **Documentation repository:** https://github.com/MunerisUK/mediahelm-docs

> Note: the code repository is the **source code**, not a customer download link.
> Customers should not be pointed at it to "download" MediaHelm.

**Recommendation:** MediaHelm should have a **public, human-readable install
page** at a stable URL. Customers need installation instructions *before* the app
is running, so relying only on in-app documentation creates a chicken-and-egg
problem — they cannot read the in-app guide until they have already installed and
started the app.

Publish the `mediahelm-docs` content to a public, stable location, such as:

- **GitHub Pages** from the `mediahelm-docs` repository
- **Cloudflare Pages**
- **Netlify**
- a branded subdomain such as `docs.mediahelm.app` or `install.mediahelm.app`
- a documentation page on the Muneris website

Then use that single URL everywhere you sell or onboard (store, invoice, licence
email), so customers always have one clean installation address.

---

## 8. Customer-facing store / onboarding wording

> Polished copy suitable for a store page, invoice, licence email or onboarding
> page.

**Installing MediaHelm**

MediaHelm is supplied as a Docker-based application. There is no traditional
installer to download — you run it as a container on your own server or NAS.

Install the latest MediaHelm container with:

```bash
docker run -d --name mediahelm --restart unless-stopped \
  -p 5005:5005 \
  -v mediahelm-data:/data \
  ghcr.io/munerisuk/mediahelm:latest
```

Then open `http://<your-server>:5005` and follow the guided setup.

The official container package is available at:
https://github.com/orgs/MunerisUK/packages/container/package/mediahelm

For setup, updates, configuration and troubleshooting, see the MediaHelm
installation guide at **[your published docs URL]**.

---

## 9. Short answer to "What is the MediaHelm download link?"

> Drop-in reply for support / sales.

MediaHelm is installed as a **Docker container**, not downloaded as a traditional
installer, so there isn't a browser download link — there's a Docker image
reference instead.

Use this image:

```
ghcr.io/munerisuk/mediahelm:latest
```

Install command:

```bash
docker run -d --name mediahelm --restart unless-stopped \
  -p 5005:5005 \
  -v mediahelm-data:/data \
  ghcr.io/munerisuk/mediahelm:latest
```

Then open `http://<your-server>:5005`.

You can view the published container package here:
https://github.com/orgs/MunerisUK/packages/container/package/mediahelm

---

## 10. Release-check instruction (reusable checklist for an agent/LLM)

> **_Internal (Muneris):_** Run this to confirm a new MediaHelm image is live on
> GHCR after a release. Do not assume timing — check the actual state.

**Product:** MediaHelm
**Code repository:** https://github.com/MunerisUK/mediahelm
**Documentation repository:** https://github.com/MunerisUK/mediahelm-docs
**Container package:** https://github.com/orgs/MunerisUK/packages/container/package/mediahelm
**Docker image:** `ghcr.io/munerisuk/mediahelm:latest`

Confirm:

1. **Build succeeded** — the "Publish Docker image" GitHub Actions run for the
   push/tag completed successfully (green).
2. **Image pushed** — a new image appears on the GHCR package page.
3. **Tags published** — check which tags were produced. For a tagged release this
   should include the semver tag (e.g. `0.2.0`), the `major.minor` tag (`0.2`), a
   short commit SHA tag, and `latest`.
4. **`latest` is correct** — `latest` points at the expected release commit /
   version.
5. **Pull access** — confirm whether the package is **public** (pull directly) or
   **private** (`docker login ghcr.io` first).
6. **Multi-architecture** — MediaHelm publishes **linux/amd64** and
   **linux/arm64**; confirm both architectures are present (important for NAS /
   Raspberry Pi / Apple Silicon customers).
7. **Public docs available** — a public installation page exists at a stable URL
   for customers.

Quick manual checks:

```bash
# Pull and confirm the image resolves
docker pull ghcr.io/munerisuk/mediahelm:latest

# Confirm both architectures were published
docker buildx imagetools inspect ghcr.io/munerisuk/mediahelm:latest
```

If the documentation repository is private or no public docs site exists,
**publish a small public install-docs site** (GitHub Pages / Cloudflare Pages /
Netlify) so customers have a clean, stable installation URL.

---

## _Internal (Muneris) — accuracy notes_

- **Port and volume are verified from the product**, not placeholders: the image
  `EXPOSE`s **5005** and runs `uvicorn ... --port 5005`, and it persists to the
  **`/data`** volume (`VOLUME ["/data"]`, SQLite DB at `/data/mediahelm.db`).
  The commands above use `/data`; a `/config` mount (as in generic templates)
  would **not** persist MediaHelm's data.
- **Multi-arch is confirmed:** the publish workflow builds
  `linux/amd64,linux/arm64`.
- **Image path:** the publish workflow pushes to `ghcr.io/${{ github.repository }}`
  → `ghcr.io/munerisuk/mediahelm`, so `ghcr.io/munerisuk/mediahelm:latest` is
  correct.
- **Action needed — stale image path in README:** the repository `README.md`
  quick-start still references the old `ghcr.io/mchammer65/mediahelm:latest`
  path. Customers reading the README would pull from the wrong location. Update
  the README (and any docs) to `ghcr.io/munerisuk/mediahelm:latest` before
  publishing the install page.
- **Version/date not invented:** no specific release date or build hash is stated
  here. The current release being prepared is `0.2.0` (tag not yet pushed at time
  of writing).
