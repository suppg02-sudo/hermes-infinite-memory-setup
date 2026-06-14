# Hermes Agent: Infinite Memory Setup Guide

By default, Hermes Agent has bounded memory (~2,200 characters). To get infinite memory with good recall, configure an external memory provider. **Holographic** is the easiest - zero external dependencies, fully local, one command.

## Quick Start: Holographic (Recommended)

### Installation & Setup

```bash
hermes memory setup
# When prompted, select "holographic"
```

Or set it manually:

```bash
hermes config set memory.provider holographic
```

That's it. Holographic is now active.

### What You Get

- **Infinite local memory** - SQLite fact store with no character limits
- **Full-text search (FTS5)** - fast semantic recall across all stored facts
- **Trust scoring** - the agent learns which memories are reliable
- **HRR algebra** - compositional queries for complex reasoning
- **Zero external services** - runs 100% locally, no API keys needed
- **Automatic session search** - always available, full conversation history searchable

---

## How It Works

### Three Memory Layers (All Active)

1. **Built-in curated memory** (`~/.hermes/memories/MEMORY.md`, `USER.md`)
   - Quick facts injected into every session
   - Agent updates manually via memory tool

2. **Session search** (automatic, no setup needed)
   - Full conversation history stored in `~/.hermes/state.db`
   - FTS5 full-text indexing for instant recall
   - Agent searches on demand when it needs past context

3. **Holographic external provider** (after setup)
   - Semantic fact store with trust scoring
   - Agent adds/updates/recalls facts via the memory tool
   - Supports compositional queries (HRR algebra)

All three work together. Built-in memory stays small and fast. Holographic absorbs everything else.

---

## Alternative Providers (If You Need More)

If Holographic doesn't fit your workflow, Hermes supports 7 other external providers:

| Provider | Setup Effort | Key Feature | Best For |
|----------|---|---|---|
| **Honcho** | Medium | Dialectic reasoning + user modeling | Understanding user patterns over time |
| **Hindsight** | Medium | Cloud or local, trust scoring | Hybrid cloud/local workflow |
| **Mem0** | Medium | Vector embeddings + structured recall | Semantic similarity search |
| **RetainDB** | Medium | Cloud multi-user memory | Teams / shared context |
| **Supermemory** | Low | Simple cloud backup | Easy cloud sync |
| **ByteRover** | Low | Knowledge graph | Entity + relationship recall |
| **OpenViking** | Low | Local semantic search | Privacy-first semantic recall |

To switch providers later:

```bash
hermes memory setup
# Select a different provider
```

Only one external provider can be active at a time. Session search works with all of them.

---

## Daily Use

### The Agent Manages Memory Automatically

The agent has a built-in `memory` tool. It will:

- **Add facts** - when learning something about you or the task
- **Search facts** - when it needs context from past conversations
- **Update facts** - when information changes
- **Remove facts** - when something is no longer relevant

You can also manually edit memory:

```bash
hermes memory list        # Show all stored facts
hermes memory add "fact"  # Manually add a fact
```

### Optional: Approval-Based Writes

If you want to review each memory save before it happens:

```bash
hermes config set memory.write_approval true
```

By default, the agent saves freely (even during background review after each turn).

---

## File Locations

All memory is stored locally in `~/.hermes/`:

```
~/.hermes/
├── memories/
│   ├── MEMORY.md          # Curated facts (injected each session)
│   └── USER.md            # User profile facts (injected each session)
├── state.db               # Session history (SQLite + FTS5)
├── holographic.db         # Holographic fact store (if using Holographic)
└── honcho.json            # Config (if using Honcho)
```

Your data never leaves your machine (unless you choose a cloud provider like RetainDB or Honcho Cloud).

---

## Troubleshooting

### "Memory write failed - limit exceeded"

Built-in memory has a character limit. The agent will refuse to write if MEMORY.md is full.

**Solution:** Let it manage compression, or manually prune old entries from `~/.hermes/memories/MEMORY.md`.

### "Holographic not found"

Hermes didn't install the Holographic plugin.

```bash
pip install hermes-memori
hermes config set memory.provider holographic
```

### Memory isn't being recalled

Session search is automatic but the agent must explicitly call the `session_search` tool. Try:

```
"Remind me what we did last time on [topic]"
```

The agent will search `state.db` and summarize relevant past sessions.

---

## Key Constraints

- **One external provider active at a time** - you can't combine Honcho + Holographic simultaneously (but session search is always available)
- **Session search is local only** - if you run Hermes on multiple machines, history isn't synced unless you manually copy `state.db`
- **Built-in memory is bounded** - MEMORY.md has a ~2,200 character ceiling; Holographic absorbs overflow

---

## Getting More Detailed

For official Hermes Agent docs on memory:
- [Memory providers](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory-providers)
- [Memory system](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory)
- [Desktop app](https://hermes-agent.nousresearch.com/docs/user-guide/desktop)

Questions? Run `hermes --help` or `hermes memory --help`.
