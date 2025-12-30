# Claude Code Context Management

Custom slash commands for Claude Code that provide reliable context persistence across sessions, using an **append-only log** approach instead of lossy compaction.

## The Problem

Claude Code's automatic context compaction, while useful for casual sessions, has significant limitations for extended development work:

- **Information Loss**: Auto-compaction summarizes and discards details, causing Claude to "forget" critical information like variable names, error messages, and architectural decisions
- **Unpredictable Timing**: Compaction triggers automatically at ~75-80% context usage, often at inconvenient moments mid-task
- **Reserved Buffer**: Auto-compact reserves ~22.5% of your context window (45,000 tokens out of 200,000) as a buffer, reducing your available working space
- **Quality Degradation**: Long sessions degrade in quality as the AI progressively loses context through repeated compactions

## The Solution

This project provides two complementary slash commands:

| Command | Purpose |
|---------|---------|
| `/context-dump` | Saves comprehensive project state to a versioned `CONTEXT.md` file |
| `/context-reload` | Intelligently reloads context with structured Chain of Thought reasoning |

These commands implement an **append-only log** approach:

- Context accumulates in versioned files (`CONTEXT.md`, `CONTEXT-v2.md`, `CONTEXT-v3.md`, ...)
- Nothing is ever lost — you can always go back to any previous state
- Each dump captures the complete project state at that moment
- Fresh sessions start with full context window available

## ⚠️ Important: Do Not Mix Approaches

**Never use `/compact` when using this system.** 

The `/compact` command and this context management system are fundamentally incompatible:

| `/compact` (Lossy) | `/context-dump` (Append-Only) |
|--------------------|-------------------------------|
| Discards information | Preserves everything |
| Summarizes and abstracts | Captures full detail |
| Cannot recover lost data | Can restore any version |
| Degrades over time | Maintains fidelity |

**Choose one approach and stick with it.** This system is designed as an append-only log where context is accumulated and preserved. Using `/compact` defeats the entire purpose by introducing data loss.

## Prerequisites

### Step 1: Disable Auto-Compact

Before using these commands, disable Claude Code's automatic context compaction:

1. **In Claude Code CLI**, run:
   ```
   /config
   ```

2. **Navigate to the Auto-Compact setting** and toggle it **OFF**

> **Note**: Disabling auto-compact means your session will end when context is full rather than automatically compacting. This is the intended behavior — you'll use `/context-dump` to save state before starting a fresh session.

### Step 2: Install the Slash Commands

Copy the command files to your Claude Code commands directory:

**For user-level commands** (available in all projects):
```bash
mkdir -p ~/.claude/commands
cp context-dump.md ~/.claude/commands/
cp context-reload.md ~/.claude/commands/
```

**For project-level commands** (available only in specific project):
```bash
mkdir -p .claude/commands
cp context-dump.md .claude/commands/
cp context-reload.md .claude/commands/
```

## Usage

### Monitoring Context

You **don't need** to manually monitor context usage. Even with auto-compact disabled, Claude Code displays remaining context in the status bar:

```
Context low (0% remaining) · Run /compact to compact & continue
```

> **Ignore the suggestion to run `/compact`** — instead, run `/context-dump` when you see **0-5% remaining**.

### Dumping Context

When you see context running low (0-5% remaining):

```
/context-dump
```

This creates a comprehensive snapshot including:

- **Architectural Decisions** — Design patterns, technology choices, rationale
- **Current Implementation State** — What's been built, modified, or deleted
- **Current Plan & Progress** — Overall objectives and completion status
- **Open Issues & Blockers** — Known problems and dependencies
- **Next Steps & TODO** — Prioritized action items
- **Key Learnings & Gotchas** — What works, what failed, what to avoid
- **Testing & Verification Status** — What's tested vs untested

The command automatically versions files:
- First dump: `CONTEXT.md`
- Subsequent dumps: `CONTEXT-v2.md`, `CONTEXT-v3.md`, etc.

### Reloading Context

In a fresh Claude Code session:

```
/context-reload
```

This performs a structured reload with Chain of Thought reasoning:

1. Discovers the latest context file
2. Reads each section progressively
3. Synthesizes understanding between sections
4. Provides a summary confirming what was loaded
5. Identifies failed approaches to avoid repeating

You can also reload a specific version:
```
/context-reload CONTEXT-v3.md
```

## Recommended Workflow

```
┌─────────────────────────────────────────────────────────────┐
│  Start Session                                               │
│  └─> /context-reload (if continuing previous work)          │
├─────────────────────────────────────────────────────────────┤
│  Work on tasks...                                            │
│  └─> Claude Code shows remaining context in status bar      │
├─────────────────────────────────────────────────────────────┤
│  Status bar shows 0-5% remaining                            │
│  └─> /context-dump                                          │
│  └─> Exit session                                           │
├─────────────────────────────────────────────────────────────┤
│  Start fresh session                                         │
│  └─> /context-reload                                        │
│  └─> Continue exactly where you left off                    │
└─────────────────────────────────────────────────────────────┘
```

### Best Practices

1. **Watch the status bar** — No need to run `/context` manually
2. **Dump at 0-5% remaining** — Don't wait until it's completely exhausted
3. **Never run `/compact`** — It defeats the append-only log approach
4. **Review the "Failed Approaches" section** — Avoid repeating mistakes
5. **Keep context files in version control** — Track project evolution
6. **Use descriptive arguments** — `/context-dump "Completed auth module, blocked on OAuth refresh"`

## Why Append-Only Log?

This approach treats your development context like a database transaction log:

| Aspect | Compaction | Append-Only Log |
|--------|------------|-----------------|
| **Philosophy** | Lossy compression | Full preservation |
| **Data Safety** | Information discarded | Nothing ever lost |
| **Recovery** | Cannot recover | Restore any version |
| **Context Available** | ~77.5% (buffer reserved) | 100% |
| **Cross-Session** | No | Yes |
| **History** | Lost | Complete timeline |
| **Human Readable** | No | Yes (Markdown) |
| **Version Control** | Not practical | Git-friendly |

## File Structure

```
your-project/
├── .claude/
│   └── commands/           # Project-level commands (optional)
│       ├── context-dump.md
│       └── context-reload.md
├── CONTEXT.md              # Latest context snapshot
├── CONTEXT-v2.md           # Previous versions (append-only)
├── CONTEXT-v3.md           # Full history preserved
└── ...
```

## Command Reference

### /context-dump

```
/context-dump [optional notes]
```

**Arguments:**
- Optional free-text notes to include in the "Additional Notes" section

**Output:**
- Creates versioned `CONTEXT.md` or `CONTEXT-vN.md` file
- Confirms filename and provides brief summary

### /context-reload

```
/context-reload [optional filename]
```

**Arguments:**
- Optional specific filename to load (defaults to latest version)

**Output:**
- Structured summary of loaded context
- Confirmation of understanding
- Offer to clarify or begin work

## Troubleshooting

### "No context files found"

Run `/context-dump` first to create an initial snapshot.

### Context file seems outdated

Check for newer versions with `ls CONTEXT*.md | sort -V` and reload the specific file needed. Since this is an append-only system, all previous versions are preserved.

### Accidentally ran /compact

Unfortunately, data lost to compaction cannot be recovered. Start fresh with `/context-dump` and continue with the append-only approach going forward.

### VS Code Extension

If using the VS Code extension, note that auto-compact settings may not sync with the CLI. Check the extension settings separately.

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License

This project is licensed under the Apache License 2.0 — see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Inspired by the challenges of maintaining context in long-running Claude Code sessions
- Thanks to the Claude Code community for discussions on context management strategies
