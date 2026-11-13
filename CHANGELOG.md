# Changelog

All notable user-visible changes to the Radarr TOS application package.

## 6.4.4-2 — 2026-11-14

### Changed
- Upgraded to `linuxserver/radarr:6.4.4.10685-ls326` (latest stable build; CVE
  remediation).
- Added `PRIVACY.md`, linked from the package `help` field: the app contacts only
  the indexers and download clients the user configures, sends no telemetry, and
  stores everything under `DockerAppData/radarrdocker`.
- Added the trademark/non-official statement: Radarr and its logo are trademarks
  of their respective owners; this is an unofficial community package.

### Fixed (review remediation)
- **C2**: trademark and non-official packaging statement added.
- **C3**: privacy policy added and linked from `help`.
- **C6**: runtime data sharing documented (indexers/download clients, no analytics).
- **C8**: third-party API destinations/transfer region documented (user-configured,
  no fixed region).
- **T1**: image updated to the latest stable LinuxServer.io build.

### Architectures
- x86_64: `radarrdocker_x86_64.tar.gz` (4346 B,
  `651323864abc260d246ccbe7ab3b40eaafa38b4b9fe01f56cefeac322954b5b1`)
- aarch64: `radarrdocker_aarch64.tar.gz` (4348 B,
  `550b2cede5f5661282d543fdc20d220c8ba03652e63d63b67d457575e1c315dd`)
- x86_64 full on-device E2E: 17/17 (healthcheck, uid 1000, three volumes,
  restart, reinstall keeps data); aarch64 passes the official check and the
  platform validation.

## 6.4.4-1 — 2026-11-08

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
