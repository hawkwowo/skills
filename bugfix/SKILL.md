---
name: bugfix
description: Standard bug investigation and repair workflow. Use when the user asks to troubleshoot a reproducible failure, crash, incorrect output, or regression and expects root cause analysis, a fix diff, validation evidence, and regression coverage.
---

# Bugfix

## Overview

Use this skill for standard bug triage and repair work. It is designed for cases where the user can provide reproduction steps, failing commands, logs, traces, or a suspected impact area and expects a full diagnosis-to-fix workflow.

## Required Inputs

Ask for or extract these inputs first:

- Reproduction steps or the exact command that triggers the failure
- Error log, stack trace, crash output, or screenshots of the failure
- Suspected impact area, if the user already has a hypothesis

If any input is already present in the request, do not ask for it again.

## Required Outputs

You must deliver all of the following unless a technical blocker prevents it:

1. Minimal reproduction
2. Root cause analysis with code references and log evidence
3. Fix diff
4. Validation output showing the fix works
5. Regression test coverage, or a concrete explanation of why a regression test could not be added

## Workflow

### 1. Reproduce

- Try to reproduce the failure exactly as reported.
- Prefer the smallest command, input, or UI path that still triggers the issue.
- Record the exact command, environment assumptions, and observed output.
- If the issue is in a UI flow, reduce it to the smallest repeatable set of clicks or inputs.

### 2. Narrow the Scope

- Identify the failing layer: input parsing, state management, business logic, rendering, I/O, persistence, concurrency, environment, or integration boundary.
- Search for the nearest code path that can explain the observed symptom.
- Compare at least 3 nearby or similar patterns before changing code.
- Distinguish between the displayed symptom and the source of truth feeding it.

### 3. Prove the Root Cause

- Tie the failure to specific code lines and the relevant log or trace.
- Explain why the current behavior produces the observed incorrect result.
- If multiple causes are possible, rule them out one by one.
- Do not present a guess as the root cause. Show the chain from trigger to bad outcome.

### 4. Implement the Smallest Safe Fix

- Prefer the minimal patch that corrects the behavior without broad refactors.
- Preserve existing project conventions, naming, and control flow style.
- Avoid mixing opportunistic cleanup into the bugfix unless it is required for safety.
- If the fix changes behavior in a non-obvious way, note the tradeoff explicitly.

### 5. Validate

- Re-run the reproduction after the change.
- Run the most targeted automated checks available first, then broader checks if warranted.
- Capture the command or test output that demonstrates the fix.
- Check for nearby regressions in the same code path or data shape.

### 6. Add Regression Coverage

- Add or update the narrowest regression test that would have caught the bug.
- Prefer tests at the layer where the bug was introduced.
- If a regression test is not practical, explain the technical reason precisely:
  - no existing test harness at that layer
  - GUI-only path with no automation support
  - hardware or external dependency not controllable in this environment
  - failure depends on nondeterministic timing that cannot be stabilized yet

## Response Contract

When using this skill, structure the final result so it clearly contains:

1. Minimal reproduction
2. Root cause
3. Fix
4. Validation
5. Regression coverage

For root cause, include file references and line numbers whenever possible. For validation, include the concrete command or test that was run and the key passing output.

## Failure Strategy

### If You Cannot Reproduce

Do not guess. Return a focused "missing information" list that helps the user unblock reproduction. Include only the missing items that matter, such as:

- exact command or UI path
- input file or payload
- environment or build variant
- expected result vs actual result
- complete error log or stack trace
- whether the issue is deterministic or intermittent

### If the Failure Is Flaky

Systematically isolate variables one at a time. Check:

- dependency or tool version drift
- timezone and locale
- network availability or latency
- concurrency and race conditions
- random seeds
- test ordering
- cached state, temp files, or persisted data

When a flaky issue cannot be fully fixed in one pass, report what was ruled out, what still varies, and the next highest-value isolation step.

## Guardrails

- Do not skip reproduction unless the environment makes reproduction impossible.
- Do not claim a fix is complete without at least one post-fix validation step.
- Do not omit regression coverage discussion.
- Keep fixes scoped to the reported bug unless expansion is necessary for correctness.
