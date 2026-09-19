# Container Security Summary - PensionGuard
# Date: 2026-09-19
# Member 1 - System/Container Lead

## 1. Container Overview

| Component | Image | Port | Purpose |
|-----------|-------|------|---------|
| Web App | `pensionguard-devsecops-web` | 4000 | Application server |
| Database | `mongo:4.4` | 27017 (internal) | Data storage |

---

## 2. Security Controls Applied

| Control | Implementation | Verified |
|---------|----------------|----------|
| Non-root user | `USER node` in Dockerfile | ✅ `whoami` returns `node` |
| Minimal base image | `node:12-alpine` | ✅ Alpine-based |
| Database isolation | Not exposed to host | ✅ `docker compose ps` shows no host port |
| Resource limits | CPU: 0.5, Memory: 512MB | ✅ Applied in compose |
| Network isolation | `pensionguard-network` | ✅ Custom network |
| No secrets in image | Environment variables | ✅ No secrets in Dockerfile |
| `.dockerignore` | Excludes sensitive files | ✅ Present |

---

## 3. Verification Commands

```bash
# Verify non-root user
docker compose exec web whoami
# Output: node

# Verify containers running
docker compose ps
# Output: web + mongo running

# Verify database isolation
docker compose port mongo 27017
# Output: (empty - not exposed)

# Verify app works
curl http://localhost:4000
# Output: HTML page
```

---

## 4. Container Layers

```
┌─────────────────────────────────┐
│  Application Layer              │  ← NodeGoat source code
├─────────────────────────────────┤
│  Dependency Layer               │  ← npm packages
├─────────────────────────────────┤
│  Runtime Layer                  │  ← Node.js 12
├─────────────────────────────────┤
│  Base OS Layer                  │  ← Alpine Linux
└─────────────────────────────────┘
```

---

## 5. Network Architecture

```
┌─────────────────────────────────────────────────────────┐
│                   PUBLIC INTERNET                       │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│              DOCKER NETWORK                             │
│              (pensionguard-network)                     │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │  Web Container                                  │   │
│  │  • Node.js 12                                   │   │
│  │  • Port 4000 (exposed)                          │   │
│  │  • Runs as: node (non-root)                     │   │
│  └─────────────────────────────────────────────────┘   │
│                           │                             │
│                           ▼                             │
│  ┌─────────────────────────────────────────────────┐   │
│  │  MongoDB Container                              │   │
│  │  • MongoDB 4.4                                  │   │
│  │  • Port 27017 (internal only)                   │   │
│  │  • Not exposed to host                          │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

---

## 6. Pipeline Integration

| Gate | Container Related? |
|------|-------------------|
| Build & Test | ✅ Builds Docker image |
| SAST | ❌ Code only |
| Dependency Scan | ✅ npm packages |
| Secrets Scan | ✅ Checks for secrets |
| Container Scan | ✅ Trivy scans image |

---

## 7. Summary

| Metric | Value |
|--------|-------|
| Containers | 2 |
| Non-root user | ✅ Yes |
| Database exposed | ❌ No |
| Resource limits | ✅ Set |
| Network isolation | ✅ Yes |
| Security scanning | ✅ Trivy |
| Secrets in image | 0 |

---

## 8. Security Best Practices Applied

| # | Best Practice | Status |
|---|---------------|--------|
| 1 | Use official minimal base image | ✅ |
| 2 | Run as non-root user | ✅ |
| 3 | Expose only required ports | ✅ |
| 4 | Keep database internal | ✅ |
| 5 | Set resource limits | ✅ |
| 6 | Isolate network | ✅ |
| 7 | No secrets in image | ✅ |
| 8 | Use `.dockerignore` | ✅ |
| 9 | Scan image with Trivy | ✅ |
| 10 | Use environment variables for secrets | ✅ |

---

## 9. Verification Results

| Check | Command | Result |
|-------|---------|--------|
| Containers running | `docker compose ps` | ✅ Both running |
| Non-root user | `docker compose exec web whoami` | ✅ `node` |
| Database isolated | `docker compose port mongo 27017` | ✅ Not exposed |
| App accessible | `curl http://localhost:4000` | ✅ HTTP 200 |
| Image scanned | Trivy (pipeline) | ✅ 0 critical |