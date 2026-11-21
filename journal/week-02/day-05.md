# W2D5 — Closed-book rebuild + v0.2 release

## Recall the week (from memory, before notes)
- Multi-stage: only the last stage ships; SDK, source, build output stay in the build stage
- csproj + restore before source: restore stays cached on code-only changes
- `USER $APP_UID`: numeric 1654, so Kubernetes can verify non-root
- Plain chiseled + SqlClient: no ICU → Invariant Mode error → `chiseled-extra`
- Two tags: `0.2.0` = which release, `sha-<commit>` = exactly which commit
- Needed help on: ___

## Closed-book rebuild
- `git diff docker\Dockerfile` result: ___ (empty = perfect)
- Rebuilt image runs on tasknet, /db-health: ___

## Demo (image pulled from GHCR)
- Image: ghcr.io/mesbaulhasanbsmrstu/taskapi:0.2.0
- Size: ___ MB
- User: ___ (expect 1654)
- Trivy CRITICAL / HIGH: ___ / ___

## Week 2 results
| | v0.1 | v0.2 |
|---|---|---|
| Build | single-stage (SDK) | multi-stage: sdk → aspnet chiseled-extra |
| Image size | 924 MB | ___ MB |
| Runs as | root (uid 0) | app (uid 1654) |
| Shell in image | yes | no |
| Trivy CRITICAL / HIGH | 0 / 6 | 0 / ___ |
| Rebuild after code change | 71.1 s | 3.8 s |

## Weekly review
### Couldn't rebuild closed-book (→ Monday recall)
- ___

### Carry-over to next week
- None

### Broke / surprised me this week
- Plain chiseled → "Globalization Invariant Mode is not supported" (SqlClient needs ICU) → chiseled-extra
- NuGet Audit caught a vulnerable transitive package (Pkcs) at build time, before Trivy
- Trivy DB download timed out at 5 min on a slow connection → `--skip-db-update` / `--download-db-only --timeout 20m`
- cmd `set IMG =value` put the space in the variable name → always `set IMG=value`, check with `echo`
- `-p 5000:8000` typo → "Empty reply from server" (nothing listening on container port 8000)

### Demo command(s) for README
- Added `### v0.2` section to README.md

## Target met
- [ ] Multi-stage, non-root image with no critical CVEs, pushed to GHCR with proper tags

## Next session starts with
Sunday Week 3 prep: read carry-over, prepare for Docker Compose + GitHub Actions CI