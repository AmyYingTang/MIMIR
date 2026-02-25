# Production Readiness Checklist

> **Purpose**: Final checks before deploying to production  
> **When to Use**: Before production deployment  
> **Principle**: All P0 must pass, P1 strongly recommended, P2 project-dependent

---

## 1. Security

| Check Item | Priority | Notes | ✓ |
|------------|:--------:|-------|:-:|
| HTTPS configured | P0 | TLS 1.2+, valid certificate | ⬜ |
| Security response headers configured | P1 | HSTS, X-Content-Type-Options, X-Frame-Options | ⬜ |
| No secrets/passwords in codebase | P0 | All injected via environment variables | ⬜ |
| Default passwords changed | P0 | Database, admin accounts, third-party services | ⬜ |
| Dependency vulnerabilities scanned | P1 | pip-audit / npm audit, no critical vulnerabilities | ⬜ |
| Debug mode disabled | P0 | No debug info exposed in production | ⬜ |
| CORS whitelist tightened | P1 | No `*` wildcard | ⬜ |
| Sensitive data encryption enabled | P0 | Encryption keys configured | ⬜ |

---

## 2. Reliability

| Check Item | Priority | Notes | ✓ |
|------------|:--------:|-------|:-:|
| Error handling is complete | P0 | Users see friendly messages, not stack traces | ⬜ |
| Health check endpoint available | P1 | `GET /health` or `/api/health` returns 200 | ⬜ |
| Database backup strategy established | P1 | Automated backups, clear frequency and retention | ⬜ |
| Backup restore tested | P1 | Successfully restored at least once | ⬜ |
| Database connection pool configured | P1 | Max connections, timeout settings | ⬜ |
| Timeout controls configured | P1 | API request timeout, DB query timeout | ⬜ |
| Disaster recovery plan documented | P2 | Know how to recover when things go wrong | ⬜ |

---

## 3. Observability

| Check Item | Priority | Notes | ✓ |
|------------|:--------:|-------|:-:|
| Logging works correctly | P0 | Production uses structured format (JSON) | ⬜ |
| Log level is correct | P1 | Production at INFO or WARNING, not DEBUG | ⬜ |
| Log sanitization is active | P1 | Passwords, tokens don't appear in logs | ⬜ |
| Error tracking configured | P1 | Sentry or similar tool | ⬜ |
| Key business metrics observable | P2 | Request volume, error rate, response time | ⬜ |
| Alert rules configured | P2 | Notify on abnormal error rates, service down | ⬜ |
| Performance monitoring configured | P2 | APM or basic metrics collection | ⬜ |

---

## 4. Database

| Check Item | Priority | Notes | ✓ |
|------------|:--------:|-------|:-:|
| Database migrations executed | P0 | Alembic/Flyway version matches code | ⬜ |
| Initial data seeded | P0 | Admin accounts, enum data, seed data | ⬜ |
| Indexes created | P1 | Frequently queried fields indexed | ⬜ |
| Character set configured correctly | P1 | UTF-8 / utf8mb4 | ⬜ |
| Database user privileges minimized | P1 | Application user is not root | ⬜ |

---

## 5. Deployment & Infrastructure

| Check Item | Priority | Notes | ✓ |
|------------|:--------:|-------|:-:|
| Deployment documentation written | P0 | A newcomer can deploy following the docs | ⬜ |
| Environment variables configured | P0 | All required variables set | ⬜ |
| Container images built and tested | P0 | docker compose up starts normally | ⬜ |
| Port mappings correct | P0 | Inter-service and external access both work | ⬜ |
| Adequate disk space | P1 | For logs, uploads, database storage | ⬜ |
| Rollback plan prepared | P1 | Can quickly revert on failed deployment | ⬜ |
| Domain/DNS configured | P1 | If applicable | ⬜ |
| SSL certificate auto-renewal | P2 | No service interruption from expired certs | ⬜ |

---

## 6. Functional Verification

| Check Item | Priority | Notes | ✓ |
|------------|:--------:|-------|:-:|
| Core business flow tested end-to-end | P0 | Full flow tested in production environment | ⬜ |
| All role permissions verified | P0 | Each role's access permissions are correct | ⬜ |
| Email/notification features tested | P1 | If applicable | ⬜ |
| File upload/download tested | P1 | If applicable | ⬜ |
| Async tasks running correctly | P1 | If using Celery or similar task queue | ⬜ |

---

## 7. Documentation & Operations

| Check Item | Priority | Notes | ✓ |
|------------|:--------:|-------|:-:|
| Operations manual written | P1 | Daily maintenance operations guide | ⬜ |
| Troubleshooting guide | P2 | Known issues and solutions | ⬜ |
| Contact list / on-call roster established | P2 | Know who to reach when things break | ⬜ |
| User documentation delivered | P1 | User manual, admin manual | ⬜ |

---

## Usage Guide

1. **1-2 days before launch**: Check each item, record status
2. **All P0 must pass** before going live
3. **P1 items not passing** need a clear fix plan with timeline
4. **P2 items not passing** recorded in open-issues for future iterations
5. **Check results archived** as evidence for launch approval

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v1.0 | 2025-02-05 | Initial version |
