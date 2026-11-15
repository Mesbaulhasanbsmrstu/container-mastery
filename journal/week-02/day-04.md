# W2D4 — Tagging, pushing to GHCR

## Recall (from memory, before notes)
- Needed explanation: localhost = own container; user-defined network = name DNS; chiseled-extra for ICU

## Did
- Added OCI source label → GHCR package linked to the repo
- Committed first, then built with two tags: 0.2.0 + sha-a9a7422
- Pushed both to ghcr.io/mesbaulhasanbsmrstu/taskapi
- Deleted locally, pulled back from GHCR, ran on tasknet: /db-health OK

## Broke / surprised me
- Docker Desktop wasn't running → "dockerDesktopLinuxEngine" pipe error again
- `set IMG =value` → the space becomes part of the variable name. Use `set IMG=value`, check with `echo`.
- docker login needs the GitHub token (ghp_...), not the account password
- `-p 5000:8000` typo → "Empty reply from server": host port worked, nothing listening on container port 8000
- Two tags = one image ID; second push: "Layer already exists"

## Carry-over
- None

## Next session starts with
Close the notes, empty docker\Dockerfile, write the multi-stage non-root Dockerfile from memory