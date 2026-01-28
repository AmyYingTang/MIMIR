# Phase 4: Testing Strategy

> **Phase Goal**: Establish layered testing system, support TDD development  
> **Prerequisites**: System design complete, business rules defined  
> **Core Deliverables**: Test case documentation, test code framework

---

## 1. Test Pyramid

```
         ▲
        /│\         E2E Tests (few)
       / │ \        • Critical business flows
      /  │  \       • Mostly manual
     /───┼───\      
    /    │    \     Integration Tests (moderate)
   /     │     \    • API interface testing
  /──────┼──────\   • Automated (pytest + httpx)
 /       │       \  
/────────┼────────\ Unit Tests (many)
         │          • Validation functions, state machines, utility functions
         │          • Automated (pytest)
```

**Principle**: More tests at lower levels, fewer at higher levels; lower levels automated, higher levels can be manual

---

## 2. TDD Workflow

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│ 1. Write    │ ──► │ 2. Run fails│ ──► │ 3. Implement│
│    test case│     │    (Red)    │     │    code     │
│    doc      │     │             │     │             │
└─────────────┘     └─────────────┘     └──────┬──────┘
                                               │
       ┌───────────────────────────────────────┘
       ▼
┌─────────────┐     ┌─────────────┐
│ 4. Run passes│ ──► │ 5. Refactor │
│    (Green)  │     │    (stay Green)│
└─────────────┘     └─────────────┘
```

### 2.1 TDD Applicability

| Suitable for TDD | Less Suitable for TDD |
|------------------|----------------------|
| Validation functions | UI interfaces |
| State machine logic | Third-party integrations |
| API interfaces | Exploratory development |
| Utility functions | Rapid prototypes |
| Computation logic | |

---

## 3. Test Case Documentation Structure

### 3.1 Unit Test Cases (unit-tests.md)

```markdown
## 1. Username Validation (validate_username)

| ID | Input | Expected | Notes |
|----|-------|:--------:|-------|
| UN-01 | `""` | ❌ | Empty |
| UN-02 | `"ab"` | ❌ | < 3 characters |
| UN-03 | `"abc"` | ✅ | Minimum length |
| UN-04 | `"123abc"` | ❌ | Starts with number |

Corresponding pytest code:

```python
@pytest.mark.parametrize("username,valid", [
    ("", False), ("ab", False), ("abc", True), ("123abc", False),
])
def test_username(username, valid):
    result, _ = validate_username(username)
    assert result == valid
```
```

### 3.2 API Test Cases (api-tests.md)

```markdown
## 1. User Login POST /api/v1/auth/login

| ID | Scenario | Expected Code | Expected Response |
|----|----------|:-------------:|-------------------|
| LOGIN-01 | Normal login | 200 | Returns token |
| LOGIN-02 | Wrong username | 401 | code=40101 |
| LOGIN-03 | Wrong password | 401 | code=40101 |
| LOGIN-04 | Account locked | 403 | code=40304 |
```

### 3.3 E2E Test Checklist (e2e-checklist.md)

```markdown
## E2E-001: User Registration to Task Submission

**Preconditions**: System deployed, admin account exists

| # | Action | Expected Result | ✓ |
|:-:|--------|-----------------|:-:|
| 1 | Visit registration page | Form displays | ⬜ |
| 2 | Fill info and submit | Shows "Pending review" | ⬜ |
| 3 | Admin approves | User status becomes active | ⬜ |
| 4 | User logs in and submits task | Task shows "Queued" | ⬜ |

**Result**: ⬜ Pass / ⬜ Fail
```

---

## 4. Test File Organization

```
project/
├── docs/
│   └── test-cases/
│       ├── unit-tests.md       # Unit test cases (human readable)
│       ├── api-tests.md        # API test cases
│       └── e2e-checklist.md    # E2E test checklist
├── tests/
│   ├── conftest.py             # pytest config and fixtures
│   ├── unit/
│   │   ├── test_validators.py
│   │   ├── test_state_machines.py
│   │   └── test_utils.py
│   ├── integration/
│   │   ├── test_auth_api.py
│   │   ├── test_user_api.py
│   │   └── test_task_api.py
│   └── e2e/
│       └── E2E-CHECKLIST.md    # Execution records
```

---

## 5. Key Points for Each Test Type

### 5.1 Unit Tests

**Coverage**:
- All validation functions (username, password, email, wake words, etc.)
- State machine transitions (user status, task status)
- Computation logic (lockout time, queue position)
- Utility functions

**Writing Principles**:
- Use `@pytest.mark.parametrize` for parameterized testing
- Each function should cover at minimum: normal case, boundary case, error case
- Tests should be fast, independent, repeatable

### 5.2 API Integration Tests

**Coverage**:
- All API endpoints
- Normal and error flows
- Permission verification
- Data validation

**Writing Principles**:
- Use test database, reset before each test
- Use fixtures to create test users, test data
- Verify status codes and response structure

### 5.3 E2E Tests

**Coverage**:
- Core business flows (registration → approval → submit task → download)
- Permission verification flows
- Exception flows (login lockout, approval rejection, etc.)

**Execution Method**:
- Manual execution before each release
- Check items one by one on the checklist
- Record execution results and issues

---

## 6. Test Run Commands

```bash
# Run all unit tests
pytest tests/unit/ -v

# Run all integration tests
pytest tests/integration/ -v

# Run all tests + coverage
pytest --cov=app --cov-report=html

# Run specific file only
pytest tests/unit/test_validators.py -v

# Run tests matching keyword only
pytest -k "username" -v

# Stop on failure
pytest -x

# Show detailed output
pytest -v --tb=short
```

---

## 7. Integration with Development Workflow

### 7.1 Feature Development Workflow

```
1. Read requirements/design docs
2. Write test case documentation (write .md first)
3. Convert test cases to pytest code
4. Run tests, confirm failure (Red)
5. Implement feature code
6. Run tests, confirm pass (Green)
7. Refactor code, keep tests passing
8. Commit code
```

### 7.2 Pre-Release Checklist

```
1. Run all automated tests
2. Execute E2E checklist
3. Check test coverage
4. Confirm no failing tests
5. Release
```

---

## 8. Agent Usage Instructions

When user says "help me write tests" or "establish testing system":

1. **Clarify test scope**: Unit tests? API tests? E2E?
2. **Write case documentation first**: Use Markdown table format
3. **Convert to code**: Generate pytest code based on cases
4. **Suggest directory structure**: Organize according to above structure

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.0 | 2025-01-27 | Initial version |
