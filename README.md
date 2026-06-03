# Backend Failure Lab

Learn backend engineering through real production failure cases.

Backend Failure Lab is a practical training repository for Python backend developers. Each case follows the same flow:

`broken code -> failing test -> diagnosis -> fix -> production notes`

**Start here → [BFL-0001: User Can Read Another User's Order](cases/security-auth/BFL-0001-user-can-read-another-users-order)**  
Level: Beginner · ~10 minutes · Requires: Docker Desktop

## Try it in 3 steps

**1. See the bug**
```bash
make broken CASE=BFL-0001
# Expected: test fails — this is the bug you need to fix
```

**2. Fix it yourself**
```bash
# Open and edit the broken implementation:
# cases/security-auth/BFL-0001-user-can-read-another-users-order/broken/app.py
```

**3. Check your fix** *(coming in next release)*
```bash
make grade CASE=BFL-0001
# ✓ All tests passed. Your fix works.
# View the canonical solution: cases/.../fixed/
```

## How It Works

Each case is a small, focused backend failure scenario. A case starts with a broken implementation and a failing test, then walks through the diagnosis, the corrected implementation, and notes about how the same issue appears in production systems.

Cases are meant to be practical, reproducible, and easy to review in pull requests.

## Prerequisites

- [Docker Desktop 4.x+](https://www.docker.com/products/docker-desktop/) — required for `make broken` and `make fixed`
- macOS, Linux, or WSL2 on Windows
- No local Python setup needed

## Run a Case

The recommended way is Docker. You do not need to install Python dependencies locally.

`CASE` is the case ID from `case.yaml`.

Broken version:

```bash
make broken CASE=BFL-0001
```

Expected result: the broken test is expected to fail because it demonstrates the bug.

Fixed version:

```bash
make fixed CASE=BFL-0001
```

Expected result: the fixed test should pass.

## Suggest a Failure Case

You do not need to write code to contribute.

If you have seen a real backend failure in practice, you can suggest it as a future case:

- auth bugs
- N+1 queries
- unsafe retries
- stale cache
- race conditions
- transaction bugs
- background job failures
- missing request IDs
- observability/debugging problems

Open a **Failure Case Proposal** issue and describe the scenario.

A maintainer can later turn it into:

`broken code -> failing test -> diagnosis -> fix -> production notes`

## Ways to Contribute

You can help by:

1. Suggesting a real backend failure case.
2. Improving an existing case explanation.
3. Adding tests.
4. Implementing broken/fixed examples.
5. Adding diagrams.
6. Reviewing whether a case feels realistic.

## Featured Cases

| Case | Category | Level | Time |
|------|----------|-------|------|
| [BFL-0001 — User Can Read Another User's Order](cases/security-auth/BFL-0001-user-can-read-another-users-order) | Security / Auth | Beginner | ~10 min |
| [BFL-0002 — N+1 Queries Hidden Behind a Simple Endpoint](cases/database-transactions/BFL-0002-n-plus-one-queries-hidden-behind-simple-endpoint) | Database / Transactions | Junior | ~15 min |
| [BFL-0003 — Retry Without Idempotency Creates Duplicate Orders](cases/idempotency-consistency/BFL-0003-retry-without-idempotency-creates-duplicate-orders) | Idempotency / Consistency | Junior-Middle | ~15 min |
| [BFL-0004 — Lost Update When Two Requests Change Balance](cases/database-transactions/BFL-0004-lost-update-when-two-requests-change-balance) | Database / Transactions | Middle | ~20 min |
| [BFL-0005 — Missing Request ID Makes Debugging Impossible](cases/observability-debugging/BFL-0005-missing-request-id-makes-debugging-impossible) | Observability / Debugging | Beginner | ~10 min |

## Browse by Category

<details open>
<summary><strong>API & HTTP</strong></summary>

1. **BFL-0001** — [User Can Read Another User's Order](cases/security-auth/BFL-0001-user-can-read-another-users-order)  
   Level: Beginner · Status: Released
2. **BFL-0005** — [Missing Request ID Makes Debugging Impossible](cases/observability-debugging/BFL-0005-missing-request-id-makes-debugging-impossible)  
   Level: Beginner · Status: Released
3. **BFL-0007** — [Blocking Code Inside Async Endpoint](cases/performance-scaling/BFL-0007-blocking-code-inside-async-endpoint)  
   Level: Junior · Status: Draft
4. **BFL-0008** — [Offset Pagination Skips or Duplicates Items](cases/api-http/BFL-0008-offset-pagination-skips-or-duplicates-items)  
   Level: Junior · Status: Draft
5. **BFL-0011** — [Timeout Without Cancellation Leaves Work Running](cases/api-http/BFL-0011-timeout-without-cancellation-leaves-work-running)  
   Level: Middle-Advanced · Status: Draft

</details>

<details open>
<summary><strong>Database & Transactions</strong></summary>

1. **BFL-0002** — [N+1 Queries Hidden Behind a Simple Endpoint](cases/database-transactions/BFL-0002-n-plus-one-queries-hidden-behind-simple-endpoint)  
   Level: Junior · Status: Released
2. **BFL-0004** — [Lost Update When Two Requests Change Balance](cases/database-transactions/BFL-0004-lost-update-when-two-requests-change-balance)  
   Level: Middle · Status: Released
3. **BFL-0008** — [Offset Pagination Skips or Duplicates Items](cases/api-http/BFL-0008-offset-pagination-skips-or-duplicates-items)  
   Level: Junior · Status: Draft
4. **BFL-0009** — [Background Job Commits Too Early Before External Call](cases/queues-background-jobs/BFL-0009-background-job-commits-too-early-before-external-call)  
   Level: Middle · Status: Draft
5. **BFL-0010** — [Missing Database Constraint Allows Duplicate Emails](cases/database-transactions/BFL-0010-missing-database-constraint-allows-duplicate-emails)  
   Level: Junior · Status: Draft

</details>

<details open>
<summary><strong>Queues & Background Jobs</strong></summary>

1. **BFL-0003** — [Retry Without Idempotency Creates Duplicate Orders](cases/idempotency-consistency/BFL-0003-retry-without-idempotency-creates-duplicate-orders)  
   Level: Junior-Middle · Status: Released
2. **BFL-0009** — [Background Job Commits Too Early Before External Call](cases/queues-background-jobs/BFL-0009-background-job-commits-too-early-before-external-call)  
   Level: Middle · Status: Draft

</details>

<details open>
<summary><strong>Reliability & Failure Recovery</strong></summary>

1. **BFL-0004** — [Lost Update When Two Requests Change Balance](cases/database-transactions/BFL-0004-lost-update-when-two-requests-change-balance)  
   Level: Middle · Status: Released
2. **BFL-0005** — [Missing Request ID Makes Debugging Impossible](cases/observability-debugging/BFL-0005-missing-request-id-makes-debugging-impossible)  
   Level: Beginner · Status: Released
3. **BFL-0009** — [Background Job Commits Too Early Before External Call](cases/queues-background-jobs/BFL-0009-background-job-commits-too-early-before-external-call)  
   Level: Middle · Status: Draft
4. **BFL-0010** — [Missing Database Constraint Allows Duplicate Emails](cases/database-transactions/BFL-0010-missing-database-constraint-allows-duplicate-emails)  
   Level: Junior · Status: Draft
5. **BFL-0011** — [Timeout Without Cancellation Leaves Work Running](cases/api-http/BFL-0011-timeout-without-cancellation-leaves-work-running)  
   Level: Middle-Advanced · Status: Draft

</details>

<details open>
<summary><strong>Idempotency & Consistency</strong></summary>

1. **BFL-0003** — [Retry Without Idempotency Creates Duplicate Orders](cases/idempotency-consistency/BFL-0003-retry-without-idempotency-creates-duplicate-orders)  
   Level: Junior-Middle · Status: Released
2. **BFL-0004** — [Lost Update When Two Requests Change Balance](cases/database-transactions/BFL-0004-lost-update-when-two-requests-change-balance)  
   Level: Middle · Status: Released
3. **BFL-0006** — [Cache Returns Stale User Profile After Update](cases/caching-redis/BFL-0006-cache-returns-stale-user-profile-after-update)  
   Level: Junior · Status: Draft

</details>

<details open>
<summary><strong>Caching & Redis</strong></summary>

1. **BFL-0006** — [Cache Returns Stale User Profile After Update](cases/caching-redis/BFL-0006-cache-returns-stale-user-profile-after-update)  
   Level: Junior · Status: Draft

</details>

<details open>
<summary><strong>Security & Auth</strong></summary>

1. **BFL-0001** — [User Can Read Another User's Order](cases/security-auth/BFL-0001-user-can-read-another-users-order)  
   Level: Beginner · Status: Released
2. **BFL-0010** — [Missing Database Constraint Allows Duplicate Emails](cases/database-transactions/BFL-0010-missing-database-constraint-allows-duplicate-emails)  
   Level: Junior · Status: Draft

</details>

<details open>
<summary><strong>Observability & Debugging</strong></summary>

1. **BFL-0005** — [Missing Request ID Makes Debugging Impossible](cases/observability-debugging/BFL-0005-missing-request-id-makes-debugging-impossible)  
   Level: Beginner · Status: Released

</details>

<details open>
<summary><strong>Performance & Scaling</strong></summary>

1. **BFL-0002** — [N+1 Queries Hidden Behind a Simple Endpoint](cases/database-transactions/BFL-0002-n-plus-one-queries-hidden-behind-simple-endpoint)  
   Level: Junior · Status: Released
2. **BFL-0007** — [Blocking Code Inside Async Endpoint](cases/performance-scaling/BFL-0007-blocking-code-inside-async-endpoint)  
   Level: Junior · Status: Draft
3. **BFL-0008** — [Offset Pagination Skips or Duplicates Items](cases/api-http/BFL-0008-offset-pagination-skips-or-duplicates-items)  
   Level: Junior · Status: Draft
4. **BFL-0011** — [Timeout Without Cancellation Leaves Work Running](cases/api-http/BFL-0011-timeout-without-cancellation-leaves-work-running)  
   Level: Middle-Advanced · Status: Draft

</details>

<details open>
<summary><strong>Deployment & Operations</strong></summary>

No cases yet.

</details>

## Case ID Convention

`BFL` means `Backend Failure Lab`.

Case IDs are global, stable, and independent from categories. Categories belong in `case.yaml`, not in the ID.

Use the primary category only for the folder path:

```text
cases/security-auth/BFL-0001-user-can-read-another-users-order/
```

## Catalog Files

- [Catalog overview](catalog/README.md)
- [By category](catalog/by-category.md)
- [By technology](catalog/by-technology.md)
- [By level](catalog/by-level.md)

## Current Status

- Repository scaffold: ready
- Docker-based case runner: ready
- Released: 5 · Draft: 6 · [What is Draft?](#contributing)
- Case template: ready
- Catalog structure: ready

## Contributing

Contributions should focus on small, production-inspired backend failure cases. Start with the [case format](CASE_FORMAT.md), use the [case template](templates/case-template/), and keep each case narrow enough to explain through one failing test.

**Draft vs Released:** A Released case has been fully reviewed, verified end-to-end, and is ready to use as a learning exercise. A Draft case may have code and tests present, but it has not yet been fully checked or polished — the content could be incomplete or contain errors. If you are here to learn, start with Released cases; Draft cases are visible in the catalog but are not yet recommended for structured study. If you are a contributor, Draft cases are a good place to help — reviewing and completing them is a valuable way to contribute.