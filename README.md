# container-mastery

6-week Docker & Kubernetes learning project: a .NET 10 API + SQL Server,
evolving from `docker run` to GitOps.

## Week 3 target
One `docker compose up` brings the full stack up healthy, and CI builds, scans and pushes on every push to main.

## Progress
| Week | Focus | Status |
|---|---|---|
| 1 | Container fundamentals | ✅ v0.1 |
| 2 | Production-grade images | ✅ v0.2 |
| 3 | Compose, debugging & CI | 🔄 in progress |
| 4 | Kubernetes fundamentals | ⬜ |
| 5 | Production-ready Kubernetes | ⬜ |
| 6 | Advanced operations | ⬜ |

## Demo

### v0.1 — Week 1: Container fundamentals
**Target:** API runs from a Dockerfile I wrote; SQL Server data survives deleting its container. ✅

```cmd
:: API from my Dockerfile
docker build -f docker\Dockerfile -t taskapi:0.1 .
docker run -d --name api -p 5000:8080 taskapi:0.1
curl http://localhost:5000/weatherforecast

:: SQL Server with a named volume (data survives docker rm)
docker volume create sqldata
docker run -d --name sql -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<strong-password>" -p 14333:1433 -v sqldata:/var/opt/mssql mcr.microsoft.com/mssql/server:2022-latest
```

### v0.2 — Week 2: Production-grade images
**Target:** A multi-stage, non-root image with no critical CVEs, pushed to GHCR with proper tags. ✅

| | v0.1 | v0.2 |
|---|---|---|
| Build | single-stage (SDK) | multi-stage: sdk → aspnet chiseled-extra |
| Image size | 924 MB | ___ MB |
| Runs as | root (uid 0) | app (uid 1654) |
| Shell in image | yes | no |
| Trivy CRITICAL / HIGH | 0 / 6 | 0 / 0 |
| Rebuild after code change | 71.1 s | 3.8 s |

```cmd
:: Pull the released image from GHCR
docker pull ghcr.io/mesbaulhasanbsmrstu/taskapi:0.2.0

:: SQL + API on a user-defined network; the API reaches SQL by container name
docker network create tasknet
docker run -d --name sql --network tasknet -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<strong-password>" -p 14333:1433 -v sqldata:/var/opt/mssql mcr.microsoft.com/mssql/server:2022-latest
docker run -d --name api --network tasknet -p 5000:8080 -e "ConnectionStrings__TaskDb=Server=sql,1433;Database=TaskDb;User Id=sa;Password=<strong-password>;TrustServerCertificate=True" ghcr.io/mesbaulhasanbsmrstu/taskapi:0.2.0
curl http://localhost:5000/db-health
```