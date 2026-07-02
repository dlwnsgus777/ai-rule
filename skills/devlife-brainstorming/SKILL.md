---
name: devlife-brainstorming
description: Explore ideas conversationally to define what/why/design decisions, then produce an approved design spec document.
  First step in the devlife workflow chain — hands off to plan-creator.
  Trigger on "브레인스토밍", "아이디어 정리", "기능 구상", "devlife-brainstorming",
  "뭘 만들지 정리", "아이디어 탐색", "기획 정리".
---

# Devlife Brainstorming

## Purpose

Turn a vague idea into an approved design spec. This skill covers **what** to build, **why**, and **how it's designed** (architecture, components, data flow, error handling, testing).
Implementation breakdown (which files, step-by-step tasks, commit units) is handled by plan-creator.

<HARD-GATE>
Do not write implementation code, scaffold files, or start implementation planning until the
brainstorming document has been written and the user has explicitly approved it.
</HARD-GATE>

**Position in the workflow chain:**
```
devlife-brainstorming  (what/why + design spec)
        ↓
   plan-creator        (구현 계획 — 파일/step/테스트/커밋 단위)
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

This makes questions concrete and contextual. Do NOT report findings to the user — proceed directly to Step 1.5.

### Step 1.5: Scope Size Check

Before asking the first question, assess whether the idea contains multiple independent subsystems.

- **Single system or feature** → proceed to Step 2
- **Multiple independent subsystems** (e.g., "platform with chat, billing, file storage, analytics")
  → flag immediately:
    > "Your idea contains several independent pieces. It's better to tackle them one at a time.
    >  Which would you like to start with — [A], [B], or [C]?"
  → brainstorm only the first sub-project; each gets its own design spec and plan-creator cycle

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

> "I think I have enough to write the brainstorming document. Is there anything else you'd like to cover first?"

- Confirmed → proceed to Step 3
- More to discuss → continue questions
- User says "stop asking" / "that's enough" → mention any uncovered item once, then follow the user's call

### Step 3: Present Direction Options

Before presenting the design, propose 2-3 possible product/design directions with trade-offs.
Lead with the recommended direction and explain why it best fits the user's answers.

Ask:

> "이 방향이 맞나요? 이걸 바탕으로 설계를 발표하겠습니다."

- Approved → proceed to Step 4
- Changes requested → revise the direction options or ask one more clarifying question

### Step 4: Present Design Sections

Once the direction is approved, present the design **section by section** and get confirmation after each.

**Sections to cover (in order):**
1. **Architecture** — overall structure, layers, main modules
2. **Components** — key classes/modules, responsibilities, interfaces
3. **Data Flow** — how data moves through the system, key transformations
4. **Error Handling** — failure modes, error boundaries, recovery strategies
5. **Testing** — test strategy, key scenarios, test boundaries

**Scale each section to its complexity:**
- Simple feature: a few sentences
- Complex system: up to 200-300 words

After each section, ask:

> "이 [섹션명] 방향이 맞나요?"

- Confirmed → proceed to the next section
- Changes requested → revise and re-present that section before continuing

Once all sections are confirmed → proceed to Step 5.

### Step 5: Write the Design Spec Document

**Filename**: `YYYY-MM-DD-{topic}.md` (e.g. `2026-07-02-payment-refund.md`)
**Location**: `docs/brainstorming/` (create the directory if it does not exist)

```markdown
# Design Spec: {Feature Name}

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

{Trade-offs and the chosen direction}

## Architecture

{Overall structure, layers, main modules — scaled to complexity}

## Components

{Key classes/modules, responsibilities, interfaces}

## Data Flow

{How data moves through the system, key transformations}

## Error Handling

{Failure modes, error boundaries, recovery strategies}

## Testing Strategy

{Test strategy, key scenarios, test boundaries}

## Constraints

{Technical, timeline, or business constraints — only if applicable}
```

### Step 6: Self-Review

Before showing the file to the user, review it yourself and fix issues inline:
- Placeholder scan: no `TBD`, `TODO`, or empty sections
- Consistency: scope, success criteria, and design sections do not contradict each other
- Ambiguity: any decision with multiple interpretations is made explicit
- YAGNI: no feature is added unless it came from the conversation

### Step 7: User Review Gate

After writing the document, present the path and wait for explicit approval:

> "설계 spec 문서를 `{file path}`에 작성했습니다.
>  파일을 열어 확인해주세요. 준비되시면 **'승인'**이라고 말씀해주세요.
>  수정할 부분이 있으면 알려주시면 반영하겠습니다."

- **승인** → proceed to Step 8
- **change request** → apply changes, then repeat Step 7 (do NOT auto-proceed)
- **"다음" / "계속" without reviewing** → ask once:
  > "문서를 확인하셨나요? 준비되시면 '승인'이라고 말씀해주세요."

### Step 8: Hand Off to plan-creator (Terminal State)

Once the document is approved:

> "설계 spec이 완성되었습니다. 다음 단계는 plan-creator로 구현 계획을 작성하는 것입니다. 이어서 진행할까요?"

- **Yes / continue** → invoke plan-creator with the design spec document path as input
- **No / stop here** → share the document path and exit

**If plan-creator is not available:**
> "plan-creator를 찾을 수 없습니다. 문서 경로: `{file path}`"

---

## Principles

- **what/why + design** — covers direction, architecture, components, dataflow, error handling, testing; implementation breakdown (how exactly) belongs to plan-creator
- **Scale to complexity** — design sections can be a few sentences or up to 200-300 words; match the depth to the task
- **One question at a time** — never ask multiple questions in one message
- **Never guess** — if something is unclear, ask; don't fill in the blanks
- **YAGNI** — don't add scope the user didn't mention
