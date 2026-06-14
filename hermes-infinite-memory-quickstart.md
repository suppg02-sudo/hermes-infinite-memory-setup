# Hermes Infinite Memory Quickstart (Agent-Friendly)

Use this with Hermes for fast setup + Q&A.

## 1) One-command setup (Holographic)

```bash
hermes memory setup
# Select "holographic"
```

Or:

```bash
hermes config set memory.provider holographic
```

## 2) Three memory layers (all active)

1. **Curated memory**: `~/.hermes/memories/MEMORY.md`, `USER.md`
2. **Session search**: `~/.hermes/state.db` (FTS5 conversation recall)
3. **External provider**: `holographic` (infinite fact store + trust scoring)

Built-in memory stays small; external memory stores long-term facts.

## 3) Alternative providers (7)

| Provider | Best For |
|---|---|
| Honcho | User modeling over time |
| Hindsight | Cloud/local hybrid memory |
| Mem0 | Embedding-based semantic recall |
| RetainDB | Team/shared context |
| Supermemory | Easy cloud backup |
| ByteRover | Knowledge-graph style recall |
| OpenViking | Local privacy-first semantic search |

Switch anytime:

```bash
hermes memory setup
```

Only one external provider can be active at once.

## 4) Daily usage

```bash
hermes memory list
hermes memory add "fact"
```

Optional approval before writes:

```bash
hermes config set memory.write_approval true
```

## 5) File locations

```text
~/.hermes/
├── memories/MEMORY.md
├── memories/USER.md
├── state.db
├── holographic.db
└── honcho.json
```

## 6) Troubleshooting highlights

- **Write limit exceeded**: prune `MEMORY.md` or let Hermes compress memory.
- **Holographic missing**:

```bash
pip install hermes-memori
hermes config set memory.provider holographic
```

- **Not recalling context**: ask, "Remind me what we did last time on [topic]".

## 7) Key constraints

- One external provider at a time.
- Session search is local-only (not multi-machine synced by default).
- Built-in memory is bounded; external memory handles overflow.

## Prompt You Can Give Hermes

Use this URL as setup context and answer questions from it:

`https://raw.githubusercontent.com/suppg02-sudo/hermes-infinite-memory-setup/main/hermes-infinite-memory-quickstart.md`
