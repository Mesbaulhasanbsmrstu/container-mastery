\# W3D3 — Break-it day: exit codes, logs, inspect, stats, events



\## Recall (from memory, before notes)



\## Baseline (docker stats, healthy stack)

\- api memory: \_\_\_   sql memory: \_\_\_



\## Failures

| Break | Symptom | Exit code | Found with | Fix |

|---|---|---|---|---|

| Bad connection string (sqlx) | API Up, /db-health error: \_\_\_ | none (still running) | curl + logs | correct service name |

| mem\_limit 30m on API | \_\_\_ | \_\_\_ (OOMKilled=\_\_\_) | ps -a, inspect, events | remove/raise limit |

| Port 5000 already used | `up` failed: \_\_\_ | container never started | ps --filter publish, netstat | stop the blocker |



\## Broke / surprised me

\- Running ≠ working: Break 1 had no exit code at all

\- 128 + signal: 137 = KILL (9), 143 = TERM (15)

\- Compose recreated only the changed service



\## Carry-over



\## Next session starts with

Create `.github/workflows/ci.yml`: build → trivy → push to GHCR on every push to main

