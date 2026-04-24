# Backend Development Standards

## AUTO-TRIGGER
Apply to ALL backend tasks: planning, coding, testing. Read BEFORE starting.

## Core Principles

**SOLID**
- **SRP**: 1 class = 1 responsibility
- **OCP**: Extend via interface, not modification
- **LSP**: Subtypes must be substitutable
- **ISP**: Small, specific interfaces — not fat ones
- **DIP**: Depend on abstractions; interfaces live in Domain, impls in Infrastructure

**Clean Architecture** (dependency: outside → inside, never reversed)

```
Domain          (core — zero external deps)
Application     (uses Domain only)
Infrastructure  (implements Domain interfaces)
Presentation    (uses Application)
```

**Layer responsibilities**
- **Domain**: Entities, value objects, domain interfaces, business rules
- **Application**: Use cases, DTOs, orchestration (no framework deps)
- **Infrastructure**: DB, external APIs, messaging — implements Domain interfaces
- **Presentation**: Controllers, endpoints, serialization

**TDD Flow**
RED → GREEN → REFACTOR
1. Write failing test FIRST
2. Minimal code to pass
3. Refactor while green

**Pragmatic Development**
- Solve ACTUAL problem, not imaginary ones
- No "we might need..." features
- Add complexity ONLY when needed NOW
- YAGNI (You Aren't Gonna Need It)

## Planning Template

**Problem Definition**
- What problem RIGHT NOW? [state clearly]
- What's NOT included? [list out-of-scope]

**Layer mapping**
```
Domain:         entities, interfaces
Application:    usecases, DTOs
Infrastructure: implementations
Presentation:   controllers, endpoints
```

**TDD Implementation Order**
```
Per layer: Test → Code → Refactor
Order: Domain → Application → Infrastructure → Presentation
```

## Test Strategy Per Layer

| Layer          | Approach                        | Tool              |
|----------------|---------------------------------|-------------------|
| Domain         | Pure unit — no mocks            | JUnit 5 + AssertJ |
| Application    | Mock repositories               | Mockito           |
| Infrastructure | Integration tests               | Testcontainers    |
| Presentation   | API tests                       | MockMvc           |

**Test naming:** `methodName_scenario_expected`

## Anti-Patterns

**Architecture**
❌ Controller accesses DB directly
❌ Entity used as API response
❌ Business logic in Controller
❌ Domain depends on Infrastructure
❌ Interface defined in Infrastructure (belongs in Domain)

**Over-engineering**
❌ Abstract classes "for future flexibility" when only 1 impl exists
❌ Caching added without a measured performance problem
❌ Patterns added "just in case"
❌ Features not in current requirements

**TDD**
❌ Code before test
❌ Multiple behaviors in 1 test
❌ Mocking domain objects

## Validation Checklist

- [ ] Solves actual requirement (not a future "what if")?
- [ ] Tests written first?
- [ ] Single responsibility per class?
- [ ] Dependencies point inward only?
- [ ] Interfaces defined in Domain, impls in Infrastructure?
- [ ] No unnecessary abstraction?
- [ ] DTOs used at every layer boundary?

## Key Rules

- Test first, code second, refactor third
- Domain = pure logic, zero external deps
- Interfaces in Domain, impls in Infrastructure
- DTOs for layer boundaries
- Dependencies flow: outside → inside
- Implement ONLY what's needed NOW
- Add complexity when requirement CONFIRMED

## Mandatory Actions

1. Read this skill BEFORE any work
2. Define problem & non-goals
3. Write test FIRST
4. Design layers before writing code
5. Implement with TDD
6. Validate against checklist
