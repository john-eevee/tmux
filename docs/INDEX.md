# Documentation Index

Complete guide to tmux configuration and usage.

## Start Here

**New to tmux?** Start with [QUICKSTART.md](QUICKSTART.md) (5 minute intro)

**Need a command?** Check [KEYBINDINGS.md](KEYBINDINGS.md) (complete reference)

**Want to learn workflows?** Read [WORKFLOWS.md](WORKFLOWS.md) (real examples)

## All Documents

| Document | Purpose | Best For |
|----------|---------|----------|
| **README.md** | Overview & navigation | Understanding the setup |
| **QUICKSTART.md** | Getting started | First-time users |
| **KEYBINDINGS.md** | Complete key reference | Looking up commands |
| **WORKFLOWS.md** | Practical examples | Learning patterns |
| **ARCHITECTURE.md** | Design & philosophy | Understanding why |
| **SESSION_MANAGEMENT.md** | Advanced session features | Power users |

## Navigation Flowchart

```
Start
  │
  ├─→ "I've never used tmux"
  │   └─→ QUICKSTART.md
  │
  ├─→ "I need to find a key"
  │   └─→ KEYBINDINGS.md
  │
  ├─→ "I want to learn a workflow"
  │   └─→ WORKFLOWS.md
  │
  ├─→ "I want to understand the design"
  │   └─→ ARCHITECTURE.md
  │
  └─→ "I want to manage sessions advanced"
      └─→ SESSION_MANAGEMENT.md
```

## By Use Case

### Learning Path

1. **First 5 minutes**: [QUICKSTART.md](QUICKSTART.md)
2. **Next 30 minutes**: [KEYBINDINGS.md](KEYBINDINGS.md)
3. **Practical work**: [WORKFLOWS.md](WORKFLOWS.md)
4. **Deep understanding**: [ARCHITECTURE.md](ARCHITECTURE.md)
5. **Advanced features**: [SESSION_MANAGEMENT.md](SESSION_MANAGEMENT.md)

### Quick Reference

- `Ctrl+a /` - Interactive keybindings menu (in tmux)
- `Ctrl+a ?` - All keybindings (in tmux)
- [KEYBINDINGS.md](KEYBINDINGS.md) - Reference guide
- [QUICKSTART.md](QUICKSTART.md) - Common tasks

### Workflows

Need to do something specific?

- **Create session**: [QUICKSTART.md](QUICKSTART.md) → "Your First Session"
- **Switch projects**: [QUICKSTART.md](QUICKSTART.md) → "Resume Work Later"
- **Split windows**: [KEYBINDINGS.md](KEYBINDINGS.md) → "Panes"
- **Web development**: [WORKFLOWS.md](WORKFLOWS.md) → "Web Development"
- **Backend services**: [WORKFLOWS.md](WORKFLOWS.md) → "Backend Services"
- **Debugging**: [WORKFLOWS.md](WORKFLOWS.md) → "Debugging & Troubleshooting"
- **DevOps**: [WORKFLOWS.md](WORKFLOWS.md) → "DevOps & Infrastructure"
- **Session tricks**: [SESSION_MANAGEMENT.md](SESSION_MANAGEMENT.md)

## Files & Organization

```
~/.config/tmux/
├── tmux.conf                           Main configuration file
├── bin/
│   ├── session-manager.sh              Session management utility
│   └── session-quick.sh                Quick session switcher
├── .sessions/                          Session metadata & snapshots
└── docs/
    ├── README.md                       ← Start here for overview
    ├── INDEX.md                        ← This file
    ├── QUICKSTART.md                   → Getting started (5 min)
    ├── KEYBINDINGS.md                  → Complete reference
    ├── WORKFLOWS.md                    → Real examples
    ├── ARCHITECTURE.md                 → Design decisions
    └── SESSION_MANAGEMENT.md           → Advanced sessions
```

## Key Concepts

### Three Levels

1. **Sessions** - Complete workspaces (one per project)
2. **Windows** - Organized sections (edit, build, logs)
3. **Panes** - Split views (side-by-side editing)

See [ARCHITECTURE.md](ARCHITECTURE.md) for detailed explanation.

### Core Keybindings

All use prefix: `Ctrl+a`

| What | Key | Learn More |
|------|-----|-----------|
| Create session | `C` | [QUICKSTART.md](QUICKSTART.md) |
| Switch session | `S` | [KEYBINDINGS.md](KEYBINDINGS.md#sessions) |
| Create window | `c` | [KEYBINDINGS.md](KEYBINDINGS.md#windows) |
| Navigate panes | `hjkl` | [KEYBINDINGS.md](KEYBINDINGS.md#panes) |
| Split window | `\|` / `-` | [KEYBINDINGS.md](KEYBINDINGS.md#splitting) |
| Detach session | `d` | [QUICKSTART.md](QUICKSTART.md) |

See [KEYBINDINGS.md](KEYBINDINGS.md) for complete list.

### Session Manager Commands

Quick reference (from shell):

```bash
~/.config/tmux/bin/session-manager.sh create myproject ~/path
~/.config/tmux/bin/session-manager.sh select
~/.config/tmux/bin/session-manager.sh list
~/.config/tmux/bin/session-manager.sh create-from-template myproject dev
```

See [SESSION_MANAGEMENT.md](SESSION_MANAGEMENT.md) for all commands.

## Frequently Asked

**"Where do I start?"**
→ [QUICKSTART.md](QUICKSTART.md)

**"How do I switch between projects?"**
→ [QUICKSTART.md](QUICKSTART.md) → "Resume work later"

**"What key does...?"**
→ [KEYBINDINGS.md](KEYBINDINGS.md)

**"Show me an example"**
→ [WORKFLOWS.md](WORKFLOWS.md)

**"Why is it designed this way?"**
→ [ARCHITECTURE.md](ARCHITECTURE.md)

**"How do I manage sessions?"**
→ [SESSION_MANAGEMENT.md](SESSION_MANAGEMENT.md)

**"I'm confused"**
→ Press `Ctrl+a /` in tmux for interactive menu

## Key Files

### Essential Configuration

- `~/.config/tmux/tmux.conf` - Main config (ready to use, customizable)

### Utilities

- `~/.config/tmux/bin/session-manager.sh` - Session creation & management
- `~/.config/tmux/bin/session-quick.sh` - Fast session switcher

### State

- `~/.config/tmux/.sessions/` - Metadata, snapshots, recovery

## Getting Help

### In tmux

- **See all commands**: `Ctrl+a ?`
- **Interactive menu**: `Ctrl+a /`
- **Specific command**: `tmux list-keys | grep <pattern>`

### In Documentation

1. Check [KEYBINDINGS.md](KEYBINDINGS.md) for any command
2. See [WORKFLOWS.md](WORKFLOWS.md) for examples
3. Read [ARCHITECTURE.md](ARCHITECTURE.md) for understanding
4. Check [SESSION_MANAGEMENT.md](SESSION_MANAGEMENT.md) for advanced

### Shell

```bash
# List all commands
tmux list-keys

# Test syntax
tmux -f ~/.config/tmux/tmux.conf list-keys

# Session info
tmux list-sessions
tmux list-windows -t session_name
tmux list-panes -t session_name
```

## Tips

1. **Use the menu** - `Ctrl+a /` shows all available commands
2. **Number jumps** - `Ctrl+a 1-9` is faster than `n`/`p`
3. **Fuzzy finder** - `Ctrl+a S` for quick session switching (if fzf installed)
4. **Detach, don't close** - `Ctrl+a d` keeps everything running
5. **Name things** - Clear window/session names make navigation easier

## Next Steps

Choose your adventure:

- **I want to start using tmux right now**
  → [QUICKSTART.md](QUICKSTART.md)

- **I need to look up a specific command**
  → [KEYBINDINGS.md](KEYBINDINGS.md)

- **I want to see real examples**
  → [WORKFLOWS.md](WORKFLOWS.md)

- **I want to understand why it's built this way**
  → [ARCHITECTURE.md](ARCHITECTURE.md)

- **I want to master session management**
  → [SESSION_MANAGEMENT.md](SESSION_MANAGEMENT.md)

---

**Ready?** Start with [QUICKSTART.md](QUICKSTART.md) and you'll be productive in minutes!
