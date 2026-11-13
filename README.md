# radarr-for-tos

> TerraMaster TOS 7 App Center — **Docker application** package for
> [Radarr](https://github.com/linuxserver/docker-radarr).

## What this is

A four-file TOS Docker application archive (`radarrdocker.tar.gz`) that installs
Radarr on TOS 7 through the App Center:

- `config.ini` — TOS application metadata (`application_type: docker`, id `radarrdocker`)
- `radarrdocker.lang` — 23-language superset (TOS requires 14)
- `radarrdocker.svg` — application icon (529 bytes, 2 SVG elements)
- `docker-compose.yml` — one service, image `linuxserver/radarr:6.4.4.10685-ls326`

## How it works

| | |
|---|---|
| Container image | `linuxserver/radarr:6.4.4.10685-ls326` (Docker Hub, used unmodified) |
| Open | `http://<NAS-IP>:8016/` (container port 7878) |
| Runs as | non-root, `user: "1000:1000"` |
| Persistent data | `/config` → `/Volume1/DockerAppData/radarrdocker/config`, `/media` → `/Volume1/DockerAppData/radarrdocker/media`, `/downloads` → `/Volume1/DockerAppData/radarrdocker/downloads` |
| Logs | `docker logs radarrdocker` (stdout/stderr) |

`config` holds the database, settings and logs; `media` is the movie library; `downloads` is where completed downloads land. Back up `config`; the media folders are your own files.

## Design highlights

- **Non-root.** `user: "1000:1000"`; verified on real hardware (the container reports uid 1000).
- **Fixed image tag, never `:latest`**, Docker Hub only, no `privileged`, no `network_mode: host`, and no Docker socket, no `cap_add`.
- **Health check** probing the service on container port 7878.
- **Deterministic build**: GNU tar with uid/gid 0 and zeroed timestamps, gzip without a timestamp, plus the review-standards self-check above.
- **Runs as a non-root container.** Verified on real hardware as uid 1000.
- **Three volumes.** `/config` keeps the database and settings; `/media` and `/downloads` give the app a library and a download folder inside its own application data directory.
- **Health check** probes the web UI on container port 7878.
- **Fixed image tag, never `:latest`**, Docker Hub only, no `privileged`, no `network_mode: host`, no Docker socket, no `cap_add`.

## Privacy

The package sends no telemetry. Radarr contacts only the indexers and download
clients you configure (and may check GitHub for its own updates); the
destinations are chosen by you. See [PRIVACY.md](PRIVACY.md) for the full policy,
also linked from the package `help` field.

Radarr and its logo are trademarks of their respective owners. This is an
unofficial community package and is not endorsed by or affiliated with the Radarr
project or TerraMaster.

## Runtime file manifest

Everything the application writes on the NAS lives inside its own data
directory; the container-internal `/tmp` is discarded with the container. The
table lists what is created and how it behaves. Nothing else is written.

| Path | Purpose | Created when | Growth bound | Lifecycle |
|---|---|---|---|---|
| `config/radarr.db` | movies, quality profiles, indexers and download-client settings (SQLite) | first start | grows with the library | never overwritten; back it up |
| `/Volume1/DockerAppData/radarrdocker/config` | Radarr application data (everything the container writes outside /tmp) | on first start / on demand | bounded by your own data | persistent; kept on uninstall unless you ask to delete it |
| `/Volume1/DockerAppData/radarrdocker/media` | Radarr application data (everything the container writes outside /tmp) | on first start / on demand | bounded by your own data | persistent; kept on uninstall unless you ask to delete it |
| `/Volume1/DockerAppData/radarrdocker/downloads` | Radarr application data (everything the container writes outside /tmp) | on first start / on demand | bounded by your own data | persistent; kept on uninstall unless you ask to delete it |

## Architectures

The TOS store takes one package per architecture. This release provides both,
built from the same sources by `scripts/build-dual.sh`:

| Architecture | Package | Size | SHA-256 |
|---|---|---|---|
| x86_64 | `radarrdocker_x86_64.tar.gz` | 4346 B | `651323864abc260d246ccbe7ab3b40eaafa38b4b9fe01f56cefeac322954b5b1` |
| aarch64 | `radarrdocker_aarch64.tar.gz` | 4348 B | `550b2cede5f5661282d543fdc20d220c8ba03652e63d63b67d457575e1c315dd` |

The x86_64 package was verified with the full on-device E2E; the aarch64 package
passes the official `TOSAppSelfTestingTool -c` check and the developer platform's
automated validation (no ARM host was available for a container E2E).

## Build

```bash
scripts/build.sh                  # -> out/radarrdocker.tar.gz + .sha256
scripts/build.sh 6.4.4-2   # packaging iteration bump
scripts/build.sh 6.4.4-2 aarch64
```

The build stages the four files, substitutes the version/platform placeholders,
removes CRLF/BOM/AppleDouble, then runs
`tools/verify.py`, a review-standards self-check (archive layout, JSON validity,
required fields, 14+ language coverage, version consistency, Docker-Hub-only
images, fixed tag, reserved/recommended host ports, non-root user, per-service
healthcheck/TZ/restart, data path below the application data root, never the
`/Volume*` wildcard, comment-free sequence blocks, `x-app-meta` position, no
literal secrets and the SVG size/element limits).

## Local verification

```bash
./TOSAppSelfTestingTool -c out/radarrdocker.tar.gz   # official spec check
bash test/install-and-verify.sh                  # on-device E2E (needs root)
```

> Docker applications cannot be side-loaded through the App Center's manual
> install page (it accepts `.deb` only); the official self-testing tool's `-i`
> mode or `docker compose` are the only ways to install one for testing.

## Store submission checklist

1. Public GitHub repo (code + README only).
2. Release **tag `v6.4.4-2`**, assets `radarrdocker.tar.gz` +
   `radarrdocker.tar.gz.sha256` (Docker asset naming: `<app_id>.tar.gz`, **no
   platform suffix**).
3. Developer platform → Add Application: ID `radarrdocker`, package type Docker,
   repository URL, architecture `x86_64`.
4. Version Management → Add Version `6.4.4-2` → automated validation → review.

## License / branding

Radarr is distributed by its upstream authors under **GPL-3.0-only**. The container image is the official image from Docker Hub (`linuxserver/radarr:6.4.4.10685-ls326`) and is used unmodified; no upstream source code is redistributed here. This packaging is an independent community submission by Moechz and is not affiliated with the upstream project or TerraMaster. Radarr and its logo are trademarks of their respective owners.

## Limitations

No download client and no indexer are included; add them in Settings.
`/media` and `/downloads` belong to this app only. They are not shared with the Deluge, Sonarr or Lidarr packages - each TOS Docker app has its own data folder.
The page is plain HTTP on the LAN; use a TLS reverse proxy before exposing it to the internet.
Docker applications cannot be upgraded in place in the App Center: updating means uninstalling and reinstalling, which keeps the data folder.

---

Packaged for TOS 7 by Moechz. Upstream image used unmodified from Docker Hub.
