# Architecture & Design Decisions

Understanding the structure and philosophy behind this tmux configuration.

## Table of Contents

- [Three-Layer Organization](#three-layer-organization)
- [Design Decisions](#design-decisions)
- [Session Management System](#session-management-system)
- [Keybinding Philosophy](#keybinding-philosophy)
- [Performance Considerations](#performance-considerations)
- [Integration Points](#integration-points)

---

## Three-Layer Organization

The fundamental architecture consists of three nested levels:

### Layer 1: Sessions

A **session** is a completely independent workspace, typically representing one project.

```
Session "webapp"          Session "api"          Session "devops"
    │                         │                      │
    ├─ Windows               ├─ Windows              ├─ Windows
    └─ Running processes     └─ Running processes   └─ Running processes
```

**Characteristics:**
- **Independent**: Each session runs separately
- **Persistent**: Sessions survive terminal closure via `Ctrl+a d` (detach)
- **Reattachable**: Resume exactly where you left off with `Ctrl+a S`
- **Isolated**: Processes in one session don't affect others

**Use case:** One session per project, switch between projects by detaching/attaching

### Layer 2: Windows

A **window** is a subdivision within a session, typically organizing work by concern.

```
Session "webapp"
    │
    ├─ Window 1: edit (code editor)
    │   └─ Running: nvim
    │
    ├─ Window 2: build (compilation/scripts)
    │   └─ Running: npm run dev
    │
    ├─ Window 3: logs (monitoring)
    │   └─ Running: tail -f app.log
    │
    └─ Window 4: tests (continuous testing)
        └─ Running: npm test --watch
```

**Characteristics:**
- **Compartmentalized**: Each window has its own working directory and process
- **Navigable**: Jump to any window with `Ctrl+a 1-9` or `Ctrl+a n/p`
- **Independent history**: Each pane has scrollback buffer
- **Flexible count**: Create as many as needed

**Naming convention:**
- `edit` or `editor` - Neovim or text editor
- `build` or `dev` - Build commands, compilation
- `logs` or `monitor` - Log tailing, output monitoring
- `tests` - Test runner, continuous testing
- `shell` or `term` - General purpose terminal

### Layer 3: Panes

A **pane** is a split within a window, showing multiple views simultaneously.

```
Window "edit"
    │
    ├─ Pane 1 (left): Code editing
    │   ├─ nvim open
    │   └─ Focus: editing files
    │
    └─ Pane 2 (right): Quick commands
        ├─ Shell prompt
        └─ Focus: running npm commands
```

**Characteristics:**
- **Flexible layout**: Create arbitrary splits with `Ctrl+a |` and `Ctrl+a -`
- **Resizable**: Adjust with `Ctrl+a H/J/K/L`
- **Temporary**: Useful for specific tasks, easy to close
- **Zoomable**: Maximize a pane with `Ctrl+a z`

**Common patterns:**
- Editor + quick terminal side-by-side
- Main view + monitoring pane
- Comparison view (two files/branches)

---

## Design Decisions

### Why This Architecture?

#### Problem: Context Switching

Without tmux, you need:
- Multiple terminal tabs (hard to manage across projects)
- Window switching via OS features (slow, mental overhead)
- Re-running startup commands (time wasted)

With this architecture:
- One session per project (clean separation)
- All processes stay running (detach with `Ctrl+a d`)
- Resume instantly (attach with `Ctrl+a S`)

#### Problem: Visual Organization

Without organized windows:
- All work in one pane (hard to multitask)
- Switching between editor and logs requires full screen changes
- Context lost when switching

With this architecture:
- Editor window open while build runs
- Monitor logs in separate window
- Panes can show multiple views simultaneously

### Why `Ctrl+a` Prefix?

Default tmux uses `Ctrl+b`, but we use `Ctrl+a`:

**Advantages:**
- Matches common SSH multiplexer habits
- Easier to reach on keyboard
- Single key from "home row" position
- Less conflict with common editor shortcuts

**No conflict with applications** because:
- Most apps don't intercept `Ctrl+a` alone
- Apps only see full command sequences (e.g., `Ctrl+a n`)
- Send literal `Ctrl+a` with `Ctrl+a Ctrl+a`

### Why Vi Keybindings?

Navigation uses `hjkl` (vi style):

**Advantages:**
- Matches vim/neovim users' muscle memory
- No hand movement from home row
- Consistent across tmux, vim, zsh, etc.
- Faster than arrow keys

**Alternative:** Could use arrow keys, but `hjkl` is more efficient

### Why No Plugins?

This configuration uses only native tmux features:

**Advantages:**
- Zero dependencies beyond tmux
- Fast startup (no plugin loading)
- Stable (no plugin conflicts)
- Fully customizable (see all code)
- Works anywhere tmux exists

**Trade-offs:**
- Some features need shell scripts (session manager)
- Less dynamic themeing (but Catppuccin theme included)
- Manual configuration required (but well-documented)

### Why Catppuccin Theme?

Color scheme choices:
- Consistent with modern terminal trends
- High contrast for readability
- Available for other tools (nvim, shell, etc.)
- Mocha variant is dark but not black

Can change if desired, see `tmux.conf` around line 350.

---

## Session Management System

The session-manager.sh script provides:

### 1. Session Creation

```bash
session-manager.sh create myproject ~/path
```

Creates:
- Session named `myproject`
- Starting directory set to `~/path`
- Metadata file saved for tracking
- Ready for immediate use

### 2. Session Templates

```bash
session-manager.sh create-from-template myproject dev ~/path
```

Applies predefined window layouts:

**`dev` layout:**
```
├─ edit  (editor)
├─ term  (terminal)
└─ logs  (monitoring)
```

**`full-dev` layout:**
```
├─ edit   (editor)
├─ build  (compilation)
├─ term   (terminal)
└─ logs   (monitoring)
```

### 3. Metadata Tracking

Each session gets a metadata file:

```bash
~/.config/tmux/.sessions/myproject.meta
```

Contents:
```
SESSION_NAME=myproject
START_DIR=/home/user/projects/myproject
CREATED=2024-02-19T10:30:45Z
```

Used for:
- Documentation
- Future recovery features
- Session cleanup

### 4. Session Snapshots

```bash
session-manager.sh snapshot myproject
```

Saves session state for later recovery:

```bash
~/.config/tmux/.sessions/myproject.snapshot
```

Allows:
- Saving complex setup
- Restoring after crashes
- Sharing configurations

### 5. Fuzzy Session Selection

```bash
session-manager.sh select
```

Or from tmux:
```bash
Ctrl+a S
```

Uses fzf if available (fuzzy finding), falls back to native tmux menu.

---

## Keybinding Philosophy

### Logical Organization

Keybindings are organized by function:

| Category | Prefix | Examples |
|----------|--------|----------|
| Sessions | `Ctrl+a` | `S` (switch), `C` (create) |
| Windows | `Ctrl+a` | `c` (create), `n` (next) |
| Panes | `Ctrl+a` | `h/j/k/l` (navigate) |
| Copy | `Ctrl+a` | `[` (enter), `]` (paste) |
| Layouts | `Ctrl+a` | `L` (menu), `1-5` (presets) |

### Consistency

- **Navigation**: Always `hjkl` for vi-style movement
- **Creation**: Usually `c` or `e` or `b` for specific types
- **Deletion**: Often `x` or `w` or `q`
- **Modification**: Usually `$` (rename) or `,` (rename)

### Discoverability

Three ways to find commands:

1. **Which-key menu**: `Ctrl+a /` (interactive, categorized)
2. **Native list**: `Ctrl+a ?` (raw, comprehensive)
3. **Documentation**: This file

---

## Performance Considerations

### Memory Usage

This configuration is lightweight:

- **Tmux process**: ~5MB base
- **Session metadata**: ~1KB per session
- **Keybinding definitions**: Negligible
- **Theme loading**: ~100KB

No background services, no polling.

### Startup Time

- **First attach**: Instant (tmux already running)
- **Session creation**: <100ms
- **Window creation**: <50ms
- **Script execution**: <200ms

### CPU Usage

- **Idle**: 0% (truly idle)
- **Scrolling**: Minimal
- **Copy mode**: Minimal
- **Layout switching**: Minimal

Tmux is highly optimized for terminal multiplexing.

### Network Impact

- **Session persistence**: Local only
- **No external calls**: Scripts are all local
- **fzf integration**: Local fuzzy finding
- **No telemetry**: Fully offline

---

## Integration Points

### With Neovim

Tight integration possible:

```lua
-- In nvim config
vim.keymap.set('n', '<C-a>h', '<C-w>h')   -- Navigate to left pane
vim.keymap.set('n', '<C-a>j', '<C-w>j')   -- Navigate to down pane
```

Or use plugins like `christoomey/vim-tmux-navigator`.

### With Shell

Add aliases and functions:

```bash
# ~/.bashrc or ~/.zshrc
alias tm='~/.config/tmux/bin/session-manager.sh'
alias tms='~/.config/tmux/bin/session-manager.sh select'
alias tml='~/.config/tmux/bin/session-manager.sh list'

# Create session in any project
project() {
    tm create "$(basename $PWD)" "$PWD"
}
```

### With Git

Workflow for branch work:

```bash
# Create separate session per branch
git checkout -b feature/new-feature
Ctrl+a C
feature-new-feature

# Edit in isolation
# When done:
git push
Ctrl+a q     # Kill session
```

### With Docker

Container-aware tmux setup:

```bash
# Window for docker-compose
Ctrl+a T
docker

# Run:
docker-compose up

# Another window for shell into container
Ctrl+a T
shell
docker-compose exec app /bin/bash
```

### With External Tools

Session snapshots enable integration:

```bash
# Export sessions as JSON
~/.config/tmux/bin/session-manager.sh export-json

# Use in other tools:
# - Dashboard
# - IDE integrations
# - Automation scripts
```

---

## Customization Points

Key areas for customization:

### Colors & Appearance

Edit `tmux.conf` around line 350:

```bash
set -g @catppuccin_flavor "mocha"
set -g @catppuccin_status_background "default"
```

### Keybindings

Add to `tmux.conf`:

```bash
bind -n M-1 select-layout main-vertical
bind -n M-2 select-layout main-horizontal
```

### Session Templates

Modify `session-manager.sh`:

```bash
case "$layout" in
    custom)
        # Your custom layout here
        tmux new-window -t "$session_name" -n 'window1'
        tmux new-window -t "$session_name" -n 'window2'
        ;;
esac
```

### Prefix Key

Change in `tmux.conf`:

```bash
set -g prefix C-b       # Change from C-a to C-b
bind C-b send-prefix
```

---

## Future Enhancements

Possible additions:

1. **Custom layout files** - Define layouts as configuration
2. **Session presets** - Store complete session configurations
3. **Automatic session recovery** - Restore on crash
4. **Session synchronization** - Run commands across panes
5. **Smart naming** - Auto-detect project type
6. **IDE integration** - Deep vim/nvim/vscode integration
7. **Session bookmarks** - Favorite sessions with quick access
8. **Dashboard** - GUI for session management

---

## Philosophy Summary

This configuration embodies:

1. **Simplicity** - Use only what's needed
2. **Composability** - Small, reusable tools (sessions, windows, panes)
3. **Locality** - No external services, all local
4. **Discoverability** - Commands visible and organized
5. **Efficiency** - Keyboard-driven, optimized for speed
6. **Stability** - No plugins, no external dependencies
7. **Customizability** - Easy to modify and extend

The goal: **Reduce mental overhead and physical effort** in terminal multiplexing so you can focus on your work.

---

See [QUICKSTART.md](QUICKSTART.md) for practical getting started, or [WORKFLOWS.md](WORKFLOWS.md) for real-world examples.
