---
name: x-cli-skill
description: >
  Deprecated compatibility stub for the old standalone x-cli skill repository.
  Use only when the user explicitly asks about x-cli-skill migration or legacy
  installation. For X/Twitter research, tweets, trends, timelines, or posting,
  use the bundled x-research skill from nemoaigc/x-cli instead.
---

# Deprecated: x-cli-skill

This standalone skill repository is deprecated. The supported layout is now one
project:

```text
nemoaigc/x-cli
  src/x_cli/              # CLI engine
  skills/x-research/      # Agent research/action skill
```

Install the current CLI and bundled skill:

```bash
uv tool install --force git+https://github.com/nemoaigc/x-cli.git@main
x-cli skill install
```

Then restart the agent client and use:

```text
/x 去 X 调研下今天的 agent 新闻，有啥论文也拿过来
/x-research post-training 新论文 this month
```

Do not maintain new workflow logic in this repository. Add it under
`x-cli/skills/x-research` instead.
