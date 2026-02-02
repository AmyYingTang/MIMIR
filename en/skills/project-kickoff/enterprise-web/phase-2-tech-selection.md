# Phase 2: Technology Selection

> **Phase Goal**: Determine tech stack, architecture style, deployment plan  
> **Deliverables**: Tech selection document (with rationale), architecture diagram  
> **Key Principle**: Give recommendations + rationale, not just list options

---

## 1. Selection Principles

### 1.1 Core Principles

| Principle | Description | Priority |
|-----------|-------------|:--------:|
| **Team Familiarity** | Tech team knows > newest coolest tech | ⭐⭐⭐⭐⭐ |
| **Tech Stack Consistency** | Align with company's existing tech stack | ⭐⭐⭐⭐ |
| **Community & Ecosystem** | Active community, rich docs, easy to hire | ⭐⭐⭐⭐ |
| **Meet Requirements** | Just enough to meet needs, don't over-engineer | ⭐⭐⭐⭐ |
| **Maintainability** | Low long-term maintenance cost | ⭐⭐⭐ |

### 1.2 Anti-Pattern Warnings

| Anti-Pattern | Symptoms | Consequences |
|--------------|----------|--------------|
| **Resume-Driven Development** | Choosing new tech to learn it | Low team efficiency, high project risk |
| **Over-Engineering** | Microservices for a 10-user system | Dev/ops cost explosion |
| **Tech Obsession** | "Must use XX because it's the best" | Ignoring team reality |
| **Following Trends** | "Big companies are all using XX" | Doesn't fit your scenario |

---

## 2. Decision Framework

### 2.1 Backend Language Decision Tree

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     Backend Language Decision Tree                           │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  Q1: What language is the team most familiar with?                          │
│  ├── Has clear preference ─────────────────────────► Use team's language    │
│  └── No particular preference / New team ──────────► Q2                     │
│                                                                             │
│  Q2: Does the project have special requirements?                            │
│  ├── Need AI/ML model integration ─────────────────► Python 🏆             │
│  ├── Need extreme performance (high concurrency) ──► Go / Rust             │
│  ├── Need to integrate with Java ecosystem ────────► Java                  │
│  ├── Unified frontend/backend language ────────────► Node.js (TypeScript)  │
│  └── No special requirements ──────────────────────► Q3                     │
│                                                                             │
│  Q3: Project scale and team situation?                                      │
│  ├── Small team + rapid iteration ─────────────────► Python / Node.js      │
│  ├── Medium/large team + enterprise project ───────► Java / C#             │
│  └── Balance dev efficiency + performance ─────────► Go                    │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 2.2 Backend Language Comparison Matrix

| Dimension | Python | Node.js | Java | Go |
|-----------|:------:|:-------:|:----:|:--:|
| **Dev Efficiency** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Runtime Performance** | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **AI/ML Ecosystem** | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| **Web Framework Maturity** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Hiring Difficulty** | Easy | Easy | Easy | Medium |
| **Best For** | API services, AI integration | I/O intensive, full-stack | Large enterprise systems | High concurrency services |

### 2.3 Backend Framework Recommendations

| Language | Recommended Framework | Use Case | Alternatives |
|----------|----------------------|----------|--------------|
| **Python** | FastAPI | API-first, modern async | Django (full-stack), Flask (lightweight) |
| **Node.js** | NestJS | Enterprise-grade, structured | Express (flexible), Koa (lightweight) |
| **Java** | Spring Boot | Enterprise standard | Quarkus (cloud-native) |
| **Go** | Gin | Lightweight, high-performance | Echo, Fiber |

---

### 2.4 Database Decision Tree

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        Database Decision Tree                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  Q1: What type of data structure?                                           │
│  ├── Structured data (tables, relations) ──────────► Relational DB (Q2)    │
│  ├── Document-type (JSON, nested structures) ──────► MongoDB               │
│  ├── Key-value / Cache ────────────────────────────► Redis                 │
│  ├── Graph relations ──────────────────────────────► Neo4j                 │
│  └── Time-series data ─────────────────────────────► InfluxDB / TimescaleDB│
│                                                                             │
│  Q2: Relational database selection                                          │
│  ├── Company already has MySQL ────────────────────► MySQL 🏆              │
│  ├── Company already has PostgreSQL ───────────────► PostgreSQL            │
│  ├── Need advanced features (JSON, full-text) ─────► PostgreSQL 🏆         │
│  ├── Cloud-native / Serverless ────────────────────► Cloud database        │
│  └── Simple project / Embedded ────────────────────► SQLite                │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 2.5 Database Comparison Matrix

| Dimension | PostgreSQL | MySQL | MongoDB |
|-----------|:----------:|:-----:|:-------:|
| **Feature Richness** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **JSON Support** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Performance** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Ops Difficulty** | Medium | Easy | Medium |
| **Adoption (Global)** | High | Very High | Medium |

---

### 2.6 Frontend Framework Decision Tree

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      Frontend Framework Decision Tree                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  Q1: What framework is the team familiar with?                              │
│  ├── Familiar with React ──────────────────────────► React                 │
│  ├── Familiar with Vue ────────────────────────────► Vue 3                 │
│  ├── Familiar with Angular ────────────────────────► Angular               │
│  └── None ─────────────────────────────────────────► Q2                    │
│                                                                             │
│  Q2: Project characteristics?                                               │
│  ├── Pursue flexibility, rich ecosystem ───────────► React 🏆              │
│  ├── Pursue easy learning, good docs ──────────────► Vue 3 🏆              │
│  ├── Large enterprise project, strong constraints ─► Angular               │
│  └── Simple project, few interactions ─────────────► Vanilla or lightweight│
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 2.7 UI Component Library Recommendations

| Framework | Recommended Library | Characteristics | Alternatives |
|-----------|--------------------|--------------------|--------------|
| **React** | Ant Design | Enterprise-grade, component-rich | Material UI, Shadcn |
| **Vue** | Element Plus | Popular, good docs | Naive UI, Vuetify |
| **Angular** | Angular Material | Official, consistent | NG-ZORRO |

---

## 3. Architecture Decisions

### 3.1 Architecture Style Decision

| User Volume | Recommended Architecture | Notes |
|-------------|-------------------------|-------|
| < 1000 | **Monolith** | Simple and reliable, one repo |
| 1K-10K | **Modular Monolith** | Monolith but clearly modular, can be split |
| 10K-100K | **Monolith + Separate Services** | Core monolith, specific functions separate |
| > 100K | **Microservices** | Requires mature team and infrastructure |

### 3.2 Must-Follow Architecture Principles

| Principle | Description | Practice |
|-----------|-------------|----------|
| **Layered** | Separate presentation, business, data layers | Controller → Service → Repository |
| **Dependency Inversion** | High-level doesn't depend on low-level, both depend on abstractions | Define interfaces |
| **Externalized Configuration** | Separate configuration from code | Environment variables, config files |
| **Separation of Concerns** | One module does one thing | Single responsibility |

### 3.3 Must-Abstract External Dependencies

These external dependencies **must be abstracted through interfaces** for future switching:

```python
# ✅ Correct: Abstract through interface
class IEmailService(ABC):
    @abstractmethod
    async def send(self, to: str, subject: str, body: str): ...

class SMTPEmailService(IEmailService): ...     # Current implementation
class SendGridEmailService(IEmailService): ... # Future switchable

# ❌ Wrong: Direct dependency on concrete implementation
class UserService:
    def __init__(self):
        self.smtp = smtplib.SMTP('smtp.example.com')  # Hardcoded!
```

**Service Abstraction Checklist:**

| Service Type | Current Implementation | Future Possible Switch |
|--------------|------------------------|----------------------|
| Email Service | SMTP | SendGrid, AWS SES |
| File Storage | Local filesystem | MinIO, S3, OSS |
| Message Queue | Redis | RabbitMQ, Kafka |
| Auth Service | Local JWT | SSO, OAuth |
| SMS Service | Twilio | AWS SNS, etc. |
| Payment Service | Stripe | PayPal, etc. |

---

## 4. Security-Related Decisions

### 4.1 Authentication Scheme

| Scheme | Use Case | Pros | Cons |
|--------|----------|------|------|
| **JWT** | API-first, frontend/backend separation | Stateless, scalable | Token revocation complex |
| **Session** | Traditional web apps | Simple, immediate revocation | Needs session storage |
| **OAuth 2.0** | Third-party login | Standardized | Implementation complex |

**Recommended: JWT + Refresh Token**

```
Access Token:  Short-lived (15-30 minutes), stored in memory
Refresh Token: Long-lived (7-30 days), stored in httpOnly Cookie
```

### 4.2 Password Storage

| Scheme | Recommendation | Notes |
|--------|:--------------:|-------|
| **bcrypt** | ⭐⭐⭐⭐⭐ | Industry standard, built-in salt |
| **Argon2** | ⭐⭐⭐⭐⭐ | Newer and more secure, recommend Argon2id |
| **PBKDF2** | ⭐⭐⭐ | Usable but older |
| MD5/SHA1 | ❌ | **Absolutely prohibited** |

### 4.3 Sensitive Data Encryption

| Data Type | Encryption Method | Notes |
|-----------|-------------------|-------|
| Password | bcrypt/Argon2 (Hash) | Irreversible |
| Sensitive Fields | AES-256-GCM | Reversible, decrypt when needed |
| Transport | TLS 1.2+ | HTTPS |
| Database | TDE (optional) | Transparent data encryption |

---

## 5. Deployment-Related Decisions

### 5.1 Containerization Decision

| Scenario | Recommended Solution |
|----------|---------------------|
| Development environment | Docker Compose |
| Small-scale production | Docker Compose + Watchtower |
| Medium-scale production | Docker Swarm or K3s |
| Large-scale production | Kubernetes |

#### Full Containerization Principle (Dev = Deploy)

**Principle**: If the production deployment target is Docker, the development environment must also run inside Docker. The host machine should only need Docker installed — no local Python, Node.js, or database setup required.

**Why**: Eliminates "works on my machine" problems. When dev environment equals deploy environment, the gap between development and production shrinks to near zero.

**Practice**:

| Operation | Correct (containerized) | Incorrect (host-based) |
|-----------|------------------------|----------------------|
| Install Python packages | Defined in `Dockerfile` → `pip install -r requirements.txt` | `pip install` on host |
| Run tests | `docker compose exec backend pytest` | `pytest` on host |
| Run database migration | `docker compose exec backend alembic upgrade head` | `alembic upgrade head` on host |
| Install npm packages | Defined in `Dockerfile` → `npm install` | `npm install` on host |
| Start dev server | `docker compose up` (auto-starts all services) | `uvicorn ... &` on host |

**When to apply**: When all of the following are true:
- Production target is Docker/container-based
- Project scale is small-to-medium (performance overhead of containers is negligible)
- Team members have varying local environments

**docker-compose.yml best practices**:
- Use `healthcheck` for service dependency ordering, ensure healthcheck paths match actual application endpoints
- Use volume mounts for hot reload during development (mount source code, exclude `node_modules`)
- Use `depends_on` with `condition: service_healthy` for proper startup ordering
- Expose only necessary ports to the host

#### Healthcheck Path Alignment

**Rule**: The `healthcheck` test URL in docker-compose.yml **must** match the actual health endpoint implemented in the application.

```yaml
# ✅ Correct: healthcheck matches actual endpoint
backend:
  healthcheck:
    test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
    # And the application actually has: @app.get("/health")

# ❌ Wrong: healthcheck path doesn't match
backend:
  healthcheck:
    test: ["CMD", "curl", "-f", "http://localhost:8000/api/v1/health"]
    # But the application only has: @app.get("/health")
    # Result: container shows "unhealthy" even though it's working fine
```

**Impact of misalignment**: Dependent services using `condition: service_healthy` may fail to start or start with incorrect status indicators, causing cascading confusion in debugging.

### 5.2 Cloud vs Self-Hosted

| Service | Recommendation | Rationale |
|---------|----------------|-----------|
| Database | Cloud database preferred | No ops, auto backup |
| Cache | Cloud Redis preferred | High availability, no ops |
| Object Storage | Cloud storage preferred | Unlimited scaling, CDN |
| Application Server | Depends | Container or Serverless |

**Exception**: If compliance requires on-premise deployment, self-host.

---

## 6. Agent Output Template

After tech selection is complete, output in this format:

```markdown
# [Project Name] - Tech Selection Document

## 1. Tech Stack Overview

| Layer | Selection | Version |
|-------|-----------|---------|
| Backend Language | [Selection] | [Version] |
| Backend Framework | [Selection] | [Version] |
| Database | [Selection] | [Version] |
| Frontend Framework | [Selection] | [Version] |
| UI Component Library | [Selection] | [Version] |
| ... | ... | ... |

## 2. Selection Rationale

### 2.1 Backend Language: [Selection]

**Selection Rationale**:
- [Reason 1]
- [Reason 2]

**Alternatives Considered**:
- [Alternative 1]: Not chosen because [reason]

### 2.2 Database: [Selection]

...(same format)

## 3. Architecture Design

### 3.1 Overall Architecture Diagram

[Architecture diagram]

### 3.2 Layer Description

...

## 4. Security Plan

### 4.1 Authentication Scheme
### 4.2 Encryption Scheme
### 4.3 Other Security Measures

## 5. Deployment Plan

### 5.1 Deployment Architecture
### 5.2 Environment Planning

## 6. Development Standards

### 6.1 Directory Structure
### 6.2 Coding Standards
### 6.3 Git Workflow
```

---

## 7. Checklist

Before tech selection is complete, ensure these questions are answered:

### Must-Answer Questions

- [ ] Backend language and framework? Why this choice?
- [ ] Database? Why this choice?
- [ ] Frontend framework and UI library? Why this choice?
- [ ] Authentication scheme? JWT or Session?
- [ ] How are passwords stored?
- [ ] How is sensitive data encrypted?
- [ ] Where to deploy? How to deploy?
- [ ] Are external services (email, storage, etc.) abstracted?

### Advanced Questions

- [ ] If user volume grows 10x, can the architecture handle it?
- [ ] If we need to switch databases, how much code change?
- [ ] If we need to integrate SSO, how much code change?
- [ ] If the lead developer leaves, can new people take over?

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.0 | 2025-01-27 | Initial version |
| v1.1 | 2025-02-02 | Added Full Containerization Principle (Dev=Deploy) and Healthcheck Path Alignment best practice, based on s-1-1 execution experience |
