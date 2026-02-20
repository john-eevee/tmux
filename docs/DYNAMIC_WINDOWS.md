# Dynamic Window Management in Tmux

Your tmux configuration now supports **multiple approaches** for creating flexible, unlimited windows. No more naming limits!

## Problem Solved

**Before:** You had fixed shortcuts (e=editor, b=build, g=logs) that created only one window each.

**Now:** You can create as many windows as you need, with any names you want.

## Solution: Four Approaches (Pick Your Style)

### Approach 1: Interactive Naming (Recommended for Flexibility)

**Best for:** When you want custom names for each window

```bash
Ctrl+a T    # Create terminal with custom name
# Prompt: "Window name (default: term):"
# Type: "npm" and press Enter
# Creates window named "npm"

Ctrl+a T    # Create another
# Type: "tests"
# Creates window named "tests"

Ctrl+a T    # Create another
# Type: "docs"
# Creates window named "docs"
```

Now you have:
```
Window 1: editor
Window 2: npm
Window 3: tests
Window 4: docs
Window 5: build
Window 6: logs
```

Navigate with:
```bash
Ctrl+a n        # Next window
Ctrl+a p        # Previous window
Ctrl+a 1-9      # Jump by number
Ctrl+a w        # List windows and search
```

### Approach 2: Auto-Numbered Tool Windows (Best for Quick Creation)

**Best for:** When you just want to spin up multiple windows fast

```bash
Ctrl+a M-t      # Auto-create tool1
Ctrl+a M-t      # Auto-create tool2
Ctrl+a M-t      # Auto-create tool3
Ctrl+a M-t      # Auto-create tool4
```

No prompts, instant creation! Windows are named `tool1`, `tool2`, `tool3`, etc.

Navigate:
```bash
Ctrl+a 4        # Jump to tool1
Ctrl+a 5        # Jump to tool2
Ctrl+a 6        # Jump to tool3
```

### Approach 3: Generic Add Window (Most Flexible)

**Best for:** General-purpose window creation with optional commands

```bash
Ctrl+a a        # Add new window
# Prompt: "Window name:"
# Type: "watcher"
# Creates window named "watcher"

Ctrl+a a
# Type: "git-status"
# Creates window named "git-status"
```

### Approach 4: Generic Create Window (Tmux Default)

**Best for:** When you want built-in tmux behavior with names

```bash
Ctrl+a c        # Standard tmux: create window
# Prompt: "New window name:"
# Type whatever you want
```

Same as `Ctrl+a T` but works with any previous version of tmux.

## Complete Example Workflow

```bash
# Start project
tmux-project webapp ~/projects/webapp

# Now you're in "edit" window with nvim

# Add development tools
Ctrl+a T
# Type: "dev-server"
# Runs dev server here

Ctrl+a T
# Type: "tests"
# Run tests here

Ctrl+a T
# Type: "git"
# Do git operations here

Ctrl+a T
# Type: "build"
# Run build scripts here

# Now switch between them
Ctrl+a n        # Next window (wrap around)
Ctrl+a p        # Previous window
Ctrl+a 1        # Jump to editor
Ctrl+a 2        # Jump to dev-server
Ctrl+a 3        # Jump to tests
Ctrl+a 4        # Jump to git
Ctrl+a 5        # Jump to build

# Or list all and search
Ctrl+a w        # Shows all windows, can search by name
```

## Comparison Table

| Feature | Ctrl+a T | Ctrl+a M-t | Ctrl+a a | Ctrl+a c |
|---------|----------|-----------|----------|----------|
| Prompts for name | ✓ | ✗ (auto) | ✓ | ✓ |
| Fastest | ✗ | ✓ | ✗ | ✗ |
| Custom names | ✓ | ✗ | ✓ | ✓ |
| Auto-numbering | ✗ | ✓ | ✗ | ✗ |
| Best for | Organizing | Speed | Flexibility | Standard |

## Pro Tips

### Rename After Creation
If you created with auto-numbering but want better names:

```bash
# Create with auto-numbering
Ctrl+a M-t      # Creates tool1

# Rename it
Ctrl+a ,        # Rename window
# Type: "webpack"
# Now it's named "webpack"
```

### List All Windows by Name
```bash
Ctrl+a w        # Shows all windows in session
# Can type to search: "dev" shows all dev-* windows
```

### Jump to Window by Name
```bash
Ctrl+a f        # Find window (tmux built-in)
# Type part of name, press Enter
```

### See Which Window You're In
```bash
Ctrl+a q        # Show pane numbers (status bar also shows window name)
```

### Auto-Execute Commands
If you want a window to **start a command automatically**, do this:

```bash
# Manual way
Ctrl+a b
# Type: "build-watch"
# Then immediately type: npm run watch
# Press Enter

# Or use this shortcut in your shell:
# Edit the keybinding to auto-run after naming
```

## Recommended Workflow

Based on your needs, here's the suggested approach:

**For Fixed Project Setup:**
```bash
tmux-project myapp ~/path
Ctrl+a T        # Terminal 1
Ctrl+a T        # Terminal 2  
Ctrl+a b        # Build window
# You have: editor, build, term1, term2, plus logs
```

**For Dynamic Multi-Tool Projects:**
```bash
tmux-project myapp ~/path

# Create custom-named windows as needed
Ctrl+a T        # dev-server
Ctrl+a T        # test-watch
Ctrl+a T        # log-tail
Ctrl+a b        # build-prod
# Organized, descriptive names
```

**For Maximum Speed (No Thinking):**
```bash
tmux-project myapp ~/path

# Just spam this when you need new windows
Ctrl+a M-t      # tool1
Ctrl+a M-t      # tool2
Ctrl+a M-t      # tool3
# Jump with Ctrl+a 1, 2, 3, etc.
```

## Advanced: Custom Scripts

If you want even more power, you can create custom functions in your Fish shell:

Add to `~/.config/fish/config.fish`:

```fish
# Create window and jump to it
function tmux-new -d "Create and switch to new tmux window"
    set -l name (read -P "Window name: ")
    tmux new-window -n "$name" -c "#{pane_current_path}"
    tmux select-window -t "#{session_name}:-1"
end

# Create numbered tool window
function tmux-tool -d "Quick auto-numbered tool window"
    set -l count (tmux list-windows | wc -l)
    tmux new-window -n "tool$count"
end
```

Then use:
```bash
tmux-new           # Interactive
tmux-tool          # Auto-numbered
```

## Keyboard Shortcuts Summary

```
Creation:
  Ctrl+a c        Standard tmux create (prompts for name)
  Ctrl+a T        Terminal window (prompts for name)
  Ctrl+a b        Build window (prompts for name)
  Ctrl+a g        Logs window (prompts for name)
  Ctrl+a a        Add window (prompts for name)
  Ctrl+a M-t      Auto-numbered tool (no prompt)

Navigation:
  Ctrl+a n        Next window
  Ctrl+a p        Previous window
  Ctrl+a 1-9      Jump to window number
  Ctrl+a w        List all windows (searchable)
  Ctrl+a f        Find window by name (tmux built-in)

Management:
  Ctrl+a ,        Rename window
  Ctrl+a w        Kill window
  Ctrl+a &        Kill window (confirm prompt)
```

## FAQ

**Q: Can I have both fixed and dynamic windows?**
A: Yes! Use the tmux-project script to create standard editor/build/logs, then add more with Ctrl+a T or Ctrl+a M-t.

**Q: What if I forget window names?**
A: Press `Ctrl+a w` to list all windows with numbers and names.

**Q: Can I auto-start commands in windows?**
A: Yes, after creating the window, type your command. Or bind custom keybindings if you use specific workflows repeatedly.

**Q: Do window numbers stay the same?**
A: Yes, they auto-renumber if you kill a window (configurable with `renumber-windows on`).

**Q: Can I save window layouts?**
A: Not built-in, but you can save session layouts with `tmux capture-pane`, or use tmux-resurrect plugin.

---

**Your terminal is now truly flexible!** Create as many windows as you need, with any names you want, in seconds. 🚀
