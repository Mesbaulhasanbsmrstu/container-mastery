# W2D1 — Multi-stage builds, layer caching, .dockerignore

## Baseline (Sunday) — trivy, CRITICAL+HIGH
- taskapi:0.1 (single-stage): CRITICAL 0, HIGH 6 (5 unique)
  - OS (Ubuntu 24.04, 124 pkgs): 0
  - PowerShell bundled in SDK image: 4 HIGH → removed by multi-stage
  - Microsoft.OpenApi 2.0.0 (my dependency): 1 HIGH, found twice (/app + leftover /src/bin)

## Recall (from memory, before notes)
- Week 1 carry-over: aspnet = runtime only (no SDK, can't publish); sdk = build. `AS` names a stage.

## Did
- Multi-stage Dockerfile: sdk (build) → aspnet (runtime), COPY --from=build
- .dockerignore at repo root: bin/, obj/, .git/, journal/, k8s/, charts/, *.md
- csproj copied + restored before source → restore layer cached on code changes
- Verified: API responds from taskapi:0.2

## Measurements
| | v0.1 (single-stage) | v0.2 (multi-stage) |
|---|---|---|
| Image size | 924 MB | 231 MB (−75%) |
| Rebuild after code change | 71.1 s | 3.8 s (~19× faster) |
| OS packages | 124 | 97 |
| .NET dependency files scanned | 35 | 3 |
| Trivy CRITICAL / HIGH | 0 / 6 | 0 / 1 |

## Broke / surprised me
- 5 of 6 HIGH findings fixed without changing code — just by not shipping the SDK.
- The remaining HIGH is in my own dependency: shipping less can't fix it, a version update can.
- "Container name already in use": old `api` container from Friday. Use `--rm` for test runs.
- Trivy re-downloaded its DB (~130 MB): it refreshes roughly daily.
- `.dockerignore` must sit at the build context root (repo root), not in docker/.

## Carry-over
- Microsoft.OpenApi 2.0.0 HIGH (CVE-2026-49451) → fix tomorrow

## Next session starts with
`cd src\TaskApi` → `dotnet add package Microsoft.AspNetCore.OpenApi` → `dotnet list package --vulnerable --include-transitive`