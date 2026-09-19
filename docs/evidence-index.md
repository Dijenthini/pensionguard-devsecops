# Evidence Index - PensionGuard
# Date: 2026-09-14
# Member 4 - Pipeline/Evidence Lead

## Evidence Files

### Baseline Evidence (Day 1)

| ID | File | What It Shows |
|----|------|---------------|
| EVID-01 | `baseline/EVID-01-login-page.png` | NodeGoat login page |
| EVID-02 | `baseline/EVID-02-admin-dashboard.png` | Admin dashboard |
| EVID-03 | `baseline/EVID-03-tutorial-page.png` | Tutorial page |
| EVID-04 | `baseline/EVID-04-docker-ps.png` | Docker containers running |
| EVID-05 | `baseline/EVID-05-vuln1-eval.png` | eval vulnerability |
| EVID-06 | `baseline/EVID-06-vuln2-xss.png` | XSS vulnerability |
| EVID-07 | `baseline/EVID-07-vuln3-idor.png` | IDOR vulnerability |
| EVID-08 | `baseline/EVID-08-vuln4-nosql.png` | NoSQL vulnerability |
| EVID-09 | `baseline/EVID-09-git-clean.png` | Clean git status |
| EVID-10 | `baseline/EVID-10-baseline-tag.png` | baseline-vulnerable tag |
| EVID-11 | `baseline/EVID-11-sast-baseline.png` | Baseline SAST scan |
| EVID-12 | `baseline/EVID-12-architecture-diagram.png` | System architecture |

### Exploit Evidence (Day 2)

| ID | File | What It Shows |
|----|------|---------------|
| EVID-13 | `exploits/EVID-13-vuln1-eval-exploit.png` | eval exploit working |
| EVID-14 | `exploits/EVID-14-vuln2-xss-exploit.png` | XSS exploit working |
| EVID-15 | `exploits/EVID-15-vuln3-idor-exploit.png` | IDOR exploit working |
| EVID-17 | `exploits/EVID-17-vuln5-benefits-access.png` | Benefits access exploit |

### Fix Evidence (Day 3)

| ID | File | What It Shows |
|----|------|---------------|
| EVID-18 | `secured/EVID-18-vuln1-fix.png` | eval fixed |
| EVID-19 | `secured/EVID-19-vuln2-fix.png` | XSS fixed |
| EVID-20 | `secured/EVID-20-vuln3-fix.png` | IDOR fixed |
| EVID-21 | `secured/EVID-21-vuln4-fix.png` | Benefits access denied |
| EVID-22 | `secured/EVID-22-sast-after.png` | SAST comparison |

### Pipeline Evidence (Day 2-3)

| ID | File | What It Shows |
|----|------|---------------|
| EVID-23 | `pipeline/EVID-23-pipeline-basic.png` | Basic pipeline |
| EVID-24 | `pipeline/EVID-24-pipeline-red-run.png` | Pipeline FAILED |
| EVID-24b | `pipeline/EVID-24b-sast-failure-details.png` | SAST failure details |
| EVID-25 | `pipeline/EVID-25-pipeline-green-run.png` | Pipeline PASSED |

## Traceability Matrix

| Threat | Risk | Vulnerability | Control | Code Location | Evidence |
|--------|------|---------------|---------|---------------|----------|
| T2 | MEDIUM | Benefits Access | RBAC | `app/routes/benefits.js` | EVID-21 |
| T3 | MEDIUM | Stored XSS | URL validation | `app/routes/profile.js` | EVID-19 |
| T5 | HIGH | IDOR | Session auth | `app/routes/allocations.js` | EVID-20 |
| T8 | HIGH | eval Injection | Numeric validation | `app/routes/contributions.js` | EVID-18 |

## Total Evidence Files

| Category | Count |
|----------|-------|
| Baseline | 12 |
| Exploits | 4 |
| Fixes | 5 |
| Pipeline | 4 |
| **Total** | **25** |