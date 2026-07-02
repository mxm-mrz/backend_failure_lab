<p align="right">
  <a href="README.md">English</a> |
  <a href="README.ru.md">Русский</a>
</p>

# Timeout Without Cancellation Leaves Work Running

An API request times out, but the backend work continues and marks the export as completed.

This case teaches that response timeout and work cancellation are different backend concerns.

## Metadata

- ID: `BFL-0011`
- Category: `api-http`
- Secondary categories: `reliability-failure-recovery`, `performance-scaling`
- Level: `middle-advanced`
- Technologies: `python`, `fastapi`, `pytest`
- Failure modes: `timeout`, `partial-failure`
- Patterns: `graceful-shutdown`
- Status: `draft`

## Problem

The endpoint starts a long-running export. When the request times out, the broken implementation still completes the export in backend state.

The client receives `504 Gateway Timeout`, but the stored export says `completed`.

## Why This Happens in Production

Timeouts are often added around the response path, while the actual work keeps running in the background or in an uncancelled task.

That can leave confusing state: the client retries because it saw a timeout, but the backend may already have changed data, called another service, or produced an artifact.

## Broken Scenario

1. Export `100` is pending.
2. The client starts the export.
3. The request times out.
4. The API returns `504`.
5. The backend work still marks the export as completed.
6. The client does not know whether the operation failed or succeeded.

## Broken Implementation

The broken code treats timeout as a response error, but not as a cancellation signal:

```python
if simulate_timeout:
    export.status = "completed"
    export.result = "report.csv"
    raise ExportTimeoutError()
```

## Where the Bug Happens

The bug is in `broken/repository.py`, inside `run_export`. The timeout branch still writes final successful state.

This makes the API contract misleading: the response says the operation timed out, but the backend state says the operation completed.

## How to Catch This Bug

Assert the state after the timeout. A test that checks only `504` is too weak.

The useful test sends a timed-out request, then reads the export and verifies that it was not completed.

## Failing Test

`tests/test_broken.py` expects a timed-out export to become `cancelled` with no result.

The broken implementation stores `completed` and `report.csv`, so the test fails.

## Diagnosis

The system fails because timeout handling is confused with cancellation.

Returning a timeout to the client does not automatically stop the work. Backend code must decide what should happen to the operation state when the request can no longer finish safely.

## Fixed Implementation

The fixed implementation marks the operation as cancelled when the timeout path is reached:

```python
if simulate_timeout:
    export.status = "cancelled"
    export.result = None
    raise ExportTimeoutError()
```

For real long-running work, the better design is often a job model: start the job, return a job ID, and let the client poll status. That makes timeout behavior explicit instead of pretending the request can safely finish everything inline.

## How to Run

From the repository root:

### Broken version

```bash
make broken CASE=BFL-0011
```

Expected: the test should fail because the broken implementation completes work after the timeout.

### Fixed version

```bash
make fixed CASE=BFL-0011
```

Expected: the tests should pass because the fixed implementation does not leave completed work behind after timeout.

## Files

- `broken/` — intentionally unsafe implementation
- `fixed/` — corrected implementation
- `tests/` — tests that demonstrate the failure and verify the fix

## Production Notes

- Do not assume a client timeout cancels backend work.
- Track operation state explicitly for long-running tasks.
- Prefer job/status APIs for work that can outlive the request.
- Make retries safe when the client cannot tell whether a timed-out operation finished.

## Trade-Offs

Cancelling work on timeout keeps state honest, but it may discard useful partial progress.

Letting work continue can be valid for background jobs, but then the API should expose a stable job ID and status instead of returning an ambiguous timeout from an inline request.
