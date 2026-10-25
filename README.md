\# container-mastery



6-week Docker \& Kubernetes learning project: a .NET 10 API + SQL Server,

evolving from `docker run` to GitOps.



\## Week 1 target

API runs from a Dockerfile I wrote; SQL Server data survives deleting its container.



\## Demo

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

