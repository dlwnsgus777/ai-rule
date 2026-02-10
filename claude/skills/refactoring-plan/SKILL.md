# Refactoring Plan

## Trigger
- `.java` file path provided
- "리팩터링 계획" or "refactoring plan" requested

## Process

1. **Read file** via `view` tool
2. **Analyze** using 4 criteria
3. **Output** prioritized plan

## Analysis Criteria

**Readability**: Clear naming, method length <20 lines, nesting <3 levels
**DRY**: Duplicated code blocks, similar patterns
**SOLID**: Single responsibility, proper abstractions, correct inheritance
**Cohesion**: Related methods grouped, focused class purpose

### Spring/JPA Checks (if applicable)
- `@Transactional` scope
- N+1 query risks in JPA
- SQL injection in MyBatis

## Output Template
````markdown
## 🔍 [ClassName] Refactoring Plan

### Issues: [N]🔴 [N]🟡 [N]🟢

### 🔴 High Priority

**[Title]** (L[X-Y])
Problem: [Description]
```java
// Current
[code]
```
Solution:
```java
// Refactored
[code]
```
Benefit: [Why this matters]

### 🟡 Medium / 🟢 Low
[Same format, condensed]

### Steps

1. **[Action]** → `[file]`
   - Change: [What]
   - Why: [Benefit]
   - Risk: [Low/Medium/High]

2. **[Action]** (after Step 1)
   - Change: [What]

### Next: Start Step 1? / See details? / Adjust?
````

## Rules

✅ Include actual code (3-10 lines)
✅ Reference exact line numbers
✅ Consider performance (queries, indexes)
✅ Spring patterns (`@Transactional` placement)

❌ No abstract suggestions without code
❌ No changes that break functionality