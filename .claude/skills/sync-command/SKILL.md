---
name: sync-command
description: Copies a command from the global ~/.claude/commands/ into this project's
  .claude/commands/ directory. Use when the user wants to copy or update a command
  from their global config into the project. Trigger on "커맨드 복사", "커맨드 동기화",
  "커맨드 최신화", "sync command", "copy command", or any phrase like
  "[커맨드명] 커맨드 프로젝트에 복사/추가/최신화".
---

# Sync Command

Copies or updates a command from `~/.claude/commands/` into `.claude/commands/` of the current project.

## Usage

User specifies a target command name, e.g.:
- "branch-review 커맨드 동기화해줘"
- "review 커맨드 프로젝트에 복사해줘"
- "start-task 커맨드 최신화해줘"

If no command name is given, ask the user which command to sync using `AskUserQuestion`.

---

## Process

### Step 1: Resolve Command Name

Extract the target command name from the user's message (without `.md` extension).

If ambiguous or not provided:
- List available global commands via `ls ~/.claude/commands/`
- Ask the user to pick one using `AskUserQuestion`

---

### Step 2: Inspect Both Locations

Run the following checks in parallel:

1. **Global command** — `~/.claude/commands/{command-name}.md`
   - Read full content

2. **Project command** — `.claude/commands/{command-name}.md`
   - Check if it exists; if so, read full content for comparison

---

### Step 3: Sync Global → .claude/commands/

| Situation | Action |
|---|---|
| Project command does not exist | **Copy** from global to `.claude/commands/` |
| Project command exists, content identical | No changes needed |
| Project command exists, content differs | **Overwrite** with global version |

Always ensure `.claude/commands/` directory exists (`mkdir -p`) before writing.

---

### Step 4: Report

After the operation is complete, summarize:

```
## 동기화 결과: {command-name}

### .claude/commands/
- {command-name}.md: [복사됨 / 업데이트됨 / 변경 없음]

변경된 내용:
- [추가되거나 변경된 주요 내용 요약]
```

Then ask:
> "동기화 결과를 확인해 주세요. 수정이 필요한 부분이 있으신가요?"
