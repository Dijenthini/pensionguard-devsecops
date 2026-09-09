# Risk Assessment - NodeGoat
# Date: 2026-09-07
# Created by: Member 2
 
## 5x5 Likelihood/Impact Matrix
 
| Impact/Likelihood | 1 - Rare | 2 - Unlikely | 3 - Possible | 4 - Likely | 5 - Almost Certain |
|-------------------|----------|--------------|--------------|------------|---------------------|
| 5 - Severe | 5 | 10 | 15 | 20 | 25 |
| 4 - Major | 4 | 8 | 12 | 16 | 20 |
| 3 - Moderate | 3 | 6 | 9 | 12 | 15 |
| 2 - Minor | 2 | 4 | 6 | 8 | 10 |
| 1 - Negligible | 1 | 2 | 3 | 4 | 5 |
 
## Treat Risk Scores

| Threat | Likelihood | Impact | Score | Level |
|--------|------------|--------|-------|-------|
| T1 - Weak Authentication | 3 | 4 | 12 | HIGH |
| T2 - Broken Access Control (Benefits) | 3 | 3 | 9 | MEDIUM |
| T3 - Stored XSS | 3 | 3 | 9 | MEDIUM |
| T4 - No Audit Logs | 3 | 2 | 6 | MEDIUM |
| T5 - IDOR | 4 | 4 | 16 | HIGH |
| T6 - Error Disclosure | 2 | 2 | 4 | MEDIUM |
| T7 - DoS | 2 | 3 | 6 | MEDIUM |
| T8 - IDOR (Allocations) | 4 | 4 | 16 | HIGH |
 
## Risk Level Classification
 
| Level | Score Range | Action Required |
|-------|-------------|-----------------|
| **CRITICAL** | 20-25 | Immediate action required |
| **HIGH** | 12-19 | Priority action required |
| **MEDIUM** | 5-11 | Action planned in next iteration |
| **LOW** | 1-4 | Acceptable risk, monitor |
 
## Summary
| Level | Count | Threats |
|-------|-------|---------|
| CRITICAL (20-25) | 0 | None |
| HIGH (12-19) | 3 | T1, T5, T8 |
| MEDIUM (5-11) | 4 | T2, T3, T4, T7 |
| LOW (1-4) | 1 | T6 |