# PensionGuard - System Architecture
# Date: 2026-09-07
# Created by: Member 1

## Application Overview
- **Name:** OWASP NodeGoat
- **Working Title:** PensionGuard
- **Purpose:** Retirement savings management platform
- **Tech Stack:** Node.js, Express, MongoDB

## Components

### 1. User Browser
- **Role:** Client-side interface for users
- **Communication:** HTTP/HTTPS
- **Interaction:** Login, view data, submit forms

### 2. Web Application (NodeGoat Web)
- **Technology:** Node.js with Express framework
- **Port:** 4000 (exposed to host)
- **Purpose:** Serve web interface, handle user requests, business logic
- **Key Files:** `routes/`, `models/`, `controllers/`

### 3. Database (MongoDB)
- **Technology:** MongoDB 4.4
- **Port:** 27017 (internal, not exposed)
- **Purpose:** Store user data, profiles, allocations, contributions
- **Key Collections:** users, profiles, allocations, contributions

## Trust Boundaries

### Boundary 1: Internet → Internal Network
- **Separation:** User browser vs application server
- **Security Controls:** HTTPS, session management, authentication
- **What crosses:** HTTP requests (login, profile, allocations)

### Boundary 2: Application → Database
- **Separation:** Web app vs database server
- **Security Controls:** Database authentication, query sanitization
- **What crosses:** MongoDB queries (find, update, insert)

## Data Flows

| Source | Target | Data Type | Protocol | Port |
|--------|--------|-----------|----------|------|
| Browser | Web App | Login credentials, profile data, allocations | HTTP | 4000 |
| Web App | Browser | HTML pages, JSON responses | HTTP | 4000 |
| Web App | Database | Queries (find, update, insert) | MongoDB | 27017 |
| Database | Web App | User data, allocations, profiles | MongoDB | 27017 |

## Network Configuration

| Service | Container | Host Port | Container Port |
|---------|-----------|-----------|----------------|
| Web App | web | 4000 | 4000 |
| Database | mongo | (none) | 27017 |

## Security Considerations
1. MongoDB is NOT exposed to the host - only accessible internally
2. Web app port 4000 is the only entry point
3. All user input crosses Trust Boundary 1 - must be validated
4. All database queries cross Trust Boundary 2 - must be sanitized