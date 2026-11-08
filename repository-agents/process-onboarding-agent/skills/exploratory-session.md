# Skill: Exploratory Session

**Purpose:** Be the scribe for a human exploratory session. Record what the engineer tried and what they saw. Classify each defect. This skill does not explore the product, and it does not turn acceptance criteria into a script.

**Trigger:** On demand, when the engineer asks for an exploratory session or a QA session. Invoke directly with: "Read {path}/exploratory-session.md and record an exploratory session for <unit or intent>."

Never read this skill because a unit is Done, because an exploratory row is still `planned`, or because UAT is next. Never start it on a schedule. "Run the tests", "record test evidence", "write the test plan", and "run UAT" are not this skill.

**Classification:** ◆ Standalone. The engineer names the unit or intent. With or without the framework, write one dated note under `ops/operate/` and stop.

Use an answer the engineer has already given in this session. Do not ask a second time.

---

## What this skill changes

One new file: a dated session note directly under `ops/operate/`. Not under `retros/`, `incidents/`, or `improvements/`. Not a template, and not `_template.md`.

On the unit, the Evidence and Status cells of the exploratory row this session covers. Nothing else on that file.

It does not run a command, install a runner, read application code to look for defects, open `bug-bolt.md`, create a unit or a bolt, append an acceptance criterion, add a Test Plan row, mark a unit Done, start UAT, or change a `unit`, `integration`, `system`, or `acceptance` row.

---

## Step 1 — Take the four inputs

All four are required. If any is missing, ask once and wait:

> "To record this session I need four things:
> 1. Charter — what do you want to learn?
> 2. Timebox — how long is the session?
> 3. Areas in scope — where will you look?
> 4. Which unit file or intent file should I link the note from?"

Do not fill a gap from acceptance criteria, from a Test Plan row, or from the codebase.

Refuse the charter before doing anything else when its purpose is to execute, demo, check, or sign off acceptance criteria, ACs, or Given/When/Then. Say this, write nothing, and stop:

> "That is a UAT session. Read uat.md. This skill does not script acceptance criteria."

A charter that names a product question stays in scope even when the unit has acceptance criteria.

Record the timebox as the engineer stated it. Do not convert it into a command or a timer you run.

---

## Step 2 — Find the exploratory row

Read the named file far enough to find its title and, on a unit, the one `## Test Plan` table. You may read an intent's Extracted Units table far enough to find unit paths. Do not read `## Acceptance Criteria`. Do not quote acceptance criteria back.

A unit file must contain one `## Test Plan` table with columns `AC`, `Level`, `Evidence`, `Status`. If it has two, stop. Do not pick one. If it has none, ask whether to write the note with no link, and wait. Do not create a Test Plan.

An exploratory row is eligible when Level is exactly `exploratory`, Evidence is not `not this unit`, and Status is not `waived`.

- One eligible row: that row is the one this session covers, unless the engineer named a different AC number.
- Several eligible rows: list AC number, unit, and Evidence, ask which AC numbers this session covers, and wait. Link only those.
- Every exploratory row is `not this unit` or `waived`: do not change those cells. Ask which other unit this session belongs to, or whether to write the note with no link. Wait.
- The named file is an intent: collect eligible rows from the unit files in its Extracted Units table. If that table is missing, ask which unit files, and wait. Do not guess a path.

If the engineer says to write the note with no link, do that. Do not add a row.

---

## Step 3 — They explore. You write down

Confirm, then wait. Do not open the session with a step:

> "Charter: [charter]. Timebox: [timebox]. Areas: [areas]. I'll write the note under ops/operate/ and link it from [unit AC n | no exploratory row]. You run the session. Tell me what you tried and what you saw. I will not turn acceptance criteria into steps."

If the invoking message already reports what they tried, what they saw, and that the session is done, do not wait for a second telling. Still do not add anything they did not report.

While they are in the session:

- Record only what they tried and what they saw. One observation, one bullet. Use their words. Do not tighten, translate, or rephrase. Do not split a report into acceptance-shaped steps.
- If they have not said what they saw, ask what they saw, and wait. Do not invent an observation.
- If they ask what to try, repeat the charter and the areas. Do not add a probe, a path through the product, or a step from an acceptance criterion.
- If they ask you to walk, run, check, or sign off acceptance criteria, refuse that request in one sentence, point at `uat.md`, and stay on the charter. Do not list the criteria.
- Do not explore the product. Do not read application code to look for defects. Do not run a test command.
- When they say the session is done, or that the timebox has ended, stop collecting. If that same message already includes what they tried, do not ask for another observation. Otherwise ask once: "Any last observation inside the areas in scope, or shall I write the note?" A last observation they then give is part of the note. Do not suggest another area.

An observation they mark as after the timebox is included and prefixed `After timebox:`.

---

## Step 4 — Classify every defect

A defect is something they reported as wrong, surprising, or broken. An observation they do not call a defect is not one. Do not promote an observation to a defect yourself.

Zero defects is allowed.

Ask one unclassified defect at a time, then wait:

> "Defect [n]: [what they saw]. Classify it as bug-bolt, new AC, or dropped-with-reason."

Accept `bug bolt` or `bug` as `bug-bolt`. Accept `new acceptance criterion` or `add an AC` as `new AC`. Accept `drop` as `dropped-with-reason` only when a reason is in the same answer. Anything else: ask which of the three, and wait. Do not guess.

Then, for that classification:

- **bug-bolt.** The note must hold four intake facts: symptom, where, how it is reproduced, expected behavior. Ask once, in one message, only for the facts they have not already stated. Wait. Do not open `bug-bolt.md`. Do not create a unit, a bolt, or a backlog row.
- **new AC.** Ask them to state the criterion in their words, unless they already have. Record that text unchanged. Do not rewrite it as Given/When/Then. Do not append it to a unit. Do not add a Test Plan row.
- **dropped-with-reason.** A reason they state is required. Empty, "no", "later", "not important", or a role name alone is not a reason. Ask what was learned and why it will not be tracked, and wait.

Do not write the note while any defect is unclassified, while a bug-bolt fact is missing, while a new AC has no wording, or while a drop has no reason.

---

## Step 5 — Write the note and link the row

Write one file. Create `ops/operate/` only if it is absent. Do not overwrite an existing file. If the filename already exists, stop and say so.

Choose the directory in this order:

1. `{FRAMEWORK_ROOT}/ops/operate/` when the framework root is known. The framework root is the directory that contains `rules/prompt-quality-gate.md` and `ops/operate/`.
2. Otherwise `ops/operate/` under the current working directory.

Never write under `process-onboarding-agent/`. That tree is the bootstrap agent.

Filename: `YYYY-MM-DD-<unix_timestamp>-<slug>-exploratory.md`. The date is the day of writing. The timestamp is seconds since epoch at writing. The slug comes from the charter, lowercased. Remove words of three letters or fewer. Also remove these words wherever they appear: what, when, does, happens, whether, with, from, that, this, your, into, have, been. Keep the next five words that remain, in order. Join them with hyphens. Remove every character that is not a letter, number, or hyphen. If nothing remains, the slug is `session`.

```markdown
# Exploratory session

**Date:** YYYY-MM-DD
**Timebox:** [as stated]
**Charter:** [what to learn]
**Areas in scope:** [as stated]
**Unit or intent:** [path the engineer named]

## What was tried

- [what the engineer tried] — [what they saw]

## Defects

| # | What happened | Classification | Record |
|---|---|---|---|
| 1 | [what they saw] | bug-bolt | Symptom: … Where: … Reproduce: … Expected: … bug-bolt.md not opened. |
| 2 | [what they saw] | new AC | [their words]. Not added to the unit. Not added to the Test Plan. |
| 3 | [what they saw] | dropped-with-reason | [reason] |
```

The block above is the shape. Write one defect row per defect. Do not copy a sample row the session did not have. Do not put a `|` character inside a cell.

When there is no observation, write `- None reported.` When there is no defect, write `Defects: none.` under `## Defects` and do not invent a table row.

After the file exists, link each chosen exploratory row. Set Evidence to a markdown link whose text is the filename and whose target is the path from the unit file's directory to the note. Set Status to `passed`.

`passed` on an exploratory row means the note is linked and every defect in it is classified. It does not mean the product had no defects.

Do not change any other cell. Do not change the Waiver line. Do not change the unit's `**Status:**` line.

If Evidence is already a link to a session note and the engineer has not said whether this session replaces it or is additional, ask which, and wait. Do not guess. Replace that link only when they said replace. When they said this is an additional session, put both links in the cell, separated by `, `. Do not delete the older file.

If the unit file and the note do not share an `ops/` directory, set Evidence to the plain text `ops/operate/<filename>` instead of a relative link that leaves the project. Still set `passed`.

Then stop. Do not start UAT, bug-bolt, test-evidence, or code generation. Do not mark the unit Done.

Report:

```
Exploratory session recorded.

Note:      [path]
Charter:   [one line]
Tried:     [N] observations
Defects:   [N]  bug-bolt [n], new AC [n], dropped [n]
Linked:    [unit] AC [n] | no exploratory row

Exploratory passed means the note is linked. It does not mean there were no defects.
This was a human session. No acceptance script was run.
```
