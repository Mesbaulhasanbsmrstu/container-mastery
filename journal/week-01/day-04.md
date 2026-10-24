# W1D4 — Volumes vs bind mounts

## Recall (from memory, before notes)
- Needed help: (list any of RUN vs ENTRYPOINT, -p host:container / 8080, build context, port 1433 conflict)

## Did
- Proved data loss: DB in a container without a volume disappeared after `docker rm` + recreate
- Created named volume `sqldata`, mounted at `/var/opt/mssql`
- Created TaskDb + Tasks row, deleted the container, recreated it with the same volume: data survived
- Used `sqlcmd` inside the container to query without SSMS

## Broke / surprised me
- Same image + same command + same name ≠ same data: every new container gets a fresh writable layer.
- `-v name:/path` = volume; `-v C:\path:/path` = bind mount.
- `sqlcmd` inside the container uses `localhost` (port 1433), not 14333: port mapping exists only on the host.
- Connection strings never go in the Dockerfile: image = code, environment = config.

## Carry-over
- None

## Next session starts with
Close the notes, empty `docker\Dockerfile`, and write it from memory