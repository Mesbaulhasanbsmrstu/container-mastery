\# W1D1 — Containers vs VMs



\*\*Date:\*\* 2026-10-19

\*\*Time spent:\*\* \~30 min (plus Sunday prep the same day)



\## Recall (from memory, before notes)

\- The `docker` CLI is only a client; the daemon does the work. No Docker Desktop = no daemon = "pipe not found" error.

\- `docker pull` stores images in Docker's internal storage (WSL2 disk), not in my project folder.

\- A Dockerfile's `FROM` declares the base image; pulling only pre-caches it.



\## Did

\- Ran nginx: `docker run -d --name web -p 8080:80 nginx`, reached it on localhost:8080

\- `docker ps`, `docker ps -a`, `docker logs`, `logs -f`, `logs --tail 20`

\- Shelled in with `docker exec -it web sh` and explored `/proc`

\- `docker stop web`, then `docker rm web`



\## Broke / surprised me

\- `ps` doesn't exist in the nginx image: minimal images skip debugging tools. Used `/proc` instead.

\- Only \~10 PIDs visible inside the container, with nginx as PID 1: the PID namespace in action.

\- `/proc/version` showed the WSL2 kernel under a Debian userland: containers share the host kernel.

\- PID 43 was the `ls` command itself, gone by the time I checked: `/proc` is a live view.

\- A stopped container still exists (`docker ps -a`) until `docker rm`.

\- `-p` is host:container. Getting it backwards forwards to a port nothing listens on.

\- Sunday setup: `msc` vs `mcr` typo gave the `'<'` error (not a real registry);

&#x20; "Repository not found" because the GitHub repo didn't exist yet;

&#x20; README was created outside the repo folder.



\## Carry-over

\- None



\## Next session starts with

`docker run -d --name sql -e "ACCEPT\_EULA=Y" -e "MSSQL\_SA\_PASSWORD=Str0ng!Passw0rd" -p 1433:1433 mcr.microsoft.com/mssql/server:2022-latest`

then connect from SSMS to `localhost,1433` as `sa`

