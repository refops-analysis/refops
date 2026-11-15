# refops
DevOps repository

## Nexis keepalive fallback (GitHub Actions)

`.github/workflows/nexis-keepalive.yml` is the second fallback scheduler for Nexis ghosting (VPS `hexad-vps` is primary, Azure DevOps `ado-refresh-drives` the first fallback; this one is `github-refops`).
Every 3 hours it runs `nexutils dehydrate --only-if-stale 6h` and `nexutils keepalive --only-if-stale 48h` on media-jpe, media-wus2 and media-wus, using the public `nexutils` release of ljcollins25/codebox.

- Secrets: `MEDIA_JPE_SAS_URL`, `MEDIA_WUS2_SAS_URL`, `MEDIA_WUS_SAS_URL`.
- Public logs: every call uses `--quiet` (counts only), no `set -x`, triggers are `schedule` and `workflow_dispatch` only.
- A weekly heartbeat commit (`heartbeat/last-run.txt`) and a `gh workflow enable` step keep the schedule from being disabled after 60 days of inactivity.
