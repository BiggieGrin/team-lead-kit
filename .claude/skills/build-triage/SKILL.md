---
name: build-triage
description: Diagnose a failed CI build/pipeline. Fetches the log, finds the real error, classifies the failure, identifies the likely culprit commit and owner, and suggests a fix. Use when the user pastes a failed build link, job ID or log, or asks "why did the build fail".
argument-hint: <pipeline/job URL, job ID, or "latest <system>">
---

# Build Triage

Goal: turn a 5,000-line log into one line: **what broke, why, who should fix it.**

## Step 1 — Get the log
- If the user pasted a log, use it.
- If given a URL/ID, fetch the console log. Use the CI from `CLAUDE.md` and the token in env var `CI_TOKEN`:
  - Jenkins: `curl -s -u "$CI_USER:$CI_TOKEN" "<job-url>/consoleText"`
  - GitLab CI: `glab ci trace <job-id>` or the jobs API `/api/v4/projects/<id>/jobs/<job-id>/trace`
- If given "latest <system>", find that system's CI job in `CLAUDE.md` and fetch the last failed run.
- For huge logs, search for error markers first (`ERROR`, `FAILED`, `Exception`, `error:`, `exit code`) instead of reading everything.

## Step 2 — Find the first real error
The first failure is usually the cause; later errors are often consequences. Ignore warnings unless warnings are treated as errors.

## Step 3 — Classify
| Category | Typical signs |
|---|---|
| Compile/build error | syntax, type, missing symbol |
| Test failure | assertion failed, specific test name |
| Flaky test | test passed on a recent run with the same code; timing/order dependent |
| Dependency | package not found, version conflict, mirror/registry unreachable |
| Infrastructure | agent offline, disk full, timeout, out of memory, network |
| Config/secrets | missing env var, expired credential |

To spot flakiness, check whether the same test failed/passed in the last few runs.

## Step 4 — Find the culprit
For code failures: identify the commits since the last green build (`git log <last-green-sha>..<failed-sha>`), and use `git blame` on the failing lines/tests. Name the most likely commit and its author.
For infra/dependency failures: no culprit commit — say who handles infra (from `CLAUDE.md`, or ask).

## Step 5 — Report
Answer in Hebrew in this format:

```
🔴 <system> — build #<n>
סיבה: <one line>
קטגוריה: <category>
קומיט חשוד: <sha> — <author> — <message>
מי מטפל: <name>
תיקון מוצע: <concrete suggestion, with file:line if known>
```
Then a short "details" section with the relevant log excerpt (max ~20 lines).

## Step 6 — Optional actions (ask first, one at a time)
- Rerun the job (only for flaky/infra).
- Post the report as a comment on the related Jira ticket or merge request.
- Open a Jira bug for a flaky test.
Never modify pipeline configuration.
