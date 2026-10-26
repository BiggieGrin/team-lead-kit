---
name: teammate
description: Work a small Jira ticket end-to-end like a team member — read the ticket, check it is suitable, fix it on its own branch in an isolated worktree, test, and open a DRAFT merge request for a human to review. Use when the user says "take ticket X", "/teammate X", or asks Claude to handle a small bug or tech-debt task.
argument-hint: <JIRA-KEY>
disable-model-invocation: true
---

# Claude as a Team Member

Goal: take small, well-defined tickets off the team's plate. A human always reviews and merges.

## Step 1 — Read and qualify the ticket
Fetch the ticket, its comments, linked tickets and attachments. Find the system and repo using `CLAUDE.md`.

The ticket is **suitable** only if all are true:
- The expected behavior is clear (or the bug is reproducible).
- The change is likely contained (rough guide: under ~10 files, no schema/data migration, no auth/permission or security changes, no public API contract changes).
- There is a way to verify it (existing tests, a new test, or a clear manual check).

If not suitable: stop and explain why, and suggest what is missing (questions for the reporter, or how to split the ticket). Do not start coding.

## Step 2 — Plan
Write a short plan in Hebrew: root cause or approach, files to change, how it will be tested. Show it and wait for approval.

## Step 3 — Isolated workspace
Work in a separate git worktree on a new branch named per `CLAUDE.md` conventions (default `<JIRA-KEY>-short-description`), based on the latest target branch. Never work on the user's current checkout.

## Step 4 — Implement
- For bugs: first write a test that reproduces the bug and fails, then fix it, then confirm the test passes.
- Follow the repo's own `CLAUDE.md` (build/test commands, style). Match the surrounding code.
- Keep the change minimal — no unrelated refactors or formatting changes.
- If you discover the ticket is bigger or different than it looked: stop and report instead of expanding scope.

## Step 5 — Verify
Run the repo's build, linter and full relevant test suite. Report the actual results. If something fails and you cannot fix it within scope, stop and report.

## Step 6 — Hand off (ask before each write)
1. Commit with message `<JIRA-KEY>: <summary>`.
2. Push the branch (never to a protected branch, never force-push).
3. Open a **draft** merge request. Description in Hebrew: what was wrong, what changed, how it was tested, what the reviewer should check carefully.
4. Comment on the Jira ticket with the MR link and move it to the status for "in review" (TODO: set the status name).

Finish with a summary: MR link, tests run, risks, suggested reviewer (from the soldiers who know that system, per `CLAUDE.md`).
