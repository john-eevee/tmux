# Complete Keybindings Reference

All keyboard shortcuts for this tmux configuration. **Prefix key is `Ctrl+a`**.

## Quick Lookup by Category

- [Sessions](#sessions) - Create, switch, and manage projects
- [Windows](#windows) - Create, navigate, and organize
- [Panes](#panes) - Split, navigate, and arrange
- [Copy Mode](#copy-mode) - Select and paste text
- [Layouts](#layouts) - Predefined pane arrangements
- [Other](#other) - Utilities and configuration

---

## Sessions

Managing independent projects/workspaces.

| Key | Action | Notes |
|-----|--------|-------|
| `C` | Create new session | Prompts for session name |
| `M-c` | Create with dev layout | Creates: edit, term, logs windows |
| `M-f` | Create with full-dev layout | Creates: edit, build, term, logs |
| `S` | Interactive session switcher | Uses fzf if available, else native menu |
| `s` | List sessions (native tmux) | Browse with arrow keys, select with Enter |
| `M-s` | List sessions with details | Shows window/pane counts |
| `d` | Detach from session | Session keeps running in background |
| `q` | Kill current session | Requires confirmation |
| `M-q` | Kill all other sessions | Useful for cleanup |
| `$` | Rename session | Prompts for new name |
| `(` | Previous session | Cycle backwards |
| `)` | Next session | Cycle forwards |
| `M-l` | Snapshot session state | Save for recovery |

### Session Examples

```bash
# Create a development project
Ctrl+a C
# Type: myproject

# Switch to another session
Ctrl+a S
# Select from fuzzy list

# Detach (keep running)
Ctrl+a d

# Return later
Ctrl+a S
# Select the session
```

---

## Windows

Organize work into separate windows within a session.

| Key | Action | Notes |
|-----|--------|-------|
| `c` | Create new generic window | Uses current directory |
| `T` | Create terminal window | Prompts for custom name |
| `e` | Create editor window | Launches nvim |
| `b` | Create build window | For compilation/scripts |
| `g` | Create logs window | For monitoring output |
| `a` | Create any window | Generic with custom name |
| `M-t` | Auto-numbered tool window | Creates tool1, tool2, etc |
| `n` | Next window | Cycles forward |
| `p` | Previous window | Cycles backward |
| `1-9` | Jump to window number | Direct navigation |
| `w` | List/search windows | Shows all windows, type to search |
| `,` | Rename window | Prompts for new name |
| `&` | Kill window | Requires confirmation (default tmux) |

### Window Examples

```bash
# Create three custom windows
Ctrl+a T        # Terminal window (prompts)
npm             # [You type]

Ctrl+a T        # Another terminal
tests           # [You type]

Ctrl+a T        # Another
docker          # [You type]

# Navigate
Ctrl+a 1        # Go to window 1
Ctrl+a 2        # Go to window 2
Ctrl+a n        # Next window
Ctrl+a w        # List all, search by typing
```

### Dynamic Window Creation

**Three approaches:**

1. **Interactive naming** (custom names)
   ```bash
   Ctrl+a T       # Creates window with prompt
   # Type name and press Enter
   ```

2. **Auto-numbered** (fast, no prompts)
   ```bash
   Ctrl+a M-t     # Creates tool1
   Ctrl+a M-t     # Creates tool2
   # Jump with: Ctrl+a 4, Ctrl+a 5, etc
   ```

3. **Predefined** (standard shortcuts)
   ```bash
   Ctrl+a e       # Editor
   Ctrl+a b       # Build
   Ctrl+a g       # Logs
   ```

---

## Panes

Subdivide a window into multiple viewports.

### Navigation

| Key | Action |
|-----|--------|
| `h` | Focus left pane |
| `j` | Focus down pane |
| `k` | Focus up pane |
| `l` | Focus right pane |
| `o` | Cycle focus to next pane |
| `q` | Show pane numbers |

### Splitting

| Key | Action |
|-----|--------|
| `\|` | Split horizontally (side-by-side) |
| `-` | Split vertically (top-bottom) |
| `%` | Split horizontally (native tmux) |
| `"` | Split vertically (native tmux) |

### Resizing

| Key | Action | Repeat |
|-----|--------|--------|
| `H` | Expand left | Hold for continuous |
| `J` | Expand down | Hold for continuous |
| `K` | Expand up | Hold for continuous |
| `L` | Expand right | Hold for continuous |

Moves 5px per press. Use `-r` flag allows holding key for rapid resizing.

### Other Pane Operations

| Key | Action |
|-----|--------|
| `z` | Toggle zoom (maximize/restore) |
| `!` | Break pane into new window |
| `{` | Swap with previous pane |
| `}` | Swap with next pane |
| `x` | Kill current pane |
| `Space` | Cycle through layouts |

### Pane Examples

```bash
# Split editor window horizontally
Ctrl+a |
# Now two panes side-by-side

# Focus the right pane
Ctrl+a l

# Run a quick command here while editor runs on left
npm test

# Back to editor
Ctrl+a h

# Maximize editor temporarily
Ctrl+a z

# Restore
Ctrl+a z
```

---

## Copy Mode

Select and copy text from tmux panes.

### Entering & Exiting

| Key | Action |
|-----|--------|
| `[` | Enter copy mode (scroll history) |
| `]` | Paste from copy buffer |
| `q` | Quit copy mode (no copy) |

### Selection

| Key | Action |
|-----|--------|
| `v` | Begin text selection |
| `y` | Copy selection (exits copy mode) |
| `<Space>` | Begin selection (alternative) |
| `Enter` | Copy selection (exits copy mode) |

### Navigation in Copy Mode

| Key | Action |
|-----|--------|
| `h/j/k/l` | Navigate (vi mode) |
| `w` | Jump word forward |
| `b` | Jump word backward |
| `e` | Jump to word end |
| `g` | Jump to buffer top |
| `G` | Jump to buffer bottom |

### Search

| Key | Action |
|-----|--------|
| `/` | Search forward |
| `?` | Search backward |
| `n` | Next match |
| `N` | Previous match |

### Copy Mode Examples

```bash
# Copy output from previous command
Ctrl+a [           # Enter copy mode
k                  # Move up to find text
v                  # Start selection
j j j              # Select multiple lines
y                  # Copy (exits)
Ctrl+a ]           # Paste into current pane
```

### Mouse Selection

- **Click & drag** - Selects text
- **Auto-copy** - Selected text automatically copies to clipboard
- **Scroll wheel** - Navigate history while in normal mode
- **Drag pane border** - Resize panes

---

## Layouts

Predefined pane arrangements for common use cases.

| Key | Action | Arrangement |
|-----|--------|------------|
| `L` | Show layout menu | Interactive selection |
| `Space` | Cycle next layout | Rotates through layouts |
| `1` | Main vertical | 1 large left, stacked right |
| `2` | Main horizontal | 1 large top, stacked bottom |
| `3` | Tiled | All panes equal size |
| `4` | Even vertical | Equal width columns |
| `5` | Even horizontal | Equal height rows |

### Layout Visualization

**Main Vertical (1):**
```
┌──────────────┬────────────┐
│              │ pane2      │
│ pane1        ├────────────┤
│              │ pane3      │
└──────────────┴────────────┘
```

**Main Horizontal (2):**
```
┌──────────────────────────┐
│ pane1                    │
├──────┬───────┬───────────┤
│ pane2│pane3  │ pane4     │
└──────┴───────┴───────────┘
```

**Tiled (3):**
```
┌──────────────┬────────────┐
│ pane1        │ pane2      │
├──────────────┼────────────┤
│ pane3        │ pane4      │
└──────────────┴────────────┘
```

### Layout Examples

```bash
# Show all available layouts
Ctrl+a L

# Apply main vertical
Ctrl+a 1

# Cycle to next
Ctrl+a Space

# Cycle again
Ctrl+a Space
```

---

## Other

Utilities, configuration, and miscellaneous commands.

| Key | Action |
|-----|--------|
| `r` | Reload tmux configuration |
| `/` | Show which-key menu (interactive) |
| `?` | List all keybindings (native) |
| `t` | Display time in current pane |
| `:` | Enter tmux command mode |
| `;` | Repeat last tmux command |

### Command Mode

Press `Ctrl+a :` to enter command mode. Examples:

```bash
# Resize pane to 80 width
resize-pane -x 80

# Send command to pane
send-keys -t 0 "npm run dev" Enter

# Split and run command
split-window -h; send-keys "npm test" Enter
```

---

## Reference by Use Case

### "I need to..."

**...navigate between my open windows**
```bash
Ctrl+a 1          # Jump to window 1
Ctrl+a n          # Next window
Ctrl+a p          # Previous window
Ctrl+a w          # List and search
```

**...compare two files side-by-side**
```bash
Ctrl+a |          # Split horizontally
Ctrl+a h/l        # Navigate between panes
nvim file1        # Left pane
nvim file2        # Right pane
```

**...see command output while editing**
```bash
Ctrl+a e          # Edit window
Ctrl+a |          # Split for terminal
Ctrl+a l          # Focus right pane
npm run dev       # Run there
Ctrl+a h          # Back to editor
```

**...monitor logs in real-time**
```bash
Ctrl+a g          # Logs window
tail -f app.log   # Monitor
Ctrl+a 1          # Work in editor
                  # Logs keep running
```

**...see all windows at once**
```bash
Ctrl+a 3          # Tiled layout
                  # All panes visible equally
```

**...focus on one pane**
```bash
Ctrl+a z          # Zoom (maximize)
# Do focused work
Ctrl+a z          # Restore layout
```

**...copy command output**
```bash
Ctrl+a [          # Copy mode
# Navigate to output
v                 # Start selection
# Select lines
y                 # Copy
Ctrl+a ]          # Paste
```

**...save my work and switch projects**
```bash
Ctrl+a d          # Detach (session persists)
Ctrl+a S          # Switch to another project
# Later, return to first project
Ctrl+a S          # Select
```

---

## Common Sequences

### Set up a typical web dev session

```bash
Ctrl+a C                # Create session
myapp                   # Enter name

Ctrl+a e                # Editor window
# nvim opens in window 1

Ctrl+a T                # Terminal window
npm                     # Enter: npm

Ctrl+a T                # Another terminal
logs                    # Enter: logs

Ctrl+a 1                # Go to editor
Ctrl+a |                # Split for quick commands
Ctrl+a l                # Right pane
npm test                # Run tests here
```

### Resume interrupted work

```bash
Ctrl+a d                # Detach current session
                        # Do something else...
Ctrl+a S                # List sessions
myapp                   # Select
                        # Everything exactly as you left it
```

### Compare two branches

```bash
Ctrl+a 1                # Go to editor
Ctrl+a |                # Split
Ctrl+a h                # Left pane
git checkout main       # One branch
Ctrl+a l                # Right pane
git checkout feature    # Other branch
# Compare side-by-side
```

---

## Prefix Alternatives

By default, all commands use `Ctrl+a` as the prefix. Inside neovim or other apps, you can:

1. **Press prefix twice** - `Ctrl+a Ctrl+a` sends `Ctrl+a` to the app
2. **Use keybindings without prefix** - Root keybindings with `-n` flag (not configured by default)
3. **Change prefix** - Edit `tmux.conf` to use different key

---

## Tips for Learning

1. **Muscle memory** - The hjkl navigation (vi keys) matches vim/neovim
2. **Prefix everything** - Always press `Ctrl+a` first
3. **Use numbers for windows** - `Ctrl+a 1-9` is faster than `n` and `p`
4. **See what's available** - `Ctrl+a /` or `Ctrl+a ?` anytime
5. **Patterns emerge** - Navigation keys are similar across different categories

---

## Cheat Sheet

Print this for quick reference:

```
NAVIGATION:        CREATION:          MANIPULATION:
h/j/k/l  panes     c    window         z    zoom
n/p      window    e    editor         |/-  split
1-9      jump      b    build          S    sessions
S        sessions  g    logs           d    detach

Copy Mode:
[  enter    v  select    y  copy    ]  paste
```

---

See [QUICKSTART.md](QUICKSTART.md) for beginner-friendly guides or [WORKFLOWS.md](WORKFLOWS.md) for practical examples.
