\# W3D2 — Healthchecks, started ≠ ready



\## Recall (from memory, before notes)

\- compose.yaml replaces long docker run commands + network

\- Service names are hostnames on the project network

\- Two volumes sections: in the service = use it; at the bottom = declare it

\- depends\_on alone = start order only, not readiness



\## Did

\- SQL healthcheck: sqlcmd SELECT 1 (interval 10s, timeout 5s, retries 10, start\_period 20s)

\- api depends\_on sql with condition: service\_healthy → API starts only after SQL is healthy

\- Ports bound to 127.0.0.1 → reachable only from my PC, not the LAN

\- Searched git history for the old SA password → found in journals → rotated it; real password only in .env



\## Broke / surprised me

\- `$$MSSQL\_SA\_PASSWORD`: SA\_PASSWORD lives in .env (for Compose); MSSQL\_SA\_PASSWORD lives inside the container. `$$` = let the container's shell expand it.

\- Without -d, compose runs attached: logs stream, Ctrl+C stops the whole stack

\- Container names = project-service-number (taskapp-sql-1); inside the network use the service name `sql`

\- Chiseled API has no shell/curl → no command healthcheck possible (Kubernetes probes check from outside)

\- A pushed secret = a leaked secret. Rotate it; deleting from files doesn't remove it from history.



\## Carry-over

\- None



\## Next session starts with

Break-it day: bad connection string, low memory, port conflict → diagnose each with logs / inspect / exit codes

