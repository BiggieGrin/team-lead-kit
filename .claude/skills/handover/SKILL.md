---
name: handover
description: Build a knowledge-handover document for a soldier who is leaving the team (release, transfer). Gathers their Jira tickets, commits and Confluence pages, interviews them to fill the gaps, and drafts a Confluence handover page. Use when the user says "handover", "חפיפה", or names a soldier who is leaving.
argument-hint: <soldier name or Jira username>
---

# Handover

Goal: when a soldier leaves, nothing they know should leave with them. Produce one Confluence page that a replacement could work from on day one.

## Step 1 — Identify
Look up the soldier in the Team table in `CLAUDE.md` (Jira username, Git username, main systems). If not found, ask the user.

## Step 2 — Gather evidence (read-only)
Collect, for roughly the last 12 months (ask the user if a different range is wanted):
- **Jira**: tickets assigned to or reported by them. Search with JQL like
  `assignee = <user> OR reporter = <user> ORDER BY updated DESC`.
  Group by system and by Epic. Note open tickets separately — they must be reassigned.
- **Git**: in each repo listed in `CLAUDE.md`, run
  `git log --author="<git user>" --since="12 months ago" --name-only --pretty=format:"%h %ad %s" --date=short`.
  Find the files/modules they touched most and modules where they are the *only* recent author (knowledge-risk areas).
- **Confluence**: pages they created or edited (design docs, runbooks).

Summarize the evidence to the user in a short list before continuing.

## Step 3 — Interview
Interview the soldier (or the user on their behalf). Ask **one question at a time**, in Hebrew, and build on the answers. Cover:
1. What do you own that nobody else knows well? (use the knowledge-risk list from Step 2 as prompts)
2. Recurring tasks you do (weekly/monthly) — how, where, with what access.
3. Things that break often and how you fix them.
4. Undocumented tricks, scripts, workarounds, hidden configuration.
5. Contacts outside the team you work with, and on what.
6. Accounts, permissions and access that must be transferred or revoked.
7. Work in progress: status, next step, gotchas — for each open ticket.
8. What you would tell your replacement in their first week.

Stop when answers stop adding new information. Ask the user before ending early.

## Step 4 — Draft the page
Use this structure:

```
# חפיפה — <name> — <date>
## תקציר
## אחריות ומערכות
## אזורי סיכון ידע (רק הוא מכיר)
## משימות שוטפות (איך, איפה, הרשאות)
## תקלות נפוצות ופתרונן
## עבודה פתוחה — לכל טיקט: סטטוס, צעד הבא, למי להעביר
## אנשי קשר
## הרשאות וגישות להעברה / ביטול
## קישורים (טיקטים, דפים, קוד)
## המלצות לממשיך
```

## Step 5 — Publish (with approval)
Show the full draft. Ask where to create it (default: team Confluence space, under a "Handovers" parent page). Create it only after approval.
Then offer, separately:
- a list of open tickets with a suggested new assignee each (reassign only after approval),
- Jira tasks for knowledge-risk areas ("cross-train X on module Y").
