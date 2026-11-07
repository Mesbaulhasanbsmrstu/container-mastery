# W2D3 — User-defined networks, container DNS

## Recall (from memory, before notes)
- Needed explanation: why numeric UID (Kubernetes can only verify a number, not a name)

## Did
- Added EF Core + /db-health endpoint (SELECT 1)
- Created `tasknet`; ran SQL and API on it
- API reached SQL as `sql,1433` (container name + container port)
- Connection string passed at runtime via `-e ConnectionStrings__TaskDb=...`, not baked into the image

## Broke / surprised me
- Adding EF Core pulled in vulnerable Pkcs 9.0.11 (transitive). NuGet Audit flagged it at build time → fixed with a direct reference.
- Plain chiseled → "Globalization Invariant Mode is not supported" (SqlClient needs ICU).
  Fixed with `aspnet:10.0-noble-chiseled-extra`. Size: ___ MB (vs 125 MB plain chiseled).
- Inside a container, localhost = the container itself. -p ports are only for traffic from my PC.

## Carry-over
- None

## Next session starts with
`git rev-parse --short HEAD` → tag taskapi with semver + SHA → push to GHCR