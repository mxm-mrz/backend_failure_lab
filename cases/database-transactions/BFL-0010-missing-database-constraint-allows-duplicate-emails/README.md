<p align="right">
  <a href="README.md">English</a> |
  <a href="README.ru.md">Русский</a>
</p>

# Missing Database Constraint Allows Duplicate Emails

## Summary

The backend checks whether an email already exists before creating a user, but the database does not enforce uniqueness.

This case teaches that application-level checks are not enough for data invariants.

## Metadata

- ID: `BFL-0010`
- Category: `database-transactions`
- Secondary categories: `security-auth`, `reliability-failure-recovery`
- Level: `junior`
- Technologies: `python`, `fastapi`, `postgresql`, `sqlalchemy`, `pytest`
- Failure modes: `race-condition`
- Patterns: `transaction-boundary`
- Status: `draft`

## Problem

The code checks for an existing user with the same email, then inserts a new user if none is found.

That works for normal sequential requests, but it is not a safe database invariant. Two concurrent requests can both check before either one commits, then both insert the same email.

## Why This Happens in Production

This bug appears when uniqueness is treated as business logic only. The service method looks reasonable, and manual testing usually passes.

Under concurrency, the check and the insert are separate operations. Without a database constraint, the database has no reason to reject the second insert.

## Broken Scenario

1. Request A checks `john@example.com` and sees no user.
2. Request B checks `john@example.com` and also sees no user.
3. Request A inserts and commits.
4. Request B inserts and commits.
5. The database now has two users with the same email.

## Broken Implementation

The broken model defines `email` as a normal column:

```python
email = mapped_column(String, nullable=False)
```

The code relies on a pre-insert check instead of a database constraint.

## Where Exactly It Breaks

The bug is in `broken/models.py`. The `users.email` column does not have `unique=True`, so the database does not protect the invariant.

The repository check in `broken/repository.py` is useful for user-friendly errors, but it cannot be the only protection.

## How To Catch It

Simulate two sessions that both pass the precheck before the first insert is committed.

A good test should prove that the database itself rejects the duplicate. If the test only calls the endpoint twice sequentially, it can miss the race because the second request sees the first committed user.

## Failing Test

`tests/test_broken.py` expects the second insert to raise `IntegrityError`.

The broken implementation has no unique constraint, so the second insert succeeds and the test fails.

## Diagnosis

The system fails because the invariant lives only in application code.

Anything that must always be true for stored data should be enforced at the database boundary. Application checks can improve error messages, but database constraints protect the data when requests overlap.

## Fixed Implementation

The fixed model adds a unique constraint to the email column:

```python
email = mapped_column(String, nullable=False, unique=True)
```

The repository still keeps the precheck for a clean `409 Conflict` response, but the database is now the final authority. If two requests race, one insert succeeds and the other fails safely.

## How to Run

From the repository root:

### Broken version

```bash
make broken CASE=BFL-0010
```

Expected: the test should fail because duplicate emails can be inserted when both requests pass the precheck.

### Fixed version

```bash
make fixed CASE=BFL-0010
```

Expected: the tests should pass because the database rejects the duplicate email.

## Files

- `broken/` — intentionally unsafe implementation
- `fixed/` — corrected implementation
- `tests/` — tests that demonstrate the failure and verify the fix

## Production Notes

- Use database constraints for invariants like unique email, unique username, or unique external ID.
- Keep application prechecks for clearer API responses, but do not rely on them alone.
- Handle database integrity errors and return a consistent client error such as `409 Conflict`.
- Add tests that model overlapping sessions, not only sequential requests.

## Trade-Offs

A precheck gives friendly errors and avoids unnecessary insert attempts in common cases.

A unique constraint is still required because it protects the system under concurrency. The trade-off is that code must handle database errors cleanly instead of assuming the precheck is always enough.
