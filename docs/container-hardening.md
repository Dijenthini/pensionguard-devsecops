# Container Hardening - PensionGuard
# Date: 2026-09-08
# Created by: Member 1

## Overview
This document describes the security hardening measures applied to the Docker containers in the PensionGuard project.

---

## 1. Base Image Security

| Aspect | Configuration | Why It Matters |
|--------|---------------|----------------|
| **Base Image** | `node:12-alpine` | Alpine is minimal, reducing attack surface |
| **Official Image** | Yes, from Docker Hub | Trusted source, regularly updated |
| **Image Size** | Small (~50MB) | Fewer packages = fewer vulnerabilities |

**Dockerfile Reference:**
```dockerfile
FROM node:12-alpine