---
name: sync-skill
description: Syncs a skill from the global ~/.claude/skills/ into this project's
  claude/skills/ directory. Use when the user wants to copy or update a skill from
  their global config into the project. Trigger on "스킬 복사", "스킬 동기화", "스킬 최신화",
  "sync skill", "copy skill", or any phrase like "[스킬명] 스킬 프로젝트에 복사/추가/최신화".
---

# Sync Skill

Copies or merges a skill from `~/.claude/skills/` into `claude/skills/` of the current project.

## Usage

User specifies a target skill name, e.g.:
- "plan-creator 스킬 동기화해줘"
- "tdd 스킬 프로젝트에 복사해줘"
- "backend-standards 스킬 최신화해줘"

If no skill name is given, ask the user which skill to sync using `AskUserQuestion`.

---

## Process

### Step 1: Resolve Skill Name

Extract the target skill name from the user's message.

If ambiguous or not provided:
- List available global skills via `ls ~/.claude/skills/`
- Ask the user to pick one using `AskUserQuestion`

---

### Step 2: Inspect Both Sides

Run the following checks in parallel:

1. **Global skill exists?**
   - Path: `~/.claude/skills/{skill-name}/`
   - List all files inside (SKILL.md, assets/, etc.)

2. **Project skill exists?**
   - Path: `claude/skills/{skill-name}/`
   - If it exists, read `SKILL.md` for comparison

---

### Step 3: Determine Action

| Situation | Action |
|---|---|
| Project skill does not exist | **Copy** all files from global to project |
| Project skill exists, content identical | Report "이미 최신 상태입니다" — no changes needed |
| Project skill exists, content differs | **Merge** — see Step 4 |

---

### Step 4: Merge (when both versions exist and differ)

Read both `SKILL.md` files fully, then produce a merged result:

**Merge rules (apply in order):**
1. Keep all trigger patterns from **both** versions (union)
2. For procedural content (steps, rules), prefer the version with **more detail**
3. If a section exists only in one version, include it
4. Never silently drop content — if two versions conflict in meaning, include both and add a comment `<!-- merged: check this section -->`

Write the merged `SKILL.md` to `claude/skills/{skill-name}/SKILL.md`.

For asset files (e.g., `assets/*.md`):
- If identical: no action
- If project version is missing: copy from global
- If both exist and differ: show a brief diff summary and ask the user which to keep via `AskUserQuestion`

---

### Step 5: Report

After writing files, summarize:

```
## 동기화 결과: {skill-name}

- SKILL.md: [복사됨 / 병합됨 / 변경 없음]
- assets/{file}: [복사됨 / 변경 없음 / 사용자 선택 필요]

변경된 내용:
- [추가된 트리거 패턴]
- [추가된 섹션]
- [기타 변경사항]
```

Then ask:
> "동기화 결과를 확인해 주세요. 수정이 필요한 부분이 있으신가요?"
