# System Overview - PensionGuard
# Date: 2026-09-08

## Application
- **Name:** OWASP NodeGoat
- **Working Title:** PensionGuard
- **Purpose:** Retirement savings management
- **Tech Stack:** Node.js, Express, MongoDB

## Components
| Component | Technology | Port |
|-----------|------------|------|
| User Browser | HTML/CSS/JS | - |
| Web App | Node.js/Express | 4000 |
| Database | MongoDB 4.4 | 27017 |

## Security Features
- Non-root container
- Database isolated
- Resource limits
- CI/CD security scanning

## Test Accounts
| Username | Password | Role |
|----------|----------|------|
| admin | Admin_123 | Admin |
| user1 | User1_123 | User |
| user2 | User2_123 | User |