# Skill: Test Evidence

**Purpose:** Run the command already stored on each `unit`, `integration`, and `system` row of a unit's Test Plan, once, and record the exit code and a result path. Set that row to `passed` or `gap`. This skill records evidence. It does not choose a level, invent a command, or start UAT.

**Trigger:** Before marking a unit Done. Invoke directly with: "Read {path}/test-evidence.md and record test evidence for <unit>."

**Gate:** On `unit`, `integration`, and `system` rows, a `gap` or empty status blocks Done. A `waived` row does not. A row still `planned` blocks Done as well: no evidence has been recorded. Do not mark the unit Done while any of those rows is `gap`, empty, or `planned`.

**Classification:** ◆ Standalone. The engineer names the unit file that already has a Test Plan. With or without the framework, update that file and stop.

---

## What this skill changes

It updates Evidence and Status on `unit`, `integration`, and `system` rows, the Waiver line under that table, a Note line for a system row that was not run, and one result file per command it ran.

It does not change `acceptance` rows, `exploratory` rows, the unit's `**Status:**` line, Definition of Done checkboxes, or any other existing file. It does not open `uat.md`, write a UAT sign-off, generate code, or install a test runner.

---

## Step 1 — Read the plan

Read the unit file the engineer named. If they did not name a file, ask and wait:

> "Which unit file should I record test evidence for?"

The file must contain one `## Test Plan` table with columns `AC`, `Level`, `Evidence`, `Status`. If it has none, stop. Tell the engineer to run `test-plan.md` first. Do not invent rows. Do not generate code. Do not start UAT. If the file has two `## Test Plan` sections, stop. Do not pick one.

Compare Level exactly, case-sensitive, to `unit`, `integration`, `system`, `acceptance`, or `exploratory`. If a row uses any other level, stop. Do not rename it and do not map it onto one of the five.

State this, then continue. Do not wait for permission to run a stored command:

> "I've read unit [name]. The Test Plan has [N] rows. I will run stored commands for unit and integration rows that have a command. System rows wait on whether that environment is up. I will not run acceptance or exploratory rows. A gap or empty status blocks Done. A waived row does not."

A command is the executable string already in that row's Evidence cell. These are not a command:

- an empty cell
- `no command`
- `uat.md`
- `not this unit`
- a coverage threshold, a percentage, a tool name, a language, or a directory of tests
- prose, including "integration tests are required" or "system tests run in staging"

Do not read `rules/code-standards.md` to replace the stored string. Do not invent a command. Do not install a runner. Do not add flags, a wrapper, or a different binary. Do not reuse another row's command.

---

## Step 2 — System rows and the environment

If any `system` row has a command, and the engineer has not already said whether that environment is up, ask once and wait:

> "Is the test or staging environment for the system-test command up? I will not run system rows until you answer."

Use an answer the engineer has already given in this session. Do not ask a second time.

- **Up.** Run each of those stored commands in Step 3.
- **Not up, or the engineer does not know.** Do not run them. Set each of those rows to `gap`. Leave the stored command unchanged in Evidence. Under the table, write `Note: AC [n] — environment not up. Not run.` Do not write a result file. Do not record an exit code. Do not record a pass. Do not append that note to the command string.
- **The engineer says this command does not use a deployed environment.** Run the stored command as stored. Do not substitute a different command.

A later invocation asks this question again. The stored command is still the Evidence cell when no result file has been written. Do not treat `Note: AC [n] — environment not up. Not run.` as a command.

A `system` row with no command skips this question. Step 3 sets it to `gap`.

---

## Step 3 — Run each remaining row once

Apply this to each `unit` row, each `integration` row, and each `system` row Step 2 told you to run. Leave every `acceptance` row and every `exploratory` row unchanged, including a status of `planned`.

Skip a row the engineer has already waived in this session when both a person's name and a reason are present for that AC. Step 4 records it. Do not run it.

Skip a row that is already `waived` when the Waiver line names a person and a reason for that AC.

Skip a row that is already `passed` when both are true: Evidence is `exit 0 -> <path>`, and that file exists and its text records `exit: 0`. Unless the engineer says "re-run", do not run it again.

A row that says `passed` without that file is not a pass. Set Status to `gap` and Evidence to `no command`. Do not invent a command to replace the missing file.

When Evidence is `exit <code> -> <path>` and that file exists, the stored command is the file's `command:` line. Do not execute the Evidence cell as a shell string.

For every other eligible row:

1. If the row has no command, set Status to `gap` and set Evidence to `no command`. Do not ask for a command. Do not mark the unit Done.
2. Otherwise run the stored string once, in a shell, from the current working directory. Do not change directory. Do not change the string. Do not background the process. A command that has not exited is not a pass.
3. Write a result file beside the unit, at `test-evidence/<unit-basename>-ac-<n>-<level>.log`. `<unit-basename>` is the unit file name without `.md`. `<n>` is the AC number. Create the `test-evidence` directory beside the unit file when it is absent. The file's first lines are exactly `command: <string>`, `exit: <code>`, and `date: YYYY-MM-DD`, then stdout, then stderr. Write this file for exit code 0 and for every other exit code, including a command that cannot be started.
4. Set Evidence to `exit <code> -> <path>`. `<path>` is the result file relative to the current working directory. The command itself stays in the result file, not in the table cell.
5. Exit code `0` sets Status to `passed`. Any other exit code sets Status to `gap`. Remove a `Note: AC [n] — environment not up. Not run.` line for an AC that is now `passed` or `waived`.

Run each eligible row once in this invocation. Do not re-run it in the same invocation.

---

## Step 4 — Waiver

Do not ask the engineer to waive a gap.

Set a `unit`, `integration`, or `system` row to `waived` only when the engineer waives that AC and gives both:

- a person's name (a role alone, such as "QA", is not a name)
- a reason

If either is missing, do not set `waived`. Write the Step 3 results first (Step 5), then ask for the missing part only, and wait. Leave the row at the status Step 3 gave it.

A reply that only supplies the missing name or reason continues this session. Update that row and the Waiver line. Do not re-run a command already run in this session.

Replace the Waiver line under the table. One line per waived AC:

```markdown
Waiver: AC [n] — [person], [reason].
```

When no row is waived, the line is:

```markdown
Waiver: none. Empty status is not a waiver.
```

An empty status is not a waiver. Do not invent a person, a reason, or an exit code. A waived row does not need a command run. Do not waive an `acceptance` row or an `exploratory` row.

---

## Step 5 — Write the table and stop

Update the existing `## Test Plan` in place. Do not add a second section. Do not add or remove rows. Do not change the unit's `**Status:**` line. Do not tick Definition of Done.

Then stop. Do not start UAT. If Step 4 is waiting on a name or a reason, ask for that missing part after the report.

Report:

```
Test evidence recorded.

Unit:      [name]
Passed:    [N]  [AC numbers]
Gap:       [N]  [AC numbers and why: no command | environment not up | exit <code>]
Waived:    [N]  [AC numbers]
Unchanged: acceptance [AC numbers], exploratory [AC numbers]

Done: [blocked | not blocked by test evidence]
```

Write `Done: blocked` while any `unit`, `integration`, or `system` row is `gap`, empty, or `planned`.

Write `Done: not blocked by test evidence` only when every such row is `passed` or `waived`. This skill still does not set Status to Done.
