# Meta-Knowledge Extraction Skill

> **Skill Type**: Meta-cognition / Knowledge Extraction  
> **Applicable Scenarios**: Extract reusable insights from human-AI collaboration history  
> **Version**: v0.1 (Pending Validation)  
> **Source**: Extracted from Voice Model Platform project collaboration experience

---

## Overview

This Skill guides AI Agents in extracting reusable meta-knowledge from collaboration history.

### Why This Skill?

```
Human Knowledge Transfer:
    One person reflects → Writes a book → Others spend years learning → Some master it
    High loss, slow speed

AI Knowledge Transfer:
    One instance + Human collaboration → Extract into Skill → All instances instantly available
    Nearly lossless, near light speed
```

**Key Insight**: AI's "memory" must be externalized into documents (Skills), otherwise it's forgotten in the next conversation.

### Design Goals

| Goal | Description |
|------|-------------|
| **Guarantee Floor** | Any AI following this Skill produces "passable" meta-discoveries |
| **No Ceiling Guarantee** | Genius-level insights require additional factors (context, luck, human guidance) |
| **Verifiable** | Outputs can be validated through subsequent project practice |

---

## Inputs

| Input | Description | Required |
|-------|-------------|:--------:|
| **Collaboration History** | Conversation records, document sequences, decision logs | ✅ |
| **Collaboration Goal** | What this collaboration aimed to accomplish | ✅ |
| **Constraints/Context** | Time, resources, technical limitations, background info | 🔶 |

---

## Execution Steps

### Step 1: Node Identification

**Goal**: List key nodes in the collaboration process

Key nodes include:
- Milestones (phase completion)
- Deliverables (documents, code, designs)
- Decision points (tech selection, priority adjustments)
- Unexpected events (problem discovery, direction changes)

**Output Format**:

```markdown
## Collaboration Node List

| # | Node | Type | Time |
|---|------|------|------|
| 1 | PRD Complete | Milestone | Day 1 |
| 2 | Tech Selection Confirmed | Decision | Day 2 |
| 3 | Permission model complexity underestimated | Unexpected | Day 3 |
| ... | ... | ... | ... |
```

---

### Step 2: Node Analysis

**Goal**: Deep analysis of each node

For each node, answer these questions:

#### 2.1 Role Division
- What did the human do?
- What did the AI do?
- Was the division reasonable? Could it be better?

#### 2.2 Difficulty Assessment
- What went smoothly? Why?
- What was difficult? Why?
- Was the difficulty avoidable?

#### 2.3 Unexpected Discoveries
- What was surprising?
- Was this surprise positive or negative?
- What can be learned from it?

#### 2.4 Dependency Analysis
- What required external information? (human judgment, real-world data)
- What could be derived internally? (logical reasoning, pattern application)

**Output Format**:

```markdown
## Node Analysis: [Node Name]

**Role Division**:
- Human: [description]
- AI: [description]
- Room for improvement: [if any]

**What Went Well**: [description + reason]

**Difficulties**: [description + reason + avoidable?]

**Unexpected Discovery**: [description + nature + learning point]

**Dependency Analysis**:
- External dependencies: [list]
- Internal derivation: [list]
```

---

### Step 3: Pattern Recognition

**Goal**: Find reusable patterns across nodes

Look for these types of patterns:

#### 3.1 Recurring Phenomena
Phenomena that appear in multiple nodes may be patterns.

**Examples**:
- "Every time permission design was involved, human confirmation was needed"
- "AI is more reliable generating code scaffolds than filling in details"

#### 3.2 Evolution Trends
Phenomena that change progressively over time may be evolution patterns.

**Examples**:
- "Collaboration mode evolved from 'human instructs → AI executes' to 'AI proposes → human decides'"
- "Trust level gradually increased with successful deliveries"

#### 3.3 Boundary Conditions
Comparisons that reveal boundaries.

**Examples**:
- "Simple tasks AI can complete autonomously; complex tasks need human decomposition"
- "Structured output is more reliable than free-form text"

---

### Step 4: Naming and Structuring

**Goal**: Create reusable expressions for each pattern

For each identified pattern:

1. **Name it**: Concise, memorable name
2. **Core Insight**: One-sentence description
3. **Concrete Examples**: 2-3 supporting examples
4. **Application Scenarios**: When to use this insight

**Output Format**:

```markdown
## Meta-Discovery: [Pattern Name]

**Core Insight**: [one sentence]

**Examples**:
1. [Example 1]
2. [Example 2]
3. [Example 3]

**Application Scenarios**: [when to use]

**Caveats**: [if any]
```

---

### Step 5: Validation and Supplementation

**Goal**: Ensure quality of meta-discoveries

For each meta-discovery, check:

| Check Item | Question | Action |
|------------|----------|--------|
| **Counter-examples** | Are there cases that don't fit this pattern? | If yes, refine wording or note boundary conditions |
| **Actionability** | Can this insight guide action? | If not, consider if worth keeping |
| **Reusability** | Does this insight apply to other projects? | If only current project, lower priority |
| **Novelty** | Is this new discovery or common sense? | If common sense, consider if worth recording |

---

## Outputs

### Primary Deliverables

```markdown
# Meta-Knowledge Extraction Report

## 1. Collaboration Overview
- Collaboration Goal: [description]
- Collaboration Period: [timeframe]
- Participants: [human/AI]

## 2. Key Nodes
[Step 1 output]

## 3. Node Analysis
[Step 2 output]

## 4. Meta-Discovery List
[Step 4 output, sorted by importance]

## 5. Items Pending Validation
- [Hypotheses to validate in future projects]

## 6. Omissions and Limitations
- [Content this extraction may have missed]
- [Limitations of the methodology itself]
```

### Optional Deliverables

- [ ] New Skill document (if independently reusable capability discovered)
- [ ] Update suggestions for existing Skills
- [ ] Collaboration pattern improvement suggestions

---

## Usage Examples

### Trigger Phrases

> "I want to extract reusable insights from this collaboration history"

> "We completed PRD through tech selection, help me extract meta-knowledge"

> "Looking back at our conversation, what patterns are worth recording?"

### Example Dialog

**Human**: We just finished database design, help me extract meta-knowledge from this process.

**AI**: Sure, let me analyze using the meta-knowledge extraction framework.

First, I identify these key nodes:
1. Requirements clarification (confirming entities and relationships)
2. Initial ER diagram (AI output)
3. Human review (pointed out omissions)
4. Iterative refinement (3 rounds)
5. Final confirmation

Next, I'll analyze each node...

[Continue analysis]

---

## Known Limitations

### 1. Context Dependency

This Skill's effectiveness highly depends on:
- Completeness of collaboration history
- Background information provided by humans
- AI's current conversation context window

### 2. Post-hoc Nature of Reflection

**Problem**: This framework is "post-hoc" reflection, not "real-time" reflection.

**Impact**: May miss subtle details during the process.

**Mitigation**: Encourage periodic mini-retrospectives during collaboration.

### 3. No Ceiling Guarantee

**Design Goal**: Guarantee "passable", not guarantee "excellent".

Excellent meta-discoveries require:
- Richer context
- Sharper human insight
- A bit of luck

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v0.1 | 2025-01-30 | Initial version, pending real project validation |

---

## Appendix: Cognitive Development Ladder Reference

This Skill's design is inspired by the human cognitive development ladder:

```
Level 0: Instinctive Response (hardcoded)
      ↓
Level 1: Pattern Copying (unconscious learning)
      ↓
Level 2: Conscious Expression (purposeful action)
      ↓
Level 3: Reflection (review and improve) ← This Skill primarily operates here
      ↓
Level 4: Externalization (transferable knowledge) ← This Skill's deliverables
```

For AI, Level 4 (externalization) is not a "higher" option—it's the **only** way to retain knowledge.

This is why MIMIR exists—it's AI's "external memory system".
