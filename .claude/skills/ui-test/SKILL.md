---
name: ui-test
description: Run smoke tests on a system's web UI in a real browser — follow the system's test plan, take screenshots, report pass/fail, and optionally open Jira bugs. Use after a release or deploy, or when the user asks to "test the UI", "smoke test", or "check system X works".
argument-hint: <system> [environment, default: test]
---

# UI Smoke Test

Goal: after every release, check that the main user flows of a system still work, without a person clicking through by hand.

Uses the `playwright` MCP server (browser automation). Test plans live in `ui-tests/<system>.md` in this project.

## Step 1 — Load the plan
- Find the system's test-environment URL in `CLAUDE.md`.
- Read `ui-tests/<system>.md`. If it does not exist, offer to create one: open the system, explore the main screens with the user, and write down 5–10 core flows in the format below.
- **Refuse to run against production.** If the URL looks like production, stop and ask.

## Step 2 — Log in
Use the test user from env vars `UITEST_<SYSTEM>_USER` / `UITEST_<SYSTEM>_PASS`. Never type real users' credentials. Never print passwords.

## Step 3 — Run each flow
For each flow in the plan:
1. Perform the steps in the browser.
2. Check the expected result (text visible, element present, navigation happened, no error message).
3. Take a screenshot at the key point and on any failure.
4. Also note: browser console errors, very slow pages (> ~5 seconds), broken layout or Hebrew/RTL display problems.

Do not create, change or delete real data unless the plan says the step is safe in that environment.

## Step 4 — Report
In Hebrew:

```
בדיקת UI — <system> — <env> — <date/time>
✅ עבר: X   ❌ נכשל: Y   ⚠️ חשוד: Z

| # | תרחיש | תוצאה | הערה |
```
For each failure: steps to reproduce, expected vs. actual, screenshot path, console errors.
Save the report and screenshots under `ui-tests/results/<system>-<date>/`.

## Step 5 — Bugs (ask first)
Offer to open a Jira bug for each failure in the system's project, with reproduction steps and screenshots attached. Show each bug before creating it.

---

## Test plan format (`ui-tests/<system>.md`)

```
# <system> — UI smoke tests

## 1. Login
Steps: open the home page, log in with the test user
Expected: dashboard loads, user name shown top-left

## 2. Search
Steps: type "test" in the main search, press Enter
Expected: results table shows at least one row
Safe to modify data: no
```
