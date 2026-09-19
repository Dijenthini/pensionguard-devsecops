# System Overview - Report
# Date: 2026-09-19
# Member 1 - System/Container Lead

## 1. Application Selection

**Application:** OWASP NodeGoat
**Working Title:** PensionGuard
**Source:** https://github.com/OWASP/NodeGoat

**Reason for Selection:**
- Intentionally vulnerable - ideal for security testing
- Two-component architecture (Web App + Database)
- Fully containerizable with Docker
- Realistic retirement management system
- Vulnerabilities mapped to OWASP Top 10

---

## 2. Technology Stack

| Layer | Technology | Version | Purpose |
|-------|------------|---------|---------|
| Runtime | Node.js | 12 | Server-side JavaScript |
| Framework | Express | 4.x | Web application framework |
| Template Engine | EJS | 3.x | HTML rendering |
| Database | MongoDB | 4.4 | Data storage |
| Containerization | Docker | Latest | Application packaging |
| Orchestration | Docker Compose | Latest | Multi-container management |
| CI/CD | GitHub Actions | Latest | Automated pipeline |

---

## 3. System Architecture

The system consists of three main components:

### 3.1 User Browser
- **Role:** Client-side interface
- **Protocol:** HTTP/HTTPS
- **Interaction:** Login, data viewing, form submission

### 3.2 Web Application (NodeGoat)
- **Technology:** Node.js + Express
- **Port:** 4000
- **Security:** Non-root user, resource limits
- **Key Features:**
  - User authentication
  - Profile management
  - Retirement allocations
  - Contribution tracking
  - Benefit management

### 3.3 Database (MongoDB)
- **Technology:** MongoDB 4.4
- **Port:** 27017 (internal)
- **Security:** Not exposed to host
- **Collections:** users, profiles, allocations, contributions, benefits

---

## 4. Trust Boundaries

### Boundary 1: Internet → Internal Network
- **Separation:** User browser vs application server
- **Security Controls:** Input validation, output encoding, authentication
- **What crosses:** HTTP requests (login, profile, allocations)

### Boundary 2: Application → Database
- **Separation:** Web app vs database server
- **Security Controls:** Parameterized queries, sanitization
- **What crosses:** MongoDB queries

---

## 5. Data Flows

| Source | Target | Data Type | Protocol | Port |
|--------|--------|-----------|----------|------|
| Browser | Web App | Login, profile, allocations | HTTP | 4000 |
| Web App | Browser | HTML, JSON responses | HTTP | 4000 |
| Web App | Database | Queries (find, update, insert) | MongoDB | 27017 |
| Database | Web App | User data, allocations | MongoDB | 27017 |

---

## 6. Containerization Approach

### 6.1 Dockerfile

```dockerfile
FROM node:12-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
USER node
EXPOSE 4000
CMD ["node", "server.js"]
```

### 6.2 docker-compose.yml

```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "4000:4000"
    environment:
      - SESSION_SECRET=${SESSION_SECRET}
      - DB_PASSWORD=${DB_PASSWORD}
    depends_on:
      - mongo
  mongo:
    image: mongo:4.4
    ports:
      - "27017"
```

### 6.3 Container Hardening

| Control | Implementation |
|---------|----------------|
| Non-root user | `USER node` |
| Minimal base image | `node:12-alpine` |
| Database isolation | MongoDB internal only |
| Resource limits | CPU: 0.5, Memory: 512MB |
| Network isolation | Docker internal network |
| No secrets in image | Environment variables |

---

## 7. Security Posture

### 7.1 Vulnerabilities Fixed

| Vulnerability | Status | Evidence |
|---------------|--------|----------|
| eval Injection | ✅ Fixed | EVID-18 |
| Stored XSS | ✅ Fixed | EVID-19 |
| IDOR | ✅ Fixed | EVID-20 |
| Benefits Access | ✅ Fixed | EVID-21 |

### 7.2 Security Controls Applied

| Control Type | Control | Status |
|--------------|---------|--------|
| Preventive | Input validation | ✅ Applied |
| Preventive | Authentication | ✅ Applied |
| Preventive | Authorization (RBAC) | ✅ Applied |
| Preventive | Output encoding | ✅ Applied |
| Preventive | Non-root container | ✅ Applied |
| Detective | SAST scanning | ✅ Applied |
| Detective | Secrets scanning | ✅ Applied |
| Detective | Container scanning | ✅ Applied |

---

## 8. System Summary

**PensionGuard** is a containerized retirement management application built with Node.js, Express, and MongoDB. It uses Docker Compose for orchestration and GitHub Actions for CI/CD.

**Key Characteristics:**

| Feature | Value |
|---------|-------|
| Components | 3 (Browser, Web App, MongoDB) |
| Trust Boundaries | 2 |
| Vulnerabilities Fixed | 4 |
| Security Gates in Pipeline | 5 |
| Secrets Exposed | 0 |

The application is fully reproducible with a single command:
```bash
docker compose up --build -d
```

### 8.1 Reproducibility

| Check | Status |
|-------|--------|
| Fresh clone works | ✅ Verified |
| One-command Docker start | ✅ Verified |
| All 4 vulnerabilities fixed | ✅ Verified |
| Pipeline passes on master | ✅ Verified |

---

## 9. References

- OWASP NodeGoat: https://github.com/OWASP/NodeGoat
- Docker Documentation: https://docs.docker.com
- OWASP Top 10: https://owasp.org/Top10/
- MongoDB Documentation: https://docs.mongodb.com