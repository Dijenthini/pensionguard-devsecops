# PensionGuard - DevSecOps Pipeline Project

## 📋 Project Overview

- **Application:** OWASP NodeGoat
- **Working Title:** PensionGuard
- **Module:** IE3142 - DevOps Security
- **Team:** 4 Members

## 🎯 Project Goal

1. Identify 4 vulnerabilities
2. Demonstrate exploits working
3. Apply secure coding fixes
4. Build a CI/CD pipeline with 4 security gates

## 🛠️ Tech Stack

| Component | Technology |
|-----------|------------|
| Runtime | Node.js 12 |
| Framework | Express 4.x |
| Database | MongoDB 4.4 |
| Containerization | Docker & Docker Compose |
| CI/CD | GitHub Actions |

## 🚀 Setup Instructions

### Prerequisites

- Docker Desktop
- Git
- Visual Studio Code

### Step 1: Clone Repository

```bash
git clone https://github.com/Dijenthini/pensionguard-devsecops.git
cd pensionguard-devsecops
```

### Step 2: Start Application

```bash
docker compose up --build -d
```

### Step 3: Verify Running

```bash
docker compose ps
```

### Step 4: Access Application

- Web App: `http://localhost:4000`
- Tutorial: `http://localhost:4000/tutorial`

## 🔑 Test Accounts

| Username | Password | Role |
|----------|----------|------|
| `admin` | `Admin_123` | Administrator |
| `user1` | `User1_123` | Regular User |
| `user2` | `User2_123` | Regular User |

> **Note:** These are educational seed accounts for the local test environment only. They are not production credentials.

## 🧪 Security Tools

| Tool | Purpose | Stage |
|------|---------|-------|
| Semgrep | SAST | Code |
| npm audit | SCA | Dependencies |
| Gitleaks | Secrets Scanning | Repository |
| Trivy | Container Scanning | Container Image |

## 📁 Repository Structure

```
pensionguard-devsecops/
├── .github/workflows/       # CI/CD pipeline
├── app/                     # Application source
├── ethical-clearance/       # Ethical Clearance Form (local only)
├── docs/                    # Documentation & evidence
│   ├── evidence/
│   │   ├── baseline/       # Baseline evidence
│   │   ├── exploits/       # Exploit demonstrations
│   │   ├── secured/        # Fixed evidence
│   │   └── pipeline/       # Pipeline evidence
│   ├── threat-model/       # STRIDE threat model
│   └── reports/            # Scan reports
├── Dockerfile
├── docker-compose.yml
└── README.md
```

## 🔒 Security Scorecard

| Metric | Value |
|--------|-------|
| Vulnerabilities Fixed | 4 |
| Security Gates | 4 |
| Secrets Exposed | 0 |

## 📚 References

- OWASP NodeGoat: https://github.com/OWASP/NodeGoat
- OWASP Top 10: https://owasp.org/Top10/
- Semgrep: https://semgrep.dev
- Trivy: https://github.com/aquasecurity/trivy
- Gitleaks: https://github.com/gitleaks/gitleaks