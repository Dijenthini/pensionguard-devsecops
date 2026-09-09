# Network Diagram - PensionGuard
# Date: 2026-09-09

## Network Architecture
┌─────────────────────────────────────────────────────────────┐
│ PUBLIC INTERNET │
│ │
│ ┌─────────────────────────────────────────────────────┐ │
│ │ User Browser │ │
│ │ HTTP/HTTPS (Port 4000) │ │
│ └─────────────────────────────────────────────────────┘ │
│ │ │
│ ▼ │
│ ═══════════════════════════════════════════════════════ │
│ TRUST BOUNDARY │
│ ═══════════════════════════════════════════════════════ │
│ │ │
│ ▼ │
└─────────────────────────────────────────────────────────────┘
│
▼
┌─────────────────────────────────────────────────────────────┐
│ DOCKER INTERNAL NETWORK │
│ (pensionguard-network) │
│ │
│ ┌─────────────────────────────────────────────────────┐ │
│ │ NodeGoat Web Container │ │
│ │ (Node.js / Express) │ │
│ │ │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ Port Mapping: 4000:4000 │ │ │
│ │ │ User: node (non-root) │ │ │
│ │ │ Network: pensionguard-network │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────────┘ │
│ │ │
│ ▼ │
│ ┌─────────────────────────────────────────────────────┐ │
│ │ Database Connection │ │
│ │ MongoDB Protocol (Port 27017) │ │
│ │ Internal Only (Not Exposed to Host) │ │
│ └─────────────────────────────────────────────────────┘ │
│ │ │
│ ▼ │
│ ┌─────────────────────────────────────────────────────┐ │
│ │ MongoDB Container │ │
│ │ (MongoDB 4.4) │ │
│ │ │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ Port: 27017 (Internal Only) │ │ │
│ │ │ Network: pensionguard-network │ │ │
│ │ │ No Host Port Exposure │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────────┘ │
│ │
└─────────────────────────────────────────────────────────────┘



## Network Security Controls

| Control | Implementation | Purpose |
|---------|----------------|---------|
| Internal Network | Docker network (pensionguard-network) | Isolates containers |
| Port Restriction | Only port 4000 exposed | Minimizes attack surface |
| Database Isolation | MongoDB port 27017 internal | Prevents external database access |
| Non-Root User | `USER node` in Dockerfile | Limits container privileges |
| Resource Limits | CPU: 0.5 cores, Memory: 512MB | Prevents resource exhaustion |

## Firewall Rules

| Source | Destination | Port | Protocol | Allowed |
|--------|-------------|------|----------|---------|
| Browser | Web Container | 4000 | HTTP | ✅ Yes |
| Browser | Web Container | 27017 | MongoDB | ❌ No |
| Web Container | MongoDB | 27017 | MongoDB | ✅ Yes |
| External | MongoDB | Any | Any | ❌ No |

## Summary

- **Single entry point:** Port 4000
- **Database not exposed:** Internal only
- **Network isolation:** Docker internal network
- **Resource limits:** Applied to web container