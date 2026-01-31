# MIMIR Core Principles

> **Version**: v1.0  
> **Created**: 2025-01-31  
> **Source**: Task Decomposition Validation Practice

---

## Overview

This document records the core design principles of the MIMIR methodology. These principles apply to all Skills and guide AI-human collaboration.

---

## Principle 1: Minimum User Input

### Definition

When designing AI-led task flows, maximize AI autonomy and minimize user operational burden.

### Key Points

1. **Only Ask Necessary Questions**
   - Only ask for information that AI cannot obtain automatically
   - Collect all necessary inputs at once, avoiding repeated interruptions

2. **User Answers Once, AI Completes the Rest**
   - After obtaining necessary information, AI autonomously completes all subsequent steps
   - Including: execution, verification, error handling

3. **AI Reports Rather Than Asks**
   - When encountering problems during execution, AI tries to solve them first
   - Only reports when unable to solve, with possible solutions attached

4. **"Lazy User" is the Correct Assumption**
   - System adapts to user, not user to system
   - Users should achieve the best results with minimum effort

### Anti-Patterns

| Anti-Pattern | Problem | Correct Approach |
|--------------|---------|------------------|
| Step-by-step questioning | Interrupts user flow | Collect all necessary info at once |
| List steps for user to execute | Increases user burden | AI executes autonomously and reports results |
| Assume user knows technical details | Unfriendly | Hide technical details, expose only business decisions |
| Only report errors | Not constructive | Report error + provide solutions |

### Application Scenarios

- Writing Claude Code prompts
- Designing any human-AI collaboration flow
- Dividing human/AI responsibilities in Task Decomposition
- All Skill designs requiring human-AI interaction

### Origin

This principle comes from the Task Decomposition validation practice on 2025-01-31. When designing Claude Code prompts for user authentication module, the initial design assumed users would manually execute technical steps. After review, changed to AI-led execution with users only providing necessary inputs, significantly improving effectiveness.

---

## Principle 2: Verifiable Steps

### Definition

Each step after task decomposition must have clear acceptance criteria that can be objectively verified.

### Key Points

1. **Clear Completion Markers**
   - Each step ends with verifiable deliverables
   - Example: file created, test passed, service accessible

2. **Automated Verification First**
   - Verify through code/scripts rather than manual checking when possible
   - Example: run tests, check file existence, curl endpoints

3. **Locatable When Failed**
   - When verification fails, clearly indicate which step went wrong
   - Facilitates troubleshooting and fixing

### Origin

This principle comes from the acceptance criteria design for each Prompt in the Task Decomposition validation practice.

---

## Principle 3: Incremental Building

### Definition

Complex systems should start from the minimum runnable version and gradually add features.

### Key Points

1. **Skeleton Before Flesh**
   - First build the minimum runnable structure
   - Then gradually fill in business logic

2. **Each Step is Runnable**
   - Not waiting until everything is done to run
   - System remains runnable after each addition

3. **Continuous Verification**
   - Verify after each addition
   - Find problems early

### Origin

This principle comes from the design of 6 Prompts in the Task Decomposition validation practice: from project initialization (skeleton) to final initialization script (complete functionality), each step maintains system runnability.

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v1.0 | 2025-01-31 | Initial version with 3 core principles |
