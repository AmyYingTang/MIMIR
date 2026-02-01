# Phase 3 Appendix: UI/UX Design Principles

> **Purpose**: Frontend interaction design guidelines for enterprise web projects  
> **Version**: v1.0  
> **Source**: Distilled from Voice Model Platform UI Review experience  
> **Audience**: AI Agents designing UI prototypes or frontend pages

---

## 1. Core Philosophy: Minimize User Cognitive Load

### 1.1 Role-Based Experience Design

Different user roles have different technical backgrounds and usage frequencies. Interface design should match role characteristics:

| Role Type | Typical Traits | Design Strategy |
|-----------|----------------|-----------------|
| **End User** | Non-technical, low frequency, clear goals | Wizard-guided, minimal options, foolproof |
| **Admin** | Some technical background, moderate frequency | Traditional tables/forms, bulk ops, filters |
| **Super Admin** | Strong technical background, high frequency | High info density, advanced config, system ops |

**Principles**:
- End users should NOT see an admin-style Dashboard as their landing page
- End users should see a **task-oriented** interface, not an **information overview**
- Only Admins and Super Admins need Dashboard-style landing pages

### 1.2 Information Layering

```
┌─────────────────────────────────────────────────────────────────┐
│                    Information Layering Pyramid                   │
│                                                                   │
│                         ┌───────┐                                │
│                         │Welcome│  ← First screen after login    │
│                         │3 btns │    Decision only, no overload  │
│                         └───┬───┘                                │
│                      ┌──────┼──────┐                             │
│                      │      │      │                             │
│                 ┌────┴─┐ ┌─┴───┐ ┌┴─────┐                       │
│                 │Create│ │View │ │Dash- │  ← Enter on demand     │
│                 │Task  │ │Tasks│ │board │    Route users, reduce  │
│                 └──┬───┘ └─────┘ └─────┘    disorientation       │
│                    │                                              │
│            ┌───────┼───────┐                                     │
│            │ Step  │ Step  │  ← Wizard step-by-step              │
│            │  1    │  2... │    One decision at a time            │
│            └───────┴───────┘                                     │
│                                                                   │
└─────────────────────────────────────────────────────────────────┘
```

---

## 2. Wizard Pattern

### 2.1 When to Use

| Scenario | Recommended Pattern | Reason |
|----------|:-------------------:|--------|
| End user creating/submitting tasks | ✅ Wizard | Sequential dependencies, guided flow reduces errors |
| End user completing applications | ✅ Wizard | Many fields → break into steps to reduce cognitive load |
| Admin bulk operations | ❌ Traditional form | Efficiency first, admins don't need hand-holding |
| System configuration | ❌ Traditional form | Admin operation, one-time setup |
| Data queries/filtering | ❌ Table + filters | Browsing scenario, wizard is inappropriate |

### 2.2 Design Guidelines

#### Step Decomposition

```
✅ Good: Each step = one decision
  Step 1: Select chip         (1 choice)
  Step 2: Select function     (1 choice, depends on Step 1)
  Step 3: Select language     (1 choice)
  Step 4: Enter wake word     (1 input)
  Step 5: Select model sizes  (1 multi-select + optional)
  Step 6: Review & submit     (summary)

❌ Bad: All form fields on one page
  → 10+ fields in one long form = user abandonment
```

#### Visual States

Each panel must have 3 clear states:

| State | Visual | Interaction |
|-------|--------|-------------|
| **Done** | Collapsed, shows selected value ✓ | Clickable to re-expand and edit |
| **Active** | Expanded, highlighted border | Fully interactive, forward/back |
| **Locked** | Collapsed, semi-transparent | Non-interactive, waiting for prior steps |

#### Progress Visibility

- Always show a progress bar at the top (step numbers + current position)
- Completed steps turn green ✓
- Users always know "where am I" and "how many steps left"

#### Smart Defaults

- Set reasonable defaults for every option; users can click "Next" immediately
- If only one option exists (e.g., only one authorized chip), auto-select and skip
- Final step provides a complete summary for review before one-click submit

### 2.3 Anti-Patterns

| ❌ Anti-Pattern | ✅ Correct Approach |
|-----------------|---------------------|
| Too many fields per step (>3 decisions) | Focus each step on 1 core decision |
| No back navigation | Allow returning to any completed step |
| No confirmation summary at the end | Must have a Review step before submit |
| Staying on the same page after submit | Show success modal, guide to next action |
| No data coupling between steps | Later steps should adapt to earlier selections |

---

## 3. Post-Login Welcome / Launchpad Pattern

### 3.1 When to Use

When end users have ≤ 5 core tasks, use a Launchpad instead of a Dashboard as the post-login landing page.

### 3.2 Design Guidelines

| Element | Guideline |
|---------|-----------|
| **Action button count** | 3-5, corresponding to most frequent tasks |
| **Visual weight** | Most important action (e.g., "Create Task") is most prominent |
| **Information density** | Very low — only action entry points, no detailed data |
| **Quick stats** | Optional at the bottom (e.g., task counts), but not the focus |
| **Dashboard link** | One of the buttons should link to the full Dashboard |

---

## 4. Configuration-Driven UI Adaptation

### 4.1 Principle

Frontend should **dynamically adapt** based on backend configuration, never hardcode options.

### 4.2 Common Scenarios

| Scenario | Config Source | UI Behavior |
|----------|-------------|-------------|
| Only 1 model size available | `GET /system/options` | Auto-select, skip or simplify that step |
| Chip supports only 1 function | Permission + chip data | Auto-select function type, skip selection |
| Downloads never expire | `expire_days = -1` | Hide "Expiry Date" column |
| Feature module disabled | `features.modules.xxx = false` | Hide corresponding nav and pages |

---

## 5. Checklist

### 5.1 End User Interface

- [ ] Post-login page is a Launchpad/Welcome, not a Dashboard
- [ ] Core operations use Wizard pattern
- [ ] Each Wizard step contains only 1 core decision
- [ ] Reasonable defaults/presets exist
- [ ] Single-option configs are auto-selected or skipped
- [ ] Internal/meaningless fields are hidden from users
- [ ] Clear success feedback and next-step guidance after submission

### 5.2 Admin Interface

- [ ] Uses traditional table/form layout (info density first)
- [ ] Provides filter, sort, and bulk operations
- [ ] Dangerous operations have confirmation dialogs
- [ ] Advanced/dangerous operations are in collapsible sections

### 5.3 Responsiveness & Configuration

- [ ] UI renders options dynamically from system/options API
- [ ] Disabled features → hidden UI
- [ ] No hardcoded option lists (chips, function types, model sizes, etc.)

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v1.0 | 2025-02-01 | Initial version, distilled from Voice Model Platform UI Review |
