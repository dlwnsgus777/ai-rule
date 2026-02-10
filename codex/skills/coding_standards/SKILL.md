---
name: coding-standards
description: Enforce universal coding standards with focus on readability, simplicity, and long-term maintainability
scope: code-writing, code-review
priority: high
auto_trigger: true
version: 1.0
assumptions:
  - Code will be read and maintained by others
  - Simplicity and clarity are more important than cleverness
  - Premature abstraction is discouraged
---

# Coding Standards

## AUTO-TRIGGER
Apply to ALL code writing tasks.

## Code Quality Principles

**Readability First**
- Code is read more than written
- Clear variable and function names
- Self-documenting code preferred over comments
- Consistent formatting

**KISS (Keep It Simple, Stupid)**
- Simplest solution that works
- Avoid over-engineering
- No premature optimization
- Easy to understand > clever code

**DRY (Don't Repeat Yourself)**
- Extract common logic into functions
- Create reusable components
- Share utilities across modules
- Avoid copy-paste programming

**YAGNI (You Aren't Gonna Need It)**
- Don't build features before they're needed
- Avoid speculative generality
- Add complexity only when required
- Start simple, refactor when needed