---
name: x-cli
description: >
  X/Twitter toolkit for reading tweets, timelines, articles, scanning trends, and writing
  (post/reply/quote/follow/like/block/delete). Load this skill whenever the user mentions
  Twitter, X, tweets, timelines, feeds, following, followers, trending topics, or any
  X-related account — even casually, like "看看X有啥", "scan my timeline", "draft a reply",
  "find someone on Twitter", "what's trending", "帮我发推", or "check what @someone posted".
  Also trigger when the user wants to engage with social media content (like, retweet, bookmark),
  research people/topics via their X presence, or draft social media posts in X's style.
  Three modes: read (search/fetch), trend (discover), write (post/engage). If there's even
  a slight chance the task touches X/Twitter, load this skill — it's cheap to load and
  expensive to skip.
---

# x-cli skill

Companion guide for the standalone [`x-cli`](https://github.com/nemoaigc/x-cli)
binary. The CLI handles every X/Twitter operation; this skill tells you
*when* to use which subcommand, how to compose ranking/filtering, and the
discipline for write actions.

## Pick a mode

**Read mode** — user wants to search / scan / analyze X content.

  Which references to load depends on the task:

  - **Simple lookup** (single user timeline, single tweet/article, follower list):
    read `references/read-mode.md` — flag rubric for a single `x-cli` call.
  - **Research or multi-source scan** (briefing, topic scan, "what's happening with X"):
    read `references/search-plan.md` — source selection, probe-first parameter derivation,
    `--product`/time-window decision. Load `references/read-mode.md` only for edge-case flags.
  - **Ranking / filtering**: after fetching, read `references/spam-patterns.md`
    (domain-bucketed regex catalog) and `references/ranking-weights.md`
    (per-intent weight rubric) to compose `rank.filter_stack` + `rank.author_diversity`
    parameters. No defaults — derive from domain + intent.

  Anti-pattern: single keyword query → 30 raw results. Loses the follow-circle signal,
  long-form articles, and trending context.

**Trend mode** — user is discovering: "what's hot right now on X".
  → Read `references/trend-mode.md`.

**Write mode** — user wants to post / reply / quote / delete tweets, or follow / unfollow accounts.
  → Read `references/write-mode.md`. **Always dry-run first; show the user the exact
  text/target; only `--yes` after explicit human OK.**

Engagement writes (`x-cli engage like/retweet/bookmark`) execute immediately — they're
low-risk reactions, no `--yes` gate.

Ambiguous case: treat as read mode; note in the report that trend/write modes are available.

## Prerequisites

- `x-cli` installed: `pip install -e .` from [github.com/nemoaigc/x-cli](https://github.com/nemoaigc/x-cli)
- Auth: either a saved profile (`x-cli auth add NAME`) or `TWITTER_AUTH_TOKEN` +
  `TWITTER_CT0` env vars
- First run: `x-cli auth status` verifies auth

## Output shape

Every `x-cli` invocation emits JSON to stdout:

```json
{"ok": true,  "schema_version": "1", "data": {...}}
{"ok": false, "schema_version": "1", "error": {"code": "...", "message": "..."}}
```

Final deliverables for the skill are Markdown reports written to the working directory:
- Read mode: `./x-cli-<slug>-<YYYYMMDD>/`
- Trend mode: `./x-trend-<YYYYMMDD>/`
- Write mode: no deliverable file; result is the action confirmation in stdout + audit log

## Other docs (load as needed)

- `references/LLMs.md` — full command map, all flags, failure-modes table.
  Read on any non-happy-path error.
- `references/spam-patterns.md` — domain-bucketed regex catalog
- `references/ranking-weights.md` — per-intent weight + boost rubric, including
  `author_diversity` decay/floor guidance
- `references/trend-mode.md` / `references/write-mode.md` — mode-specific details

## Quick-reference invocation cheatsheet

```bash
# Read
x-cli search "claude code" --since 2026-05-10 --until 2026-05-17 --top 30
x-cli user karpathy --recommended --top 10
x-cli user --recommended --top 20            # general recs (no handle)
x-cli trend scan

# Self
x-cli me status
x-cli me health
x-cli me likes --max 50

# Write (--dry-run is the default; --yes actually executes)
x-cli post --text "Hello"
x-cli post --text "Hello" --yes
x-cli follow add karpathy --yes
x-cli engage like 1234567890                # no --yes gate (immediate)
```

## Write-mode discipline (TL;DR)

Never auto-`--yes` on `post`/`follow`/`x-list`. Dry-run → show user verbatim → wait
for explicit OK → then `--yes`. Audit log at `~/.config/x-cli/write-log.jsonl`.

Full discipline + edge cases: see `references/write-mode.md`.

## Non-goals

- DM (private messages) — out of scope (privacy/ethics)
- Real-time streaming — batch only
- Communities, Spaces operations — deferred
- Scheduled tweets / bookmark folders — X Premium only, deferred
