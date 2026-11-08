# Skill: Oversight Review

**Purpose:** Record whether a named reviewer who did not author the change agrees with the test plan, or which row ids they reject. Rejection sets those rows back to `gap`. This skill keys off the intent's AI Risk field. It does not set the intent status, it does not replace UAT, and it does not authorize a release.

**Trigger:** Before setting an intent's status to Implemented, and only when AI Risk is Limited or High, after a UAT status exists. Minimal risk skips it. Invoke directly with: "Read {path}/oversight-review.md and record the oversight review for <intent>."

**Classification:** ◇ Framework-only. The AI Risk field on the intent is required. A master rule file is not required to record the review. If the engineer has not named an intent file, ask once which intent file to review, and wait. Do not write a standalone note. Do not invent an intent.

This skill may cite a verification record; the release-readiness checklist must not start inspecting the repo.

Use an answer the engineer has already given in this session. Do not ask a second time.

---

## What this skill changes

On Minimal, one section on the intent: `## Oversight Review`, with result `not required`. No unit file is opened.

On Limited or High, that same section, with result `agree` or `disagree`. On `disagree` only, the Status cell of each rejected test-plan row becomes `gap`.

It does not change the intent's opening `**Status:**` line. It does not change a row's AC, Level, or Evidence. It does not change the Waiver line, a unit's `**Status:**` line, or Definition of Done. It does not add or remove a Test Plan row. It does not start UAT, test-evidence, exploratory-session, or `release-readiness-checklist.md`. It does not open `product-engineering-essentials.md` and it does not copy a compliance checklist or a control matrix. It does not give legal advice and it does not embed control text. It does not run a test command and it does not name a test runner.

Never write under `process-onboarding-agent/`.

---

## Step 1 — Read AI Risk

Read the intent file the engineer named. Read the opening `**AI Risk:**` line and the opening `**Status:**` line. Remember the status value. You will write it back unchanged.

The AI Risk value is the text after `**AI Risk:**`, trimmed. It must be exactly one of `Minimal`, `Limited`, or `High`. Ignore the definition blockquote under that line. The template text `Minimal | Limited | High` is not a value. If the value is missing, blank, or anything else, stop. Ask which of the three it is, and wait. Do not guess. Do not write a section.

### Minimal

Write the section in Step 7 with result `not required`. Do not open a unit file. Do not ask for a reviewer. Do not read a test plan. Do not ask about a regulation. Then stop.

The report is:

```
Oversight review recorded.

Intent:    [name]
AI Risk:   Minimal
Result:    not required

Intent status was not changed.
```

### Limited or High

Continue at Step 2. Do not write the section until Step 7.

---

## Step 2 — A UAT status must already exist

Read `## UAT Sign-off`. A UAT status exists only when that section has a `**Status:**` line whose value is exactly one of:

- `Passed`
- `Passed with findings`
- `Failed`
- `Failed — not ready to close`
- `Deferred`
- `Not required`

`Pending`, an empty value, a line that contains `|`, or a missing section means no UAT status exists.

If none exists, stop. Say this, and write nothing:

> "A UAT status has to exist before oversight. I will not start UAT and I will not set the intent status."

Do not open `uat.md`. Do not edit the UAT section. A status of Failed, Deferred, or Not required is still a status. Do not skip the review because of it, and do not treat it as permission to set Implemented.

---

## Step 3 — Read the test plans

From `## Extracted Units`, open every linked unit file. Resolve a relative link from the directory that contains the intent file. Each file must contain one `## Test Plan` table with columns `AC`, `Level`, `Evidence`, `Status`. If a linked unit has no such table, stop. If it has two, stop. Do not pick one. Do not invent rows. Do not append `## Test Plan`.

A row id is the value in the AC column. Say it as `AC [n]`. When the intent has more than one unit, say `AC [n]` on `[unit path]`.

Show every row to the reviewer: row id, level, evidence, status. Do not change a cell in this step. Do not open an evidence path. A path, an exit line, or a link in the Evidence cell may be cited later. Citing it does not mean reading it.

State this, then continue:

> "AI Risk is [Limited or High]. I will not set the intent status. Name a reviewer who did not author this change. They either agree with the test plan, or list the row ids they reject."

---

## Step 4 — Named reviewer who is not the author

The author is the opening `**Owner:**` value, trimmed. If the engineer has named a different author in this session, that name is the author instead.

A reviewer is one person's name. A role alone is not a name. QA, security, product, technical owner, operations, engineer, reviewer, and "the team" are roles. If the only answer is a role, ask for a person's name, and wait. Do not appoint someone.

Compare the reviewer and the author after trimming and ignoring case. The same text is the same person. If they match, write nothing and do not change a row. Say:

> "[name] authored this change. Name a reviewer who did not."

Then wait. Do not record agree or disagree for the author.

The engineer running this session is not the reviewer unless they name a person who is not the author. If they name themselves and that name is the author, refuse as above.

---

## Step 5 — If the intent names a regulation

Search the intent body outside the AI Risk definition blockquote. A regulation is named when the intent says the work must meet, comply with, or is subject to a named statute, regulation, or compliance regime. Copy that name from the intent. Do not add a name the intent did not use. Do not keep a list of regimes. Do not quote control objectives, requirement numbers, or control text. Do not decide whether the plan satisfies it. Do not give legal advice.

If none is named, skip this step. Do not ask.

If one or more are named, ask once, then wait, unless the reviewer has already stated the row ids:

> "The intent names [name as written]. Which test-plan row ids are the evidence for it? State the row ids, or say that none of these rows are the evidence. I will record what you say. I will not advise."

Record the reviewer's words as the regulation evidence. Do not add a row they did not name. Do not drop one they did name. Do not judge it. This statement does not by itself change a Status cell. Do not write Step 7 until they have answered.

---

## Step 6 — Agree or list rejections

If they have not already answered, ask once, then wait:

> "Do you agree with the test plan, or which row ids do you reject?"

**Agree.** They agree with the whole plan and name no rejected row. Do not edit any unit file. Result is `agree`.

**Reject.** They list one or more row ids. That is `disagree`, including when they also agree with the other rows. Set the Status cell of each named row to `gap`. Edit that cell only. Leave AC, Level, and Evidence as they are. Leave every unnamed row as it is. Leave the Waiver line. Leave the unit's `**Status:**` line. A row that is already `gap` stays `gap`.

Match a row id to the AC column. `AC 1`, `1`, and `row 1` are the same row when only one unit has that AC number. When more than one unit has that AC number and they did not say which unit, ask which unit, and wait. Do not set both.

A row id that is not in the tables you read: change nothing. Ask them to name a row you showed, and wait.

An empty list is not a rejection and not an agreement. Ask which of the two they mean, and wait.

---

## Step 7 — Write the section and stop

Write one `## Oversight Review` section on the intent. If that heading already exists, replace from that heading through the line before the next `## ` heading. Do not add a second copy.

On Minimal, place it immediately after `## UAT Sign-off` when that heading exists. Otherwise place it immediately before `## Implementation Summary` when that heading exists. Otherwise append it at the end of the file.

On Limited or High, place it immediately after the whole `## UAT Sign-off` section, before the next `## ` heading. Subheadings inside UAT stay inside UAT.

Change no other line of the intent. The opening `**Status:**` line stays the value you read in Step 1. Do not set Implemented.

Minimal:

```markdown
## Oversight Review

**Result:** not required
**AI Risk:** Minimal
**Date:** YYYY-MM-DD
```

The date is the day you write the section.

Limited or High:

```markdown
## Oversight Review

**Result:** agree
**AI Risk:** [Limited or High]
**Reviewer:** [name]
**Author:** [name]
**Date:** YYYY-MM-DD
**Rejected rows:** none
**Regulation evidence:** [the reviewer's words, or "none named"]
**Cites:** [the verification record they cited, or none]
```

Use `disagree` when they rejected rows. `**Rejected rows:**` then lists each rejected row id, comma-separated. Use the AI Risk value you read, not both words.

`**Cites:**` is a verification record the reviewer chose to name: a result path, an exit line, or another evidence path already on a row. Copy their words. If they cited nothing, write `none`. Do not open the path. Do not search the repo to check it. Do not start `release-readiness-checklist.md`.

Then stop. Do not set the intent status. Do not mark a unit Done. Do not start another skill.

Report:

```
Oversight review recorded.

Intent:    [name]
AI Risk:   [Limited or High]
Reviewer:  [name]
Result:    [agree or disagree]
Rows set to gap: [row ids, or none]
Regulation evidence: [one line]

Intent status was not changed.
```
