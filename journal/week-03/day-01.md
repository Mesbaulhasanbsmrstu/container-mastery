\# W3D1 — Docker Compose



\## Recall (from memory, before notes)



\## Did

\- compose.yaml at repo root: services sql + api, ports, environment, volumes

\- Compose builds the API image (build: context/dockerfile)

\- SA password in gitignored .env, referenced as ${SA\_PASSWORD}

\- Reused the existing sqldata volume (name + external: true)

\- One command up, one command down



\## Broke / surprised me

\- First curl right after `up`: \_\_\_ (did it fail while SQL was still starting?)

\- Compose created network `taskapp\_default` automatically; service names are hostnames



\## Carry-over



\## Next session starts with

Add a healthcheck to `sql` + `depends\_on: condition: service\_healthy` for `api`

