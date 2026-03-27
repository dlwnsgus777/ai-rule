---
description: Use when implementing any feature or bugfix in a
  Java/Spring Boot codebase, before writing production code.
name: test-driven-development
---

# Test-Driven Development (TDD) -- Java Edition

## Overview

Write the test first.\
Watch it fail.\
Write the minimal code to make it pass.

**Core principle:**\
If you didn't watch the test fail, you don't know if it actually tests
the right behavior.

Violating the letter of these rules is violating the spirit of TDD.

------------------------------------------------------------------------

## When to Use

### Always

-   New features
-   Bug fixes
-   Refactoring
-   Behavior changes
-   Performance improvements that must preserve behavior

### Exceptions (ask your human partner)

-   Throwaway prototypes
-   Generated code
-   Pure configuration files

### Do NOT Write Tests For

-   **Simple object creation** — constructors, static factory methods, or builders that only assign fields. These have no behavior to verify.
-   **Trivial getters/setters** — if the only assertion is `assertThat(obj.getName()).isEqualTo("name")` after setting it, the test adds no value.
-   **Data classes / DTOs / records** — plain data holders without business logic do not need tests.

    ```java
    // ❌ Do not test this — no behavior
    @Test
    void createOrder() {
        Order order = new Order("id", "userId");
        assertThat(order.getId()).isEqualTo("id");
    }

    // ✅ Test this — business behavior exists
    @Test
    void order_is_cancelled_when_payment_fails() {
        Order order = new Order("id", "userId");
        order.cancelDueToPaymentFailure();
        assertThat(order.getStatus()).isEqualTo(OrderStatus.CANCELLED);
    }
    ```

Thinking "skip TDD just this once"?\
That's rationalization. Stop.

------------------------------------------------------------------------

## The Iron Law

    NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST

If you wrote production code before writing a failing test:

-   Delete it.
-   Do not keep it as reference.
-   Do not "adapt" it.
-   Do not look at it.
-   Delete means delete.

Implement fresh from the tests. Period.

------------------------------------------------------------------------

# The TDD Cycle

    RED → GREEN → REFACTOR

1.  RED -- Write a failing test.
2.  GREEN -- Write the minimal code to pass.
3.  REFACTOR -- Improve structure without changing behavior.

Repeat.

------------------------------------------------------------------------

## RED -- Write a Failing Test

Write one minimal test that expresses a single behavior.

### Good Example (JUnit 5 + AssertJ)

``` java
import org.junit.jupiter.api.Test;
import static org.assertj.core.api.Assertions.*;

class RetryServiceTest {

    @Test
    void retries_failed_operation_three_times() {
        RetryService retryService = new RetryService();

        final int[] attempts = {0};

        String result = retryService.retry(() -> {
            attempts[0]++;
            if (attempts[0] < 3) {
                throw new RuntimeException("fail");
            }
            return "success";
        });

        assertThat(result).isEqualTo("success");
        assertThat(attempts[0]).isEqualTo(3);
    }
}
```

------------------------------------------------------------------------

## Verify RED -- Watch It Fail (Mandatory)

Gradle:

    ./gradlew test --tests RetryServiceTest

Maven:

    mvn -Dtest=RetryServiceTest test

Confirm:

-   The test fails (not errors).
-   The failure message matches your expectation.
-   It fails because the feature is not implemented.

Never skip this step.

------------------------------------------------------------------------

## CHECKPOINT: RED Complete — Request Feedback (Mandatory)

After RED, you MUST stop and do the following:

1.  Show the written test code to the user.
2.  Request feedback in this format:

> "Could you provide feedback on this test?
> I'd especially appreciate input on [test case coverage / missing scenarios]."

3.  Do NOT proceed to GREEN without the user's **explicit approval**.

------------------------------------------------------------------------

## GREEN -- Minimal Production Code

``` java
public class RetryService {

    public <T> T retry(Supplier<T> supplier) {
        for (int i = 0; i < 3; i++) {
            try {
                return supplier.get();
            } catch (Exception e) {
                if (i == 2) throw e;
            }
        }
        throw new IllegalStateException("Unreachable");
    }
}
```

Only implement what the test requires. Nothing more.

------------------------------------------------------------------------

## CHECKPOINT: GREEN Complete — Request Feedback (Mandatory)

After GREEN, you MUST stop and do the following:

1.  Show the written implementation code to the user.
2.  Request feedback in this format:

> "Could you provide feedback on this implementation?
> I'd especially appreciate input on [design decisions / edge case handling]."

3.  Do NOT proceed to REFACTOR without the user's **explicit approval**.

------------------------------------------------------------------------

## REFACTOR -- Improve Structure

After all tests are green:

-   Remove duplication
-   Improve naming
-   Extract methods
-   Improve domain modeling
-   Simplify structure

Behavior must not change.

------------------------------------------------------------------------

## CHECKPOINT: REFACTOR Complete — Request Feedback (Mandatory)

After REFACTOR, you MUST stop and do the following:

1.  Show the before/after diff of the refactoring to the user.
2.  Request feedback in this format:

> "Could you provide feedback on this refactoring?
> I'd especially appreciate input on [structural changes / naming]."

3.  Do NOT proceed to the next cycle (RED) without the user's **explicit approval**.

------------------------------------------------------------------------

# Final Rule

    Production code → a test existed and failed first
    Otherwise → not TDD
