# Phase 3: System Design

> **Phase Goal**: Complete database design, API design, security design  
> **Deliverables**: Database design document, API design document, configuration management scheme  
> **Key Principles**: Design-to-Interface, configuration-driven, reserve for extension

---

## 1. Database Design

### 1.1 Design Principles

| Principle | Description | Practice |
|-----------|-------------|----------|
| **Normalization** | Follow 3NF, avoid redundancy | Unless performance considerations |
| **Primary Key Strategy** | UUID vs Auto-increment | Use UUID for distributed systems |
| **Soft Delete** | Don't physically delete important data | `deleted_at` field |
| **Audit Fields** | Record creation and modification time | `created_at`, `updated_at` |
| **Index Strategy** | Create indexes for common queries | Don't over-index |

### 1.2 Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Table Name | Lowercase plural, underscore separated | `users`, `training_tasks` |
| Field Name | Lowercase, underscore separated | `created_at`, `user_id` |
| Primary Key | `id` | `id` |
| Foreign Key | `{related_table_singular}_id` | `user_id`, `chip_id` |
| Boolean Fields | `is_` or `has_` prefix | `is_active`, `has_permission` |
| Time Fields | `_at` suffix | `created_at`, `expires_at` |

### 1.3 Required Fields Checklist

Every business table should consider:

```sql
-- Basic fields
id              -- Primary key (UUID or BIGINT)
created_at      -- Creation time
updated_at      -- Update time

-- Soft delete (for important data)
deleted_at      -- Deletion time, NULL means not deleted

-- Audit (if needed)
created_by      -- Creator
updated_by      -- Updater
```

### 1.4 Sensitive Data Handling

| Data Type | Storage Method | Field Naming |
|-----------|----------------|--------------|
| Password | bcrypt Hash | `password_hash` |
| Decryptable sensitive data | AES-256 encrypted | `xxx_encrypted` |
| Sensitive data for queries | Hash + encrypted dual storage | `xxx_hash` + `xxx_encrypted` |
| Phone/Email (for display) | Store plaintext, mask when displaying | `phone`, `email` |

### 1.5 Design Checklist

- [ ] All tables have primary keys
- [ ] Foreign key relationships correctly defined
- [ ] Sensitive fields identified with encryption scheme
- [ ] Common queries have index support
- [ ] Soft delete strategy in place (if needed)
- [ ] Audit fields present
- [ ] ER diagram created

---

## 2. API Design

### 2.1 RESTful Standards

| HTTP Method | Semantics | Idempotent | Example |
|-------------|-----------|:----------:|---------|
| GET | Query | ✅ | `GET /users/123` |
| POST | Create | ❌ | `POST /users` |
| PUT | Full Update | ✅ | `PUT /users/123` |
| PATCH | Partial Update | ✅ | `PATCH /users/123` |
| DELETE | Delete | ✅ | `DELETE /users/123` |

### 2.2 URL Design Standards

```
✅ Correct
GET  /api/v1/users              # Get user list
GET  /api/v1/users/123          # Get single user
POST /api/v1/users              # Create user
POST /api/v1/users/123/approve  # Action-type operation

❌ Wrong
GET  /api/v1/getUsers           # Verb
GET  /api/v1/user               # Singular
POST /api/v1/users/create       # Redundant verb
```

### 2.3 Unified Response Format

```json
// Success response
{
  "code": 0,
  "message": "success",
  "data": { ... },
  "timestamp": "2025-01-27T10:00:00Z",
  "request_id": "req_xxx"
}

// Paginated response
{
  "code": 0,
  "message": "success",
  "data": {
    "items": [...],
    "pagination": {
      "page": 1,
      "page_size": 20,
      "total": 100,
      "total_pages": 5
    }
  }
}

// Error response
{
  "code": 40001,
  "message": "Parameter validation failed",
  "errors": [
    { "field": "email", "message": "Invalid email format" }
  ],
  "timestamp": "2025-01-27T10:00:00Z",
  "request_id": "req_xxx"
}
```

### 2.4 Error Code Design

```
Error code format: XXYYYY

XX: Category
  00 - Success
  40 - Client error
  50 - Server error

YYYY: Specific error

Examples:
  0     - Success
  40000 - Generic parameter error
  40001 - Business rule validation failed
  40100 - Unauthenticated
  40101 - Wrong username or password
  40300 - Unauthorized
  40400 - Resource not found
  50000 - Internal server error
```

### 2.5 Authentication & Authorization

```python
# Permission decorator design pattern
@router.get("/users")
@require_auth                              # Requires login
@require_role(["ADMIN", "SUPER_ADMIN"])    # Requires role
@require_permission("user_management")      # Requires permission
async def list_users():
    ...
```

### 2.6 API Design Checklist

- [ ] URLs follow RESTful standards
- [ ] Unified response format
- [ ] Unified error codes
- [ ] Authentication scheme defined
- [ ] Permission control scheme defined
- [ ] Pagination standards defined
- [ ] Version management strategy (`/api/v1`)

---

## 3. Layered Architecture Design

### 3.1 Standard Layers

```
┌─────────────────────────────────────────────────────────────┐
│                     Controller Layer                         │
│  • Receive requests, validate parameters                     │
│  • Call Service, return response                             │
│  • No business logic                                         │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      Service Layer                           │
│  • Business logic implementation                             │
│  • Transaction management                                    │
│  • Call Repository and external services                     │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│   Repository    │ │ External Service│ │     Cache       │
│   (Data Access) │ │ (External Svc)  │ │    (Cache)      │
└─────────────────┘ └─────────────────┘ └─────────────────┘
```

### 3.2 External Service Abstraction

**Services that must be abstracted:**

```python
# Email service interface
class IEmailService(ABC):
    @abstractmethod
    async def send(self, to: str, subject: str, body: str) -> bool: ...

# Storage service interface
class IStorageService(ABC):
    @abstractmethod
    async def upload(self, file: bytes, path: str) -> str: ...
    
    @abstractmethod
    async def download(self, path: str) -> bytes: ...
    
    @abstractmethod
    async def delete(self, path: str) -> bool: ...

# Training service interface (example: specific business)
class ITrainingService(ABC):
    @abstractmethod
    async def submit(self, task: TaskCreate) -> Task: ...
    
    @abstractmethod
    async def get_status(self, task_id: str) -> TaskStatus: ...
```

**Configuration to switch implementations:**

```yaml
# config/services.yaml
services:
  email:
    provider: "smtp"      # Options: smtp, sendgrid, mock
  storage:
    provider: "local"     # Options: local, minio, s3
  training:
    provider: "cli"       # Options: cli, api
```

```python
# Factory function
def get_email_service(config: Config) -> IEmailService:
    match config.services.email.provider:
        case "smtp":
            return SMTPEmailService(config.services.email.smtp)
        case "sendgrid":
            return SendGridEmailService(config.services.email.sendgrid)
        case "mock":
            return MockEmailService()
```

---

## 4. Configuration Management Design

### 4.1 Configuration Layers

```
┌─────────────────────────────────────────────────────────────┐
│                    Configuration Layers                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Environment Variables (.env)     Sensitive configuration   │
│  ├── DATABASE_URL                 ├── Database password     │
│  ├── JWT_SECRET                   ├── JWT secret key        │
│  ├── SMTP_PASSWORD                ├── Third-party API keys  │
│  └── ...                          └── Encryption keys       │
│                                                             │
│  Config Files (config/*.yaml)     Non-sensitive config      │
│  ├── services.yaml                ├── Service provider choice│
│  ├── business.yaml                ├── Business parameters   │
│  ├── features.yaml                ├── Feature flags         │
│  └── ...                          └── Defaults              │
│                                                             │
│  Code Constants (constants.py)    Enums and mappings        │
│  ├── Enum definitions             ├── FunctionType          │
│  ├── Display name mappings        ├── FUNCTION_TYPE_NAMES   │
│  └── ...                          └── Type safety           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 4.2 Enum and Configuration Separation

**Define enums and names in code (immutable):**

```python
# constants.py
class FunctionType(str, Enum):
    KWS = "KWS"
    COMMAND_CONTROL = "COMMAND_CONTROL"
    VAD = "VAD"  # Reserved

FUNCTION_TYPE_NAMES = {
    FunctionType.KWS: {"en": "Keyword Spotting", "zh": "唤醒词识别"},
    FunctionType.COMMAND_CONTROL: {"en": "Command Control", "zh": "命令词识别"},
    FunctionType.VAD: {"en": "Voice Activity Detection", "zh": "人声检测"},
}
```

**Config file controls enabled state (mutable):**

```yaml
# business.yaml
function_types:
  KWS:
    enabled: true
  COMMAND_CONTROL:
    enabled: true
  VAD:
    enabled: false  # Not launched yet
```

**Enabling new feature only requires config change:**

```yaml
# Launch VAD feature
function_types:
  VAD:
    enabled: true  # Just change to true
```

### 4.3 Feature Flag Design

```yaml
# features.yaml
features:
  # Module flags
  modules:
    user_registration: true    # Self-service user registration
    email_notification: true   # Email notifications
    model_testing: false       # Model testing (P2)
    audit_logging: false       # Audit logging (P3)
  
  # Feature flags
  functions:
    sso_login: false           # SSO login
    multi_language: false      # Multi-language
    api_rate_limit: true       # API rate limiting
```

**Using feature flags in code:**

```python
if feature_flags.is_enabled("email_notification"):
    await email_service.send_notification(user, task)
```

---

## 5. Security Design

### 5.1 Security Checklist

#### Authentication Security

- [ ] Passwords stored using bcrypt/Argon2
- [ ] JWT has reasonable expiration time
- [ ] Refresh Token uses httpOnly Cookie
- [ ] Login failures have count limit and lockout mechanism
- [ ] Password reset has secure process

#### Transport Security

- [ ] Enforce HTTPS
- [ ] Configure HSTS header
- [ ] Disable insecure TLS versions

#### Data Security

- [ ] Sensitive data encrypted at rest
- [ ] Logs don't contain sensitive information
- [ ] API responses don't leak sensitive information
- [ ] Database connections use encryption

#### API Security

- [ ] Input validation (prevent injection)
- [ ] CORS configured correctly
- [ ] Request rate limiting
- [ ] Security response headers configured

#### Permission Security

- [ ] Every API has permission check
- [ ] Data access has privilege escalation check
- [ ] Management functions have audit logs

### 5.2 Security Response Headers

```python
# Recommended security response headers
security_headers = {
    "X-Content-Type-Options": "nosniff",
    "X-Frame-Options": "DENY",
    "X-XSS-Protection": "1; mode=block",
    "Strict-Transport-Security": "max-age=31536000; includeSubDomains",
    "Content-Security-Policy": "default-src 'self'",
    "Referrer-Policy": "strict-origin-when-cross-origin",
}
```

---

## 6. Design Document Templates

### 6.1 Database Design Document Structure

```markdown
# Database Design Document

## 1. Design Overview
### 1.1 Design Principles
### 1.2 Naming Conventions
### 1.3 Table List

## 2. ER Diagram

## 3. Detailed Table Structure Design
### 3.1 [Module Name] Module
#### 3.1.1 [Table Name] Table
- Table description
- Field definitions (DDL)
- Index descriptions
- Relationships

## 4. Data Dictionary
### 4.1 Enum Value Definitions

## 5. Design Notes
### 5.1 Primary Key Strategy
### 5.2 Soft Delete Strategy
### 5.3 Sensitive Data Handling
```

### 6.2 API Design Document Structure

```markdown
# API Design Document

## 1. Design Principles
### 1.1 RESTful Standards
### 1.2 URL Naming Conventions
### 1.3 Response Format Standards

## 2. Authentication Scheme
### 2.1 Authentication Flow
### 2.2 Permission Model

## 3. API Interface Definitions
### 3.1 [Module Name] Module
#### 3.1.1 [Interface Name]
- Request method and URL
- Request parameters
- Response format
- Error codes
- Examples

## 4. Error Code Definitions

## 5. Extension Point Design
### 5.1 Feature Flags
### 5.2 Service Abstractions
```

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.0 | 2025-01-27 | Initial version |
