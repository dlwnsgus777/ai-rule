---
name: devlife-brainstorming
description: Explore ideas conversationally to define what/why/key design decisions, then produce a brainstorming document.
  First step in the devlife workflow chain — hands off to spec-creator.
  Trigger on "브레인스토밍", "아이디어 정리", "기능 구상", "devlife-brainstorming",
  "뭘 만들지 정리", "아이디어 탐색", "기획 정리".
---

# Devlife Brainstorming

## Purpose

Turn a vague idea into concrete design decisions. This skill covers **what** to build and **why**.
Technical implementation (how) is handled by spec-creator and plan-creator.

<HARD-GATE>
Do not write implementation code, scaffold files, or start implementation planning until the
brainstorming document has been written and the user has explicitly approved it.
</HARD-GATE>

**Position in the workflow chain:**
```
devlife-brainstorming  (what/why)
        ↓
   spec-creator        (technical spec + subtask breakdown)
        ↓
   plan-creator        (per-task implementation detail)
        ↓
     tdd-team          (TDD execution)
```

---

## Process

### Step 1: Scan Project Context

Before asking questions, briefly scan the current project state:
- Recent commits (`git log --oneline -5`)
- Key files in project root
- Titles of any existing spec/plan docs

This makes questions concrete and contextual. Do NOT report findings to the user — proceed directly to Step 2.

### Step 2: Explore the Idea (Conversational)

**Rules:**
- Ask **one question at a time**
- Wait for the answer before asking the next question
- Prefer open-ended questions; offer A/B/C choices only when options are clear
- Ask in Korean

**Topics to cover (order is flexible):**
- [ ] **Core problem**: What problem does this solve?
- [ ] **Target users**: Who uses it? In what situation?
- [ ] **Success criteria**: How will you know it's done well?
- [ ] **Scope**: What must be in, and what is explicitly out?
- [ ] **Key design decisions**: Any trade-offs that determine the direction?
- [ ] **Constraints**: Technical, timeline, or business constraints?

**When to stop asking:**

Once the checklist is mostly covered, proactively check in:

> "지금까지 나온 내용으로 brainstorming 문서를 작성해도 괜찮을까요? 더 다루고 싶은 부분이 있으신가요?"

- Confirmed → proceed to Step 3
- More to discuss → continue questions
- User says "그만 물어봐" / "이 정도면 됐어" → mention any uncovered item once, then follow the user's call

### Step 3: Present Direction Options

Before writing the document, present 2-3 possible product/design directions with trade-offs.
Lead with the recommended direction and explain why it best fits the user's answers.

Ask:

> "이 방향으로 brainstorming 문서를 작성해도 괜찮을까요?"

- Approved → proceed to Step 4
- Changes requested → revise the direction options or ask one more clarifying question

### Step 4: Write the Brainstorming Document

**Filename**: `brainstorming-{topic}.md` (e.g. `brainstorming-payment-refund.md`)
**Location**: Project root (current working directory)

```markdown
# Brainstorming: {Feature Name}

## Core Problem

{What problem this solves and why it needs to exist — 2-3 sentences}

## Target Users / Context

{Who uses it, when, and in what situation}

## Success Criteria

{How to judge whether this was built well — the more specific the better}

## Scope

### In
- {Must-have for this iteration}

### Out
- {Explicitly excluded from this iteration}

## Key Design Decisions

{Trade-offs and the chosen direction — only if decisions were made}

## Constraints

{Technical, timeline, or business constraints — only if applicable}
```

### Step 5: Self-Review

Before showing the file to the user, review it yourself and fix issues inline:
- Placeholder scan: no `TBD`, `TODO`, or empty sections
- Consistency: scope, success criteria, and key decisions do not contradict each other
- Ambiguity: any decision with multiple interpretations is made explicit
- YAGNI: no feature is added unless it came from the conversation

### Step 6: Request Feedback

After writing the document:

> "brainstorming 문서를 `{file path}`에 작성했습니다. 수정하거나 추가할 내용이 있으신가요?"

Apply any requested changes and re-confirm.

### Step 7: Hand Off to spec-creator (Terminal State)

Once the document is approved, ask:

> "spec-creator로 이어서 기술 명세를 작성할까요, 아니면 여기서 마칠까요?"

- Continue → if `spec-creator` is available, read its `SKILL.md` and continue with the brainstorming document path as input. If it is not available, say `not available` and share the document path.
- Stop → share the document path and exit

---

## Principles

- **what/why only** — implementation (how) belongs to spec-creator and plan-creator
- **One question at a time** — never ask multiple questions in one message
- **Never guess** — if something is unclear, ask; don't fill in the blanks
- **YAGNI** — don't add scope the user didn't mention
