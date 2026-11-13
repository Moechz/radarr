# Privacy Policy — Radarr for TOS 7

Last updated: 2026-11-14

This policy covers the TOS application package `radarrdocker` (this repository)
and the local Radarr instance it runs on your TerraMaster NAS.

## What data the app handles

- Radarr settings, the movie library metadata and the SQLite database.
- Connection settings and credentials for the indexers and download clients that
  **you** configure in Radarr.
- Logs and backups created by Radarr.

All of it is stored on your NAS, under
`/Volume1/DockerAppData/radarrdocker/`, readable only by the application user
(uid 1000).

## Network connections

- Radarr contacts the indexers and download clients that you configure, in order
  to search for releases and hand them to the download client. The destinations
  are chosen by you; there is no fixed transfer region.
- Radarr may check GitHub for its own updates; this is upstream behaviour and can
  be disabled in Radarr's settings.
- The package itself sends no telemetry, analytics or usage data to Moechz,
  TerraMaster, LinuxServer.io or any third party.
- The web interface is served over plain HTTP on your local network.

## Data sharing

- No data is sold or shared by this package.
- The indexers and download clients you configure are independent services under
  their own policies; Radarr sends them the queries and credentials needed to do
  their job. Radarr does not send your library or history to the developer.
- The package does not include any indexer or download client.

## Retention and deletion

- Data stays on the NAS until you delete
  `/Volume1/DockerAppData/radarrdocker/` or uninstall the application with data
  removal in the TOS App Center.
- The `media` and `downloads` folders are your own files; back them up or remove
  them separately.
- Uninstalling without data removal keeps the folder so a later reinstall can
  reuse the settings.

## Your rights

You can view and edit your settings in the Radarr UI, export them by copying the
config folder, and delete them as described above. Because the app runs entirely
on your NAS, the developer does not hold a copy of your data.

## Trademark / non-official statement

Radarr and its logo are trademarks of their respective owners. This is an
unofficial community package and is not endorsed by or affiliated with the Radarr
project or TerraMaster.

## Contact

- Package issues: <https://github.com/Moechz/radarr/issues>
- Upstream project: <https://github.com/Radarr/Radarr>
- Upstream image: <https://github.com/linuxserver/docker-radarr>
