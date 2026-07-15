# MediaHelm — Installation Guide

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

## 1. Docker image reference

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

## 2. Standard Docker install command

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

## 3. Host networking install command

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

Once running, open `http://<host-ip>:5005`. MediaHelm listens on port **5005** by
default.

---

## 4. Docker Compose example

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

Save the file as `compose.yaml` and run `docker compose up -d`. A ready-made
[`compose.yaml`](compose.yaml) is included in these docs.

---

## 5. GHCR package page

```
https://github.com/orgs/MunerisUK/packages/container/package/mediahelm
```

This is the **package / version-browsing page** — the closest equivalent to a
web-based "download page" for the image. It shows the published versions and
tags, the total pull count, and the exact `docker pull` reference. The actual
install still happens by pulling and running the image with Docker.

If the package is **public**, pull it directly — no login needed. If it is
**private**, sign in to the registry first:

```bash
docker login ghcr.io
```

using a GitHub username and a personal access token with the `read:packages`
scope.

---

## 6. "What is the MediaHelm download link?"

There isn't one in the usual sense — MediaHelm installs as a **Docker container**,
not a downloaded installer, so there's a Docker image reference instead of a
browser download:

```
ghcr.io/munerisuk/mediahelm:latest
```

Run the standard install command in section 2, then open
`http://<your-server>:5005`. You can browse the published container package at the
GHCR page above.
