---
name: trail
description: Makes AI-assisted coding leave a paper trail by keeping written context files (handover, decision log, architecture map, constraints, rollback, test checklist) and following review habits while working. Use for any substantive multi-step coding work in a repo - starting a project, picking one up, fixing a bug, building a feature, or making an architectural change - even if the user never mentions documentation. Also trigger on requests for a handover file, decision log, architecture map, constraints or guardrails, rollback plan, or test checklist, and on phrases like "the AI keeps losing context", "I don't know why it did that", "pick up where we left off", or "I want more control over what it touches". Do NOT use for trivial edits such as typo fixes, renames, or one-line tweaks.
---

# Trail

Coding sessions normally end with no record of *why* anything changed, so the next session (and the human) starts from zero. This skill fixes that by having you keep short written context files and follow a few review habits as you work. The goal is that the human directs the work, rather than clicking "Allow" on changes they cannot explain.

Invoke explicitly with `/trail` in agents that support slash-invoked skills, or just say "use the trail skill". It also loads on its own when the task matches the description.

Based on the "Stop clicking Allow: 15 documentation habits" field guide.

## Where the files live

Default folder in the target repo: `ai-context/`. If the project already has a convention (`docs/`, `CLAUDE.md`, an existing decisions folder), use that instead of creating a parallel one, because two sources of truth rot quickly.

```
ai-context/
├── HANDOVER.md         # where things stand right now (updated in place)
├── DECISIONS.md        # why, not just what (+ which model/version decided)
├── ARCHITECTURE.md     # the system map
├── CONSTRAINTS.md      # what's off-limits
├── FLOW.md             # how execution travels between files
├── ROLLBACK.md         # the way back out, for risky changes
├── TEST_CHECKLIST.md   # proof, not claims: commands + expected outputs
├── bugs/<slug>.md      # one file per bug, start to finish
└── features/<slug>.md  # one file per feature, start to finish
```

Templates for each live in `assets/templates/`. Full spec per file (purpose, contents, when to update, why it matters) is in `references/documentation-files.md`. Read it when creating a file for the first time or unsure what belongs in one.

## Proportionality comes first

Scale the paperwork to the size of the decision. A typo fix, rename, or one-line tweak gets no ceremony and no new files, because overhead on trivial work teaches people to ignore the whole system. When in doubt, ask: "Would a future session be worse off without a note about this?" If no, skip it.

Rule of thumb for how much trail a change earns:

| Size of change | Trail |
|---|---|
| Typo, rename, one-line tweak with no real decision in it | Nothing. Just do it and show the diff. |
| Small fix in one file, root cause obvious (a missing check, an off-by-one) | Show the diff and the test result. Add a DECISIONS entry only if you chose between real options. Update HANDOVER if the project state changed. No bug or feature trace file. |
| Multi-step work: more than one hypothesis tried, more than one file touched for the same change, or a design choice a future session might question | Full loop: plan first, DECISIONS, a `bugs/` or `features/` trace, TEST_CHECKLIST, HANDOVER. |
| Large or hard-to-undo change (migrations, deletions, data format, public API) | Everything above plus a ROLLBACK note. |

Judge by the work, not the label. A "quick fix" that turns into three failed hypotheses has become multi-step, so start the trace file when that happens. Likewise, don't inflate a small fix to justify a file.

## Workflow

### 1. Start of work
- If `ai-context/` exists, read `HANDOVER.md` first, then `ARCHITECTURE.md` and `CONSTRAINTS.md`, then skim recent `DECISIONS.md`. This is what lets you pick up without the human re-explaining everything.
- Check whether the context is stale before trusting it. Warning signs: `HANDOVER.md` is dated well before the latest commits (`git log -3`), `ARCHITECTURE.md` or `FLOW.md` name files or functions that no longer exist, or the handover describes work the code doesn't show. When the docs and the code disagree, trust the code, tell the user which file is out of date, and fix that file (or the specific lines) before building on it. A wrong map is worse than no map because it sends you confidently in the wrong direction. Don't rewrite files wholesale; correct only what you verified.
- If it is missing and the task is substantial, offer a light scaffold (HANDOVER, DECISIONS, ARCHITECTURE) from the templates. Offer, don't block: if the user just wants to start, start, and write the files as you go.

### 2. Before coding
- State the plan and tradeoffs for anything non-trivial, so the human can redirect before code exists rather than after.
- If the request bundles several changes, split them and do one logical change at a time. Small changes are reviewable; big bundles get rubber-stamped.

### 3. While coding
- Respect `CONSTRAINTS.md`. If a change would cross a listed line, stop and ask first.
- Comment non-obvious logic to explain intent, not to restate the code.
- Log decisions to `DECISIONS.md` as they happen, not batched at the end, since end-of-session reconstruction loses the reasoning. Note which model/version made the decision.
- Update `FLOW.md` when execution paths change.
- Keep one trace file per multi-step bug (`bugs/`) or feature (`features/`), from start to finish.
- Keep generated files out of the change. Before staging or committing, check `git status` and leave out build output and caches (`__pycache__/`, `node_modules/`, `.pyc`, `dist/`, test databases, scratch files like `todos.json`). If the repo has no `.gitignore` covering them, mention it rather than quietly adding one. Stray artifacts bury the real diff and defeat the "show the real diff" habit.
- If you spot an unrelated bug, flag it to the user rather than silently fixing it. Scope creep is exactly what this skill exists to prevent.

### 4. Before calling it done
- Show the real diff, not just a summary (and confirm it contains only intended files). Summaries can hide things; diffs can't.
- Write concrete test commands with expected outputs in `TEST_CHECKLIST.md`. "It should work" is a claim; a command and its expected output is proof.
- For large or risky changes, write a rollback note in `ROLLBACK.md`.

### 5. End of session
Write a ~5-line handoff in `HANDOVER.md`: what's done, what's in progress or broken, and what the next session should avoid. Update in place rather than appending, so the file always describes *now*.

## Habits that don't involve files

These matter as much as the files; see `references/review-habits.md` for the reasoning.
- Don't let the human rubber-stamp an unread diff. Point out the parts that deserve a real look.
- "The AI says it works" is not proof. Run the tests and show the output.
- Check the human could explain the change in their own words. If not, explain it more simply before moving on.
- Version-pin decisions: record which model made them, so later surprises can be traced.

## Tone

Keep notes short and written for a reader arriving cold. Explain *why*, not just what. Never pad a file to look thorough.
