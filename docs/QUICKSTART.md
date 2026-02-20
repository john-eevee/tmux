# Quick Start Guide

Get up and running with tmux in 5 minutes.

## Installation Check

First, verify tmux is installed and the config loads:

```bash
# Check tmux version
tmux -V
# Should output: tmux 3.0 or higher

# Test configuration
tmux -f ~/.config/tmux/tmux.conf list-keys | head
# Should show keybindings without errors
```

## Your First Session

### Option 1: Using the Session Manager (Recommended)

```bash
# Create a session with default layout
~/.config/tmux/bin/session-manager.sh create myproject ~/projects/myproject

# Alternatively, in tmux:
Ctrl+a C
# Enter: myproject
```

### Option 2: Traditional Method

```bash
# Create a new session
tmux new-session -s myproject -c ~/projects/myproject

# You're now inside tmux!
```

## What You See

Inside tmux, you'll see:
```
┌─────────────────────────────────────────────────────────────┐
│ [edit] [build] [logs]                    myproject           │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Your shell prompt is here                                   │
│  $                                                            │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

The status bar shows:
- **Windows**: `[edit]` `[build]` `[logs]` (or whatever you created)
- **Session name**: `myproject` (on the right)

## Basic Navigation

All commands use the **Prefix** key: `Ctrl+a`

### Move Between Windows

```bash
Ctrl+a n         # Next window →
Ctrl+a p         # Previous window ←
Ctrl+a 1         # Jump to window 1
Ctrl+a 2         # Jump to window 2
```

### Create Windows

```bash
Ctrl+a c         # New generic window
Ctrl+a T         # New terminal (prompts for name)
Ctrl+a e         # New editor window (launches nvim)
Ctrl+a b         # New build window
Ctrl+a g         # New logs window
```

### Split a Window

```bash
Ctrl+a |         # Split horizontally (side-by-side)
Ctrl+a -         # Split vertically (top-bottom)
```

### Navigate Panes (in splits)

```bash
Ctrl+a h         # Focus left pane
Ctrl+a j         # Focus down pane
Ctrl+a k         # Focus up pane
Ctrl+a l         # Focus right pane
```

### Resize Panes

```bash
Ctrl+a H         # Expand left (5px)
Ctrl+a J         # Expand down (5px)
Ctrl+a K         # Expand up (5px)
Ctrl+a L         # Expand right (5px)
```

## Common First Tasks

### 1. View All Windows

```bash
Ctrl+a w         # Shows: 1:edit  2:build  3:logs
```

### 2. Jump to a Window

```bash
Ctrl+a 2         # Go to window 2 (build)
```

### 3. See All Available Commands

```bash
Ctrl+a /         # Interactive which-key menu (shows all commands)
Ctrl+a ?         # Native tmux binding list
```

### 4. Pause Work (Keep Session Running)

```bash
Ctrl+a d         # Detach from session
                 # You're back at the shell
                 # Your session is still running!
```

### 5. Resume Work Later

```bash
# List all sessions
~/.config/tmux/bin/session-manager.sh list

# Or in tmux:
Ctrl+a S         # Interactive fuzzy finder (if fzf installed)
Ctrl+a s         # Native tmux session chooser
```

### 6. Kill a Session (When Done)

```bash
Ctrl+a q         # Kill current session
# Or from shell:
tmux kill-session -t myproject
```

## Layout Templates

Create predefined pane arrangements:

```bash
Ctrl+a L         # Show layout menu
Ctrl+a 1         # Main vertical (1 large left, stacked right)
Ctrl+a 2         # Main horizontal (1 large top, stacked bottom)
Ctrl+a 3         # Tiled (all panes equal)
Ctrl+a Space     # Cycle to next layout
```

## Window Management

### Rename Window

```bash
Ctrl+a ,         # Rename current window
```

### Kill Window

```bash
Ctrl+a w         # Kill current window
```

### Kill Pane

```bash
Ctrl+a x         # Kill current pane (but keep window)
```

## Practical Workflow: Web Development

```bash
# 1. Create session
Ctrl+a C
# Enter: webapp

# 2. Create editor window (in window 1)
Ctrl+a e
# Now running nvim

# 3. Create build window
Ctrl+a b
# Type: npm
# Switch to it: npm run dev

# 4. Create logs window
Ctrl+a g
# Type: tail -f logs/output.log

# 5. Split editor window for quick commands
Ctrl+a |         # Split editor horizontally
Ctrl+a h         # Focus left (editor)
Ctrl+a l         # Focus right (quick terminal)

# 6. Now you can see everything at once!
```

## Copy & Paste

```bash
# Enter copy mode
Ctrl+a [

# Select text
v              # Begin selection
# Move with: hjkl or arrow keys
y              # Copy selection (exits copy mode)

# Paste
Ctrl+a ]
```

### Mouse Support

You can also:
- Click to select pane
- Drag to resize panes
- Scroll wheel to navigate history
- Select text automatically copies to clipboard

## First 10 Commands to Know

1. `Ctrl+a ?` - See all keybindings
2. `Ctrl+a /` - Interactive which-key menu
3. `Ctrl+a c` - Create window
4. `Ctrl+a n` - Next window
5. `Ctrl+a h/j/k/l` - Navigate panes
6. `Ctrl+a |/-` - Split window
7. `Ctrl+a d` - Detach (pause session)
8. `Ctrl+a s` - Switch sessions
9. `Ctrl+a r` - Reload config
10. `Ctrl+a q` - Kill session

## What's Next?

Now that you know the basics:

1. **Learn more keybindings** → [KEYBINDINGS.md](KEYBINDINGS.md)
2. **See practical examples** → [WORKFLOWS.md](WORKFLOWS.md)
3. **Understand the design** → [ARCHITECTURE.md](ARCHITECTURE.md)
4. **Explore session management** → [SESSION_MANAGEMENT.md](SESSION_MANAGEMENT.md)

## Stuck?

If something doesn't work:

```bash
# Check if config loads
tmux -f ~/.config/tmux/tmux.conf list-keys

# Reload config
Ctrl+a r

# List sessions
tmux list-sessions

# Reset everything (last resort)
tmux kill-server
tmux new-session -s test
```

## Tips for Success

1. **Prefix everything**: Remember `Ctrl+a` before every command
2. **Use numbers to jump**: `Ctrl+a 1`, `Ctrl+a 2` is faster than `n` and `p`
3. **Mouse works too**: You can click on windows and drag pane borders
4. **Keyboard shortcuts in your editor**: Nvim can integrate with tmux for even faster navigation
5. **Session persistence**: Don't close tabs/windows—use `Ctrl+a d` to detach

---

**You're ready!** Start with a simple session and explore. Use `Ctrl+a /` whenever you need to see available commands.
