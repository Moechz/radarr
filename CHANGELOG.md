# Changelog

All notable user-visible changes to the Radarr TOS application package.

## 6.4.4.10685-1 — 2026-11-08

### Added
- First release. Radarr (`linuxserver/radarr:6.4.4.10685-ls325`, Docker Hub official image, unmodified)
  packaged as a TOS 7 Docker application, published on port 8016 and running
  as a non-root user (1000:1000).
- Persistent data is kept under `/Volume1/DockerAppData/radarrdocker/`.

### Architectures
- Dual-architecture release: `radarrdocker_x86_64.tar.gz` (3974 B, `0a589f5a1ee28f014deef6ceb22da9862b6058d6dcd26bfed5e7324d1621eb81`) and
  `radarrdocker_aarch64.tar.gz` (3977 B, `03556df4977a90166167a2e95f6473cee1b663b01f2bff499eb876b8ff8c1a27`), built by `scripts/build-dual.sh`.
  x86_64 has the full on-device E2E; aarch64 passes the official check and the
  platform validation (no ARM host available for a container E2E).
