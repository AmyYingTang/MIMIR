# Enterprise Concerns Checklist

> **Purpose**: Ensure enterprise projects don't overlook critical considerations  
> **When to Use**: Requirements analysis, system design, pre-launch review

---

## 1. Authentication & Identity Management

| Checklist Item | Priority | Notes |
|----------------|:--------:|-------|
| Passwords stored using bcrypt/Argon2 | P0 | Never store plaintext |
| Password strength validation | P1 | Minimum 8 chars, alphanumeric |
| Login failure lockout | P1 | 5 failures → 15 min lockout |
| JWT reasonable expiration | P1 | Access 30min, Refresh 7 days |
| Refresh Token secure storage | P1 | httpOnly Cookie |
| Auth layer abstraction (SSO prep) | P2 | For future enterprise SSO |
| Concurrent login control | P2 | Optional: single/multi device |
| MFA multi-factor auth prep | P3 | Reserve extension point in architecture |

---

## 2. Permission Control

| Checklist Item | Priority | Notes |
|----------------|:--------:|-------|
| Role permission model defined | P0 | RBAC or ABAC |
| Every API has permission check | P0 | No gaps |
| Data privilege escalation check | P0 | Users can only access their own data |
| Admin permissions configurable | P1 | Different admins different permissions |
| Permission change auditing | P2 | Record who assigned what to whom |
| Fine-grained permissions (field-level) | P3 | If needed |

---

## 3. Data Security

| Checklist Item | Priority | Notes |
|----------------|:--------:|-------|
| Sensitive data identified and marked | P0 | Clarify what is sensitive |
| Sensitive data encrypted at rest | P0 | AES-256 |
| Transport encryption HTTPS | P0 | TLS 1.2+ |
| Log masking | P1 | Don't log passwords, tokens |
| API response masking | P1 | Phone displays as 138****1234 |
| Database backup encryption | P1 | Backup files also encrypted |
| Key management | P1 | Keys not in codebase, use env vars |

---

## 4. Audit & Compliance

| Checklist Item | Priority | Notes |
|----------------|:--------:|-------|
| Operation logs | P1 | Who did what when |
| Sensitive operation auditing | P1 | Login, permission changes, data deletion, etc. |
| Log retention policy | P2 | How long to keep, how to clean |
| Data retention policy | P2 | User data retention period |
| GDPR/Privacy compliance | P2 | If involves personal information |
| Data export feature | P3 | Users have right to export their data |

---

## 5. Availability & Performance

| Checklist Item | Priority | Notes |
|----------------|:--------:|-------|
| Error handling with friendly messages | P0 | Don't expose technical details to users |
| Timeout control | P1 | Avoid infinite waits |
| API rate limiting | P1 | Prevent abuse |
| Health check endpoint | P1 | `/health` for monitoring |
| Caching strategy | P2 | Cache hot data |
| Load balancing prep | P2 | Architecture supports multi-instance |
| Async task queue | P2 | Long-running tasks processed async |

---

## 6. Maintainability

| Checklist Item | Priority | Notes |
|----------------|:--------:|-------|
| Database migrations versioned | P0 | Use Alembic/Flyway etc. |
| Configuration separated from code | P0 | Environment variables/config files |
| External service abstraction | P1 | Can switch email/storage services |
| Feature flags | P1 | Can dynamically toggle features |
| API documentation | P1 | Swagger/OpenAPI |
| Deployment documentation | P1 | New person can deploy following docs |
| Code standards | P1 | Linter + Formatter |

---

## 7. Monitoring & Operations

| Checklist Item | Priority | Notes |
|----------------|:--------:|-------|
| Log collection | P1 | Centralized logging |
| Error tracking | P1 | Sentry etc. |
| Performance monitoring | P2 | APM |
| Alert configuration | P2 | Notifications on anomalies |
| Database backup | P1 | Regular automatic backup |
| Disaster recovery plan | P2 | Backup recovery testing |
| Rollback plan | P1 | Can rollback on failed deployment |

---

## 8. Extensibility Prep

| Checklist Item | Priority | Notes |
|----------------|:--------:|-------|
| Multi-language i18n prep | P3 | If needed |
| Multi-tenant prep | P3 | If needed |
| Open API prep | P3 | Third-party integration |
| Mobile API compatibility | P3 | If need App |

---

## Usage Instructions

1. **Requirements Analysis Phase**: Go through this checklist, determine which items this project needs
2. **System Design Phase**: Design specific solutions for needed items
3. **Pre-Launch Review**: Check if all marked items are implemented
4. **Regular Review**: Periodically review if supplements are needed during project evolution

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.0 | 2025-01-27 | Initial version |
