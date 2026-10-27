# W2D2 — Non-root, chiseled images

## Recall (from memory, before notes)

## Did
- Fixed Microsoft.OpenApi HIGH CVE with a package update
- Added `USER $APP_UID`: uid 0 (root) → 1654 (app)
- Switched runtime to aspnet:10.0-noble-chiseled: no shell, non-root by default

## Measurements
| | taskapi:0.2 (aspnet + USER) | taskapi:0.2-chiseled |
|---|---|---|
| Image size | 231 MB | 125 MB |
| OS packages (trivy pkg_num) | ___ | ___ |
| CRITICAL / HIGH | ___ / ___ | ___ / ___ |
| Shell available | yes | no |

Size journey: 924 MB (v0.1) → 231 MB (multi-stage) → 125 MB (chiseled)

## Broke / surprised me
- csproj changed → restore layer re-ran (cache invalidation working as designed)
- `docker exec ... sh` on chiseled: "executable file not found": no shell by design
- `id` doesn't exist in chiseled either: check the user with `docker image inspect` instead
- "Container name in use": a running container keeps its name; `--rm` only removes it after stop
- `trivy docker image` → "unknown flag": the command is `trivy image`
- Trivy DB download timed out at 5 min (~400 KB/s). Used `--skip-db-update`; refresh with `--download-db-only --timeout 20m`

## Carry-over
- None

## Next session starts with
`docker network create tasknet` → run SQL + API on it, connect by container name