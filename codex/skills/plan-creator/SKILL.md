---
name: plan-creator
description: Writes a structured Markdown plan document for any task, feature, or project.
  Use when the user requests a "계획 문서", "구현 계획", "실행 계획", "계획 MD로 정리",
  "계획서 작성", or "plan document".
---

# Plan Creator

## When to Use

Trigger this skill when the user asks to write, create, or organize a plan, implementation plan,
or execution plan as a Markdown document.

## Process

### Step 1: Gather Info

Before writing, confirm:

- Title and purpose of the plan
- Background or problem being solved
- Key steps or phases (if already known)
- Any constraints, dependencies, or scope boundaries

If any of these are unclear, **ask first**. Do not generate the document without sufficient context.

### Step 2: Write the Plan Document

Use the fixed template below. Fill every section — omit only if truly not applicable.

- Goals and Tasks → use checkboxes (`- [ ]`)
- Each step → include a code snippet if the task involves code or configuration
- Scope → table format

### Step 3: Request Feedback (Mandatory)

After writing the document, stop and ask:

> "Could you provide feedback on this plan document?
> I'd especially appreciate input on [step structure / missing items / scope]."

Do NOT proceed to implementation without explicit approval.

---

## Output Template

```markdown
# [Plan Title]

## Overview
[2-3 sentence summary of purpose and background]

## Goals
- [ ] [Outcome 1]
- [ ] [Outcome 2]

## Scope
| Item | Included | Notes |
|---|---|---|
| [item] | ✅ / ❌ | [note] |

## Step-by-Step Plan

### Step 1: [Name]
- **Purpose**: [what this step achieves]
- **Tasks**:
  - [ ] [task 1]
  - [ ] [task 2]
- **Output**: [deliverable]
- **Dependencies**: [prerequisites]

**Example:**
\`\`\`[language]
// code snippet showing the key implementation or structure
[code]
\`\`\`

### Step 2: [Name]
- **Purpose**: [what this step achieves]
- **Tasks**:
  - [ ] [task 1]
  - [ ] [task 2]
- **Output**: [deliverable]
- **Dependencies**: [prerequisites]

## Acceptance Criteria
- [ ] [check 1]
- [ ] [check 2]

## Risks
| Risk | Impact | Mitigation |
|---|---|---|
| [risk] | High/Med/Low | [action] |

## Notes
- [additional context or links]
```
