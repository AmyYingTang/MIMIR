# Security Design Checklist

> **Purpose**: Item-by-item security verification during system design  
> **When to Use**: Phase 3 system design, pre-implementation review, pre-launch review  
> **Related**: `enterprise-concerns.md` covers high-level security concerns; this checklist covers specific implementation checks

---

## 1. Authentication & Session Management

| Check Item | Priority | Implementation Notes | ✓ |
|------------|:--------:|----------------------|:-:|
| Auth scheme selected and documented | P0 | JWT / Session / OAuth with rationale | ⬜ |
| Access Token expiry is reasonable | P0 | Recommended 15-30 minutes | ⬜ |
| Refresh Token stored securely | P0 | httpOnly Cookie, not localStorage | ⬜ |
| Refresh Token expiry is reasonable | P1 | Recommended 7-30 days | ⬜ |
| Token blacklist/revocation mechanism | P1 | Invalidate on logout, password change | ⬜ |
| Login failure lockout | P1 | Lock after N consecutive failures for M minutes | ⬜ |
| Concurrent login policy defined | P2 | Single device / multi-device / unlimited | ⬜ |

---

## 2. Password Storage

| Check Item | Priority | Implementation Notes | ✓ |
|------------|:--------:|----------------------|:-:|
| Passwords hashed with secure algorithm | P0 | bcrypt or Argon2id. **Never** MD5/SHA1 | ⬜ |
| Automatic salting | P0 | bcrypt has built-in salt; configure for Argon2 | ⬜ |
| Password strength validation | P1 | Minimum 8 chars, letters and numbers | ⬜ |
| Passwords never appear in logs | P0 | Log sanitization | ⬜ |
| Passwords never appear in API responses | P0 | Exclude during response serialization | ⬜ |

---

## 3. Sensitive Data Encryption

| Check Item | Priority | Implementation Notes | ✓ |
|------------|:--------:|----------------------|:-:|
| Sensitive data identified and labeled | P0 | List which fields are sensitive | ⬜ |
| Sensitive fields encrypted at rest | P0 | AES-256-GCM | ⬜ |
| Encryption keys not in codebase | P0 | Environment variables or key management service | ⬜ |
| Key rotation plan | P2 | Plan for key expiry and renewal | ⬜ |
| Database backups also encrypted | P1 | Backups not stored in plaintext | ⬜ |

---

## 4. Transport Security

| Check Item | Priority | Implementation Notes | ✓ |
|------------|:--------:|----------------------|:-:|
| HTTPS (TLS 1.2+) | P0 | All production communication encrypted | ⬜ |
| HTTP → HTTPS redirect | P1 | No plaintext access allowed | ⬜ |
| Security response headers configured | P1 | HSTS, X-Content-Type-Options, X-Frame-Options | ⬜ |
| No sensitive params in URLs | P1 | Tokens, passwords go in body or headers | ⬜ |

---

## 5. Input Validation & Injection Prevention

| Check Item | Priority | Implementation Notes | ✓ |
|------------|:--------:|----------------------|:-:|
| All user input validated | P0 | Length, format, range | ⬜ |
| SQL injection protection | P0 | ORM parameterized queries, no string concatenation | ⬜ |
| XSS protection | P1 | Frontend output escaping, CSP header | ⬜ |
| CSRF protection | P1 | Token-based (JWT inherent) or SameSite Cookie | ⬜ |
| File upload validation | P1 | Type, size, content checks | ⬜ |
| Path traversal protection | P1 | No user input in file path construction | ⬜ |

---

## 6. Authorization

| Check Item | Priority | Implementation Notes | ✓ |
|------------|:--------:|----------------------|:-:|
| Every API endpoint has permission check | P0 | No gaps | ⬜ |
| Vertical privilege escalation prevented | P0 | Regular users can't access admin endpoints | ⬜ |
| Horizontal privilege escalation prevented | P0 | Users can only access their own data | ⬜ |
| Permission checks run server-side | P0 | Frontend permissions are for UI display only | ⬜ |
| Default deny principle | P1 | Deny any action not explicitly authorized | ⬜ |

---

## 7. CORS Policy

| Check Item | Priority | Implementation Notes | ✓ |
|------------|:--------:|----------------------|:-:|
| CORS whitelist configured | P1 | Only allow known frontend domains | ⬜ |
| No `*` wildcard in production | P1 | Dev environment can be relaxed | ⬜ |
| Allowed HTTP methods restricted | P2 | Only open required methods | ⬜ |
| Credentials mode correctly configured | P1 | Aligned with Cookie auth scheme | ⬜ |

---

## 8. Logging Security

| Check Item | Priority | Implementation Notes | ✓ |
|------------|:--------:|----------------------|:-:|
| No passwords in logs | P0 | Request log sanitization | ⬜ |
| No full tokens in logs | P0 | Log at most first 8 chars | ⬜ |
| No sensitive business data in logs | P1 | ID numbers, bank cards, etc. | ⬜ |
| Error responses don't expose stack traces | P1 | No technical details to frontend | ⬜ |
| Sensitive operations have audit logs | P1 | Login, permission changes, data deletion | ⬜ |

---

## 9. Dependency Security

| Check Item | Priority | Implementation Notes | ✓ |
|------------|:--------:|----------------------|:-:|
| Dependency versions locked | P1 | Lock files committed to repo | ⬜ |
| Known vulnerability scanning | P2 | pip-audit / npm audit | ⬜ |
| Regular dependency updates | P2 | Establish update schedule | ⬜ |

---

## Usage Guide

1. **System design phase**: Check each item, document specific implementation approach
2. **Pre-implementation**: Confirm all P0 items have a solution
3. **Pre-launch review**: All items checked and confirmed
4. **Periodic review**: Check for new security requirements as project evolves

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v1.0 | 2025-02-05 | Initial version |
