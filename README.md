# Deprecated: x-cli-skill

This repository has been folded into [`nemoaigc/x-cli`](https://github.com/nemoaigc/x-cli).

The current supported project is one repo:

```text
x-cli/
  src/x_cli/              # CLI engine
  skills/x-research/      # Agent skill and /x commands
```

Install the CLI and bundled skill:

```bash
uv tool install --force git+https://github.com/nemoaigc/x-cli.git@main
x-cli skill install
```

After restarting Claude/Codex-style clients, use:

```text
/x 去 X 调研下今天的 agent 新闻，有啥论文也拿过来
/x-research post-training 新论文 this month
```

New workflow changes should go to `x-cli/skills/x-research`, not this repo.
