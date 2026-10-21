- Port conflict: local SQL Server already owns 1433, so SSMS hit it instead of the container (Login failed 18456).
  Diagnosed via container logs: no failed login = request never arrived. Fixed with `-p 14333:1433`.
- `@@SERVERNAME` returns the container ID: the UTS namespace again.