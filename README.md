# x-cli-skill

Claude Code skill that drives [`x-cli`](https://github.com/nemoaigc/x-cli) —
the standalone X/Twitter command-line toolkit. This package is pure prose
+ references; all logic lives in `x-cli`.

## Install

1. Install `x-cli` first (so the `x-cli` binary is on PATH):

   ```bash
   pip install -e git+https://github.com/nemoaigc/x-cli@main
   x-cli auth status       # one-time auth verification
   ```

2. Install this skill into Claude Code:

   ```bash
   git clone https://github.com/nemoaigc/x-cli-skill ~/.claude/skills/x-cli
   ```

   (Or symlink from wherever you keep this repo.)

3. Restart Claude Code. The skill activates whenever the user mentions
   Twitter / X / tweets / timelines / etc. (see `SKILL.md` `description`).

## What's in here

| File                          | Purpose                                                                  |
|-------------------------------|---------------------------------------------------------------------------|
| `SKILL.md`                    | Skill manifest + mode-selection guide (read / trend / write)              |
| `references/LLMs.md`          | Quick command map + failure-modes table                                   |
| `references/read-mode.md`     | Single-call flag rubric for `x-cli search / user / tweet / feed / list`  |
| `references/search-plan.md`   | Multi-source research pipeline (follow-circle + keyword + trending + long-form) |
| `references/trend-mode.md`    | `x-cli trend scan` / `drill` workflow                                     |
| `references/write-mode.md`    | Compose / engage discipline (dry-run → confirm → `--yes`)                |
| `references/ranking-weights.md` | Per-intent weight + boost rubric for `x_cli.core.rank`                   |
| `references/spam-patterns.md` | Domain-bucketed regex catalog for `rank.filter_stack`                     |

## License

Apache-2.0. Derivative of [twitter-cli](https://github.com/public-clis/twitter-cli)
by @jackwener. See upstream NOTICE for attribution.
