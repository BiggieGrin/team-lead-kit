---
name: requirement-check
description: Review a product requirement before planning — find gaps, ambiguities, missing acceptance criteria and cross-system dependencies, then draft questions back to Product. Use when the user pastes a requirement, gives a Jira Epic/Story key before planning, or asks "is this ready for planning".
argument-hint: <JIRA-KEY or pasted requirement>
---

# Requirement Check

Goal: catch the questions in the requirement before planning, not in the middle of the sprint.

## Step 1 — Get the requirement
Fetch the Jira issue (with child issues, comments, attachments and linked Confluence pages) or use the pasted text. Identify which of the systems in `CLAUDE.md` are affected.

## Step 2 — Check against this list
For each item: ✅ covered / ⚠️ partial or vague / ❌ missing / — not relevant.

**Purpose**
1. Problem and goal — why are we doing this, and for whom?
2. Which users/roles are affected, in which of the systems?
3. How will we know it succeeded?

**Behavior**
4. Acceptance criteria — present, and each one testable?
5. Main flow described end to end?
6. Edge cases and errors — empty data, invalid input, no permission, things running at the same time.
7. Screens/UX — is there a mockup or description? Hebrew/RTL considerations?

**Technical**
8. Permissions and roles — who may see/do what?
9. Data — new fields, migration of existing data, retention, sensitivity/classification.
10. Integrations — other systems (ours or external) that must change or be coordinated with.
11. Non-functional — performance, load, availability, logging/audit.

**Delivery**
12. Scope boundaries — what is explicitly NOT included?
13. Dependencies and blockers outside the team.
14. Deadline and why — real constraint or preference?
15. Rollout — release together with something else? feature flag? user training/communication?

Also look for: contradictions between parts of the requirement, vague words ("fast", "simple", "like before", "etc."), and hidden assumptions.

## Step 3 — Report
In Hebrew:

```
בדיקת דרישה — <KEY / title>
מוכנות לפלנינג: 🟢 מוכנה / 🟡 מוכנה עם שאלות פתוחות / 🔴 לא מוכנה
מערכות מושפעות: ...

## פערים עיקריים (לפי חומרה)
## שאלות לפרודקט   ← numbered, short, each answerable in one line
## הצעה לקריטריוני קבלה   ← draft acceptance criteria the user can propose
## סיכונים ותלויות
## הערכת גודל ראשונית   ← S/M/L/XL with one line of reasoning
```

## Step 4 — Optional actions (ask first)
- Post the questions as a comment on the Jira issue, tagging the Product owner.
- Add the drafted acceptance criteria to the issue description.
- Hand off to `/epic-breakdown` once the questions are answered.
