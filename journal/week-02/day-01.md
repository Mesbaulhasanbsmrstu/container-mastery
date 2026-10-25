## Baseline (Sunday) — trivy, CRITICAL+HIGH
- taskapi:0.1 (single-stage): CRITICAL 0, HIGH 6 (5 unique)
  - OS (Ubuntu 24.04, 124 pkgs): 0
  - PowerShell bundled in SDK image: 4 HIGH → removed by multi-stage (Mon)
  - Microsoft.OpenApi 2.0.0 (my dependency): 1 HIGH, found twice (/app + leftover /src/bin) → fix version (Tue)