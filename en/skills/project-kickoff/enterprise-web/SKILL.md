# Enterprise Web Project Startup Guide

> **Skill Type**: Enterprise Web Application  
> **Applicable Scenarios**: B2B SaaS, internal management systems, platform products, multi-user systems  
> **Version**: v1.0  
> **Source**: Extracted from real enterprise project experience

---

## Overview

This Skill guides AI Agents or developers through the complete startup process for enterprise web projects, ensuring no critical considerations are overlooked.

### Core Characteristics of Enterprise Projects

| Characteristic | Description | Must Consider |
|----------------|-------------|:-------------:|
| **Multi-user** | Multiple users, requires account system | ✅ |
| **Permission Control** | Different roles have different permissions | ✅ |
| **Data Security** | Sensitive data needs protection | ✅ |
| **Auditable** | Operations need to be traceable | ✅ |
| **High Availability** | Can't go down randomly | 🔶 |
| **Scalable** | May need to scale in the future | ✅ |
| **Compliance** | May need to meet regulatory requirements | 🔶 |

---

## Full Development Process

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                   Enterprise Web Project Development Process                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  Phase 1          Phase 2          Phase 3          Phase 4                 │
│  Requirements     Tech Selection   System Design    Implementation          │
│  ┌─────────┐      ┌─────────┐      ┌─────────┐      ┌─────────┐            │
│  │ • PRD   │  →   │ • Arch  │  →   │ • DB    │  →   │ • Backend│           │
│  │ • Cases │      │ • Stack │      │ • API   │      │ • Frontend│          │
│  │ • Proto │      │ • Deploy│      │ • Security│    │ • Integration│       │
│  └─────────┘      └─────────┘      └─────────┘      └─────────┘            │
│       │                │                │                │                  │
│       ▼                ▼                ▼                ▼                  │
│  📄 PRD Doc       📄 Tech Doc      📄 Design Docs   📁 Code Repo           │
│                                                                             │
│  Phase 5          Phase 6          Phase 7          Continuous              │
│  Testing          Documentation    Deployment       Retrospective           │
│  ┌─────────┐      ┌─────────┐      ┌─────────┐      ┌─────────┐            │
│  │ • Unit  │  →   │ • User  │  →   │ • CI/CD │  →   │ • Summary│           │
│  │ • API   │      │   Manual│      │ • Monitor│     │ • Update │           │
│  │ • E2E   │      │ • Admin │      │ • Backup│      │ • Method │           │
│  │ • TDD   │      │   Manual│      │         │      │          │           │
│  └─────────┘      └─────────┘      └─────────┘      └─────────┘            │
│       │                │                                                    │
│       ▼                ▼                                                    │
│  📄 Test Cases    📄 Delivery Docs                                          │
│                   (MD → PDF)                                                │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Phase 1: Requirements Analysis

> 📄 Detailed Guide: `phase-1-requirements.md`  
> 📄 Output Template: `templates/prd-template.md`

### Must-Answer Questions

#### 1.1 Business Questions

| Question | Why Important | Impact |
|----------|---------------|--------|
| What business problem does this system solve? | Clarify project value | Requirement priority |
| Who are the target users? How many user types? | Determines permission model complexity | System architecture |
| What is the core business process? | Define MVP scope | Development order |
| What are the success criteria? | Measurable goals | Acceptance criteria |

#### 1.2 Scale Questions

| Question | Why Important | Impact |
|----------|---------------|--------|
| Expected user volume? | Affects architecture selection | Database, caching, deployment |
| Expected concurrency? | Affects performance design | Rate limiting, queues, scaling |
| Estimated data volume? | Affects storage selection | Database, sharding strategy |
| Growth expectations? | Reserve expansion room | Architecture flexibility |

**Scale-Architecture Reference:**

| Users | Concurrency | Recommended Architecture |
|-------|-------------|--------------------------|
| < 100 | < 10 | Monolith, single server |
| 100-1K | 10-100 | Monolith, consider read/write separation |
| 1K-10K | 100-1K | Monolith or microservices, load balancing, caching |
| > 10K | > 1K | Microservices, container orchestration, distributed |

#### 1.3 Permission Questions

| Question | Why Important | Common Patterns |
|----------|---------------|-----------------|
| How many user roles? | Permission model design | RBAC / ABAC |
| Permission granularity? (page/function/data level) | Implementation complexity | - |
| Need approval workflows? | State machine design | - |
| Need data isolation? (multi-tenant) | Architecture design | Shared DB/separate DB |

#### 1.4 Integration Questions

| Question | Why Important | Design Considerations |
|----------|---------------|----------------------|
| Which external systems to integrate with? | Interface design | Adapter pattern |
| Future SSO integration possible? | Auth architecture | Abstract auth layer |
| Need to expose API to third parties? | API design | Version management, auth |
| Need data import/export? | Data formats | Batch APIs |

### Deliverables

- [ ] PRD document (including functional and non-functional requirements)
- [ ] User roles and permission matrix
- [ ] Core business process diagrams
- [ ] Priority classification (P0/P1/P2/P3)
- [ ] MVP scope definition

---

## Phase 2: Technology Selection

> 📄 Detailed Guide: `phase-2-tech-selection.md`  
> 📄 Output Template: `templates/tech-selection-template.md`

### Selection Principles

| Principle | Description |
|-----------|-------------|
| **Team Familiarity First** | Don't choose tech the team doesn't know just for "advancement" |
| **Align with Existing Stack** | Reduce maintenance cost, facilitate knowledge sharing |
| **Community Activity** | Can find solutions when problems arise |
| **Long-term Maintenance** | Avoid choosing technology that might be abandoned |
| **Just Enough** | Don't over-engineer, don't pursue perfection |

### Must-Decide Items

#### 2.1 Backend Tech Stack

| Decision Item | Popular Options | Decision Factors |
|---------------|-----------------|------------------|
| **Language** | Python / Node.js / Java / Go / C# | Team familiarity, ecosystem needs |
| **Framework** | FastAPI / Django / Express / Spring Boot / .NET | Project complexity, dev efficiency |
| **Database** | PostgreSQL / MySQL / MongoDB | Data structure, query patterns |
| **Cache** | Redis / Memcached | Need persistence? |
| **Queue** | Redis / RabbitMQ / Kafka | Message volume, reliability requirements |

#### 2.2 Frontend Tech Stack

| Decision Item | Popular Options | Decision Factors |
|---------------|-----------------|------------------|
| **Framework** | React / Vue / Angular | Team familiarity, ecosystem |
| **UI Library** | Ant Design / Element Plus / Material UI | Component richness, customization needs |
| **State Management** | Redux / Pinia / Zustand | Project complexity |
| **Build Tool** | Vite / Webpack | Development experience |

#### 2.3 Infrastructure

| Decision Item | Popular Options | Decision Factors |
|---------------|-----------------|------------------|
| **Deployment** | Docker / K8s / Direct | Ops capability, scale |
| **Cloud Provider** | AWS / Azure / GCP / On-premise | Compliance, cost |
| **CI/CD** | GitHub Actions / GitLab CI / Jenkins | Code hosting platform |
| **Monitoring** | Prometheus / Grafana / Cloud-native | Ops maturity |

### Deliverables

- [ ] Tech selection document (with selection rationale)
- [ ] Architecture diagram
- [ ] Directory structure planning
- [ ] Development environment requirements

---

## Phase 3: System Design

> 📄 Detailed Guide: `phase-3-system-design.md`  
> 📄 UI/UX Design Principles: `phase-3-ui-design-principles.md`  
> 📄 Output Templates: `templates/database-design-template.md`, `templates/api-design-template.md`

### Architecture Design Principles

| Principle | Description | Practice |
|-----------|-------------|----------|
| **Layered Architecture** | Clear responsibilities per layer | Controller → Service → Repository |
| **Dependency Inversion** | Depend on abstractions, not concretions | Define interfaces, switchable implementations |
| **Externalized Configuration** | Behavior controlled by config | Feature flags, environment variables |
| **Separation of Concerns** | Separate business logic from technical details | Domain-driven design |

### Must-Design Content

#### 3.1 Database Design

- [ ] ER diagram (entity relationships)
- [ ] Table structure design (field types, indexes, constraints)
- [ ] Sensitive data identification and encryption scheme
- [ ] Soft delete strategy
- [ ] Audit fields (created_at, updated_at, deleted_at)

#### 3.2 API Design

- [ ] RESTful interface definitions
- [ ] Unified response format
- [ ] Error code system
- [ ] Authentication scheme (JWT / Session)
- [ ] Permission control scheme

#### 3.3 Security Design

> 📄 Detailed Checklist: `checklists/security-checklist.md`

- [ ] Authentication and session management
- [ ] Password storage scheme (bcrypt/Argon2)
- [ ] Sensitive data encryption (AES-256)
- [ ] Transport security (HTTPS)
- [ ] Input validation and injection prevention
- [ ] CORS policy
- [ ] Log masking

#### 3.4 UI/UX Design

> 📄 Detailed Guide: `phase-3-ui-design-principles.md`

- [ ] End user interfaces use wizard-style interaction (Wizard Pattern)
- [ ] Post-login landing page uses Launchpad pattern, not Dashboard
- [ ] UI options fetched dynamically from backend config API, no hardcoded values
- [ ] Admin interfaces use traditional table/form layouts

#### 3.5 Extensibility Design

- [ ] External service abstractions (email, storage, third-party APIs)
- [ ] Feature flag design
- [ ] Enum and configuration management
- [ ] Multi-language preparation (if needed)
- [ ] SSO integration preparation (if needed)

### Deliverables

- [ ] Database design document
- [ ] API design document
- [ ] Security design scheme
- [ ] Configuration management scheme
- [ ] UI prototype / interaction design (including wizard flows)

---

## Phase 4: Code Implementation

> 📄 Detailed Guide: `phase-4-implementation.md`

### Implementation Principles

| Principle | Description |
|-----------|-------------|
| **Get It Running First** | Implement core flow first, optimize details later |
| **Continuous Integration** | Small commits, frequent integration |
| **Test First** | Core logic must have unit tests |
| **Code Standards** | Follow language/framework best practices |

### Recommended Implementation Order

```
1. Infrastructure
   ├── Project skeleton setup
   ├── Database connection
   ├── Configuration management
   └── Logging framework

2. Authentication Module
   ├── User model
   ├── Registration/Login
   ├── JWT issuance/validation
   └── Permission middleware

3. Core Business Modules
   ├── Implement by P0 priority
   ├── Each module: Model → Repository → Service → Controller
   └── Accompanying unit tests

4. Admin Dashboard
   ├── User management
   ├── Configuration management
   └── Data overview

5. Frontend Implementation
   ├── Routing and layout
   ├── Authentication flow
   ├── Core pages
   └── Admin dashboard
```

---

## Phase 5: Testing Strategy

> 📄 Detailed Guide: `phase-4-testing.md`

### Test Pyramid

```
         ▲
        /│\         E2E Tests (few, mostly manual)
       / │ \        
      /──┼──\       Integration Tests (moderate, automated)
     /   │   \      
    /────┼────\     Unit Tests (many, automated)
```

### TDD Workflow

```
Write test case → Run fails (Red) → Implement code → Run passes (Green) → Refactor
```

### Test Types and Coverage

| Type | Coverage | Execution | Priority |
|------|----------|-----------|:--------:|
| **Unit Tests** | Validation functions, state machines, computation logic | pytest automation | P0 |
| **API Integration Tests** | All API endpoints | pytest + httpx | P0 |
| **E2E Tests** | Critical business flows | Manual checklist | P1 |

### Test File Organization

```
docs/test-cases/
├── unit-tests.md       # Unit test cases (human readable)
├── api-tests.md        # API test cases
└── e2e-checklist.md    # E2E test checklist

tests/
├── unit/               # Automated unit tests
├── integration/        # Automated API tests
└── conftest.py         # fixtures
```

### Deliverables

- [ ] Test case documentation (before code)
- [ ] pytest test code
- [ ] E2E test checklist
- [ ] Test coverage report

---

## Phase 6: Documentation Delivery

> 📄 Detailed Guide: `phase-5-documentation.md`

### Documentation Strategy

```
Maintain Markdown during development ──────► Export PDF before launch (formal delivery)
        │
        └──────────────► Also serves as AI Q&A knowledge source
```

### Document Types

| Document | Readers | Content | Format |
|----------|---------|---------|--------|
| **User Manual** | End users | Feature usage instructions | Markdown → PDF |
| **Admin Manual** | System admins | Backend management operations | Markdown → PDF |
| **Deployment Docs** | DevOps | Installation and configuration steps | Markdown |
| **API Docs** | Developers | Interface documentation | Swagger auto-generated |

### Documentation Timing

| Development Phase | Documentation Update |
|-------------------|---------------------|
| Complete user feature | Update user-guide.md |
| Complete admin feature | Update admin-guide.md |
| Complete deployment script | Update deployment.md |

### Deliverables

- [ ] User manual (user-guide.md → PDF)
- [ ] Admin manual (admin-guide.md → PDF)
- [ ] Deployment documentation (deployment.md)
- [ ] Project knowledge index (for AI queries)

---

## Phase 7: Deployment & Operations

> 📄 Detailed Checklist: `checklists/production-readiness.md`

### Production Readiness Checklist

#### Security
- [ ] HTTPS configured
- [ ] Security response headers
- [ ] Secret management (environment variables, not in codebase)
- [ ] Dependency vulnerability scanning

#### Reliability
- [ ] Error handling with friendly messages
- [ ] Database backup strategy
- [ ] Disaster recovery plan
- [ ] Health check endpoints

#### Observability
- [ ] Log collection
- [ ] Performance monitoring
- [ ] Error tracking
- [ ] Alert configuration

#### Operations
- [ ] Deployment documentation
- [ ] Rollback plan
- [ ] Database migration process
- [ ] Operations manual

---

## Enterprise Concerns Checklist

> 📄 Detailed Checklist: `checklists/enterprise-concerns.md`

These are must-consider but easily overlooked points for enterprise projects:

### Authentication & Identity

| Concern | Description | Priority |
|---------|-------------|:--------:|
| Password policy | Length, complexity, expiration | P1 |
| Login protection | Failure lockout, captcha | P1 |
| Session management | Timeout, concurrent login control | P1 |
| SSO preparation | Abstract auth layer | P2 |
| MFA | Multi-factor authentication | P2 |

### Data Security

| Concern | Description | Priority |
|---------|-------------|:--------:|
| Data classification | Identify sensitive data | P0 |
| Encrypted storage | Passwords, sensitive fields | P0 |
| Transport encryption | HTTPS | P0 |
| Log masking | Don't log sensitive info | P1 |
| Data backup | Regular backup, test recovery | P1 |

### Audit & Compliance

| Concern | Description | Priority |
|---------|-------------|:--------:|
| Operation logs | Who did what when | P1 |
| Data retention | Retention period, deletion policy | P2 |
| Privacy compliance | GDPR, data protection laws | P2 |
| Access audit | Sensitive data access records | P2 |

### Availability & Performance

| Concern | Description | Priority |
|---------|-------------|:--------:|
| Error handling | Graceful degradation, friendly messages | P0 |
| Timeout control | Avoid infinite waits | P1 |
| Rate limiting | Prevent abuse | P1 |
| Caching strategy | Cache hot data | P2 |
| Load balancing | Multi-instance deployment | P2 |

### Maintainability

| Concern | Description | Priority |
|---------|-------------|:--------:|
| Database migrations | Version management | P0 |
| Configuration management | Environment separation | P0 |
| Documentation | API docs, deployment docs | P1 |
| Monitoring & alerts | Proactive problem discovery | P1 |

---

## Appendix: Decision Record Template

Every important technical decision should be recorded in this format:

```markdown
### Decision: [Decision Title]

**Context**: Why was this decision needed?

**Options**:
1. Option A - Description
2. Option B - Description
3. Option C - Description

**Decision**: Chose Option X

**Rationale**: Why was this option chosen?

**Consequences**: What impact does this decision have?

**Date**: YYYY-MM-DD
```

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.0 | 2026-01-27 | Initial version |
| v1.1 | 2026-01-27 | Added Testing Strategy (Phase 5) and Documentation Delivery (Phase 6) phases |
| v1.2 | 2026-02-02 | Phase 2 update: Full Containerization Principle (Dev=Deploy) and Healthcheck Path Alignment |
