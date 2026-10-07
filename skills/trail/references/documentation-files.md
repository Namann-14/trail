# Documentation files: full spec

Contents
- HANDOVER.md
- DECISIONS.md
- ARCHITECTURE.md
- CONSTRAINTS.md
- FLOW.md
- ROLLBACK.md
- TEST_CHECKLIST.md
- bugs/<slug>.md
- features/<slug>.md

Each entry: purpose, contents, when to update, why it matters.

## HANDOVER.md
**Purpose:** Where things stand right now. The first file any session reads.
**Contents:** Done / in progress or broken / what to avoid / next step. About 5 lines.
**Update:** At the end of every session, in place (overwrite the old state).
**Why:** Without it every session re-derives the project state, and the human re-explains it.

## DECISIONS.md
**Purpose:** The reasoning behind choices, not just the choices.
**Contents:** Dated entries: decision, options considered, why this one, which model/version decided, consequences.
**Update:** At the moment of the decision, not at the end of the session.
**Why:** Code shows what; only this file shows why. It stops future sessions from undoing deliberate choices or re-litigating settled ones.

## ARCHITECTURE.md
**Purpose:** The system map: components, responsibilities, how they connect.
**Contents:** Directory/module overview, key data structures, external dependencies, entry points.
**Update:** When structure changes, not for every code edit.
**Why:** Lets a fresh session find a root cause by reading one file instead of the whole repo.

## CONSTRAINTS.md
**Purpose:** What is off-limits or must be preserved.
**Contents:** Files/areas not to touch, APIs that must stay stable, libraries banned or required, style rules, security boundaries.
**Update:** When the human states a new boundary or a mistake reveals one.
**Why:** Guardrails the human can point to. The agent stops and asks before crossing one.

## FLOW.md
**Purpose:** How execution travels between files.
**Contents:** Request/command paths step by step, e.g. `cli.py -> parser.py -> store.py`, with the role each hop plays.
**Update:** Whenever an execution path changes.
**Why:** Bugs often live in the handoffs between files, which are the hardest thing to see from any single file.

## ROLLBACK.md
**Purpose:** The way back out of a risky change.
**Contents:** What changed, exact revert steps (commits, migrations, config), data implications, how to verify the revert worked.
**Update:** Written before a large or risky change is called done.
**Why:** Fear of irreversible changes makes people either refuse good changes or approve them blind.

## TEST_CHECKLIST.md
**Purpose:** Proof instead of claims.
**Contents:** Exact commands, expected outputs, manual checks, what was actually run and the result.
**Update:** Before calling work done.
**Why:** "The AI says it works" is not evidence. A command plus expected output is.

## bugs/<slug>.md
**Purpose:** One file per multi-step bug, start to finish.
**Contents:** Symptom, reproduction, hypotheses tried, root cause, fix, verification.
**Why:** Preserves the investigation, including dead ends, so the same bug isn't re-chased.

## features/<slug>.md
**Purpose:** One file per feature, start to finish.
**Contents:** Goal, plan, files touched, decisions (link to DECISIONS), tests, open questions.
**Why:** Keeps a multi-session feature coherent.

## Keeping files fresh
Docs drift. A session that edits code without updating the files it affects leaves the next session a wrong map. Staleness checks at session start: compare HANDOVER's date to `git log`, confirm paths named in ARCHITECTURE/FLOW still exist, and check that "in progress" items match the code. Fix only what you verified, and tell the user what was out of date.
