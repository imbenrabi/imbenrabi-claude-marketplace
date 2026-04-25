# agent-guidelines

Auto-triggering skills that steer **how** the assistant approaches coding work — distinct from `coding-standards`, which governs **what** the produced code should look like.

## How It Works

Skills in this plugin activate from conversation context whenever the assistant is about to write, review, or refactor code. They are read-only — they shape behavior, they do not modify files.

## Skills

| Skill | Triggers when | Guidelines enforced |
|-------|---------------|---------------------|
| `karpathy-guidelines` | Writing, reviewing, refactoring, or planning any non-trivial code change | Think before coding, simplicity first, surgical changes, goal-driven execution |

## Why a separate plugin

`coding-standards` enforces **output style** per language (TSDoc, Google docstrings, no `any`, etc.). `agent-guidelines` enforces **process** — assumptions surfaced, diffs kept minimal, verifiable success criteria stated up front. Different concerns, separately installable, so users can take one without the other.

## Source

The `karpathy-guidelines` skill is derived from [Andrej Karpathy's observations on LLM coding pitfalls](https://x.com/karpathy/status/2015883857489522876).
