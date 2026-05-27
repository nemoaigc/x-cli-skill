# X skill

Claude/Codex skill for researching and acting on X/Twitter. It plans the
research workflow, chooses sources and time windows, then drives
[`x-cli`](https://github.com/nemoaigc/x-cli), the standalone X/Twitter command
line toolkit.

## Install

1. Install `x-cli` first (so the `x-cli` binary is on PATH):

   ```bash
   pip install -e git+https://github.com/nemoaigc/x-cli@main
   x-cli auth status       # one-time auth verification
   ```

2. Install this skill into Claude Code:

   ```bash
   git clone https://github.com/nemoaigc/x-cli-skill ~/.claude/skills/x-cli
   ln -sf ~/.claude/skills/x-cli/commands/x.md ~/.claude/commands/x.md
   ln -sf ~/.claude/skills/x-cli/commands/x-research.md ~/.claude/commands/x-research.md
   ln -sf ~/.claude/skills/x-cli/commands/x-cli.md ~/.claude/commands/x-cli.md
   ```

   (Or symlink from wherever you keep this repo.)

3. Restart Claude Code. The skill activates whenever the user mentions
   Twitter / X / tweets / timelines / X research / X news / X papers / etc.
   (see `SKILL.md` `description`).

## Use

Natural language should trigger the skill:

```text
去 X 调研下今天的 agent 新闻，有啥论文也拿过来
```

Slash commands are also provided:

```text
/x 去 X 调研下今天的 agent 新闻，有啥论文也拿过来
/x 看 @karpathy 这周发了什么
/x-research post-training 新论文 this month
/x-cli trend scan
```

`/x` is the primary entry. `/x-cli` is kept as a compatibility alias.

## What's in here

| File                          | Purpose                                                                  |
|-------------------------------|---------------------------------------------------------------------------|
| `SKILL.md`                    | Skill manifest + X research/action workflow                               |
| `commands/x.md`               | Primary slash command wrapper                                             |
| `commands/x-research.md`      | Research-only slash command wrapper                                       |
| `commands/x-cli.md`           | Compatibility alias for `/x`                                              |
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
