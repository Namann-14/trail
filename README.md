# trail

Make AI-assisted coding leave a paper trail.

Coding sessions usually end with no record of *why* anything changed, so the next session (and the human) starts from zero. `trail` has your coding agent keep short written context files and follow a few review habits while it works, so you direct the work instead of clicking "Allow" on changes you can't explain.

## Install

```bash
npx skills add Namann-14/trail
```

Works with Claude Code, Cursor, Codex, GitHub Copilot, Windsurf, Gemini CLI, Cline, OpenCode and the other agents supported by the [`skills` CLI](https://skills.sh/docs/cli).

Useful flags:

```bash
npx skills add Namann-14/trail -g              # install globally (all projects)
npx skills add Namann-14/trail -a claude-code  # target one agent
npx skills update trail                                      # pull the latest version
```

## Use

- **Automatic:** describe real coding work (a feature, a bug fix, an architectural change) and the skill loads on its own.
- **Explicit:** type `/trail` in Claude Code, or say "use the trail skill".
- Trivial edits (typos, renames, one-liners) get no ceremony by design.

## What it creates

In the target repo, under `ai-context/` (or your existing docs convention):

```
ai-context/
├── HANDOVER.md         # where things stand right now
├── DECISIONS.md        # why, not just what
├── ARCHITECTURE.md     # the system map
├── CONSTRAINTS.md      # what's off-limits
├── FLOW.md             # how execution travels between files
├── ROLLBACK.md         # the way back out of risky changes
├── TEST_CHECKLIST.md   # proof, not claims
├── bugs/<slug>.md
└── features/<slug>.md
```

The skill scales the paperwork to the size of the change and checks existing context for staleness before trusting it.

## Repo layout

```
skills/trail/
├── SKILL.md
├── references/      # per-file spec + review habits
├── assets/templates/
└── evals/evals.json # three test prompts (feature, bug fix with context, trivial typo)
```

## Credits

Based on the "Stop clicking Allow: 15 documentation habits" field guide.

## License

MIT, see [LICENSE](LICENSE).
