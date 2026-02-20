# Tmux Configuration - Complete Setup Index

Your tmux configuration is now fully integrated with wezterm and neovim.

## 📁 Where Everything Is

### Configuration Files
- `~/.config/tmux/tmux.conf` - Main configuration (11KB)
- `~/.tmux.conf` - Symlink to main config
- `~/.config/tmux/plugins/catppuccin/tmux/` - Theme plugin

### Documentation (Read These!)
- **START HERE:** `~/.config/tmux/CHEATSHEET.txt` - Quick reference card
- `~/.config/tmux/README.md` - Complete guide with examples
- `~/.config/tmux/SETUP_COMPLETE.md` - Setup details & troubleshooting
- `~/.config/tmux/INDEX.md` - This file

### Scripts
- `~/.local/bin/tmux-project` - Helper script for quick project sessions

## 🎯 Quick Start (Choose One)

### Fastest Way (Recommended)
```bash
tmux-project myapp ~/path/to/project
# Automatically creates: edit (nvim), build, logs windows
```

### Manual Way
```bash
tmux new-session -s myproject
Ctrl+a e  # Editor with nvim
Ctrl+a b  # Build window  
Ctrl+a g  # Logs window
```

## 📚 Documentation Map

| Need | File | What |
|------|------|------|
| Quick reference | `CHEATSHEET.txt` | ASCII cheat sheet with all keys |
| Dynamic windows | `DYNAMIC_WINDOWS.md` | **NEW!** Multiple ways to create unlimited windows |
| Learn workflow | `README.md` | Detailed guide, examples, tips |
| Understand setup | `SETUP_COMPLETE.md` | Architecture, decisions, troubleshooting |
| Customize keys | `tmux.conf` | The actual config (commented) |

## ⌨️ Most Used Keys

**Prefix = Ctrl+a**

| Action | Keys |
|--------|------|
| New project session | `Ctrl+a C` |
| Editor window (nvim) | `Ctrl+a e` |
| Build window | `Ctrl+a b` |
| Logs window | `Ctrl+a g` |
| Next window | `Ctrl+a n` |
| Jump to window | `Ctrl+a 1-9` |
| Split horizontal | `Ctrl+a \|` |
| Split vertical | `Ctrl+a -` |
| Navigate pane | `Ctrl+a hjkl` |
| Zoom pane | `Ctrl+a z` |
| Detach session | `Ctrl+a d` |
| List sessions | `Ctrl+a s` |
| Kill session | `Ctrl+a q` |

**See CHEATSHEET.txt for complete reference**

## 🏗️ Architecture

```
WezTerm Window
  │
  ├─ Tab 1 (Project A)
  │   └─ Tmux Session A
  │       ├─ Window: edit (nvim)
  │       ├─ Window: build
  │       └─ Window: logs
  │
  ├─ Tab 2 (Project B)
  │   └─ Tmux Session B
  │       ├─ Window: edit
  │       ├─ Window: build
  │       └─ Window: logs
  │
  └─ Tab 3 (Project C)
      └─ Tmux Session C
          ├─ Window: edit
          ├─ Window: build
          └─ Window: logs
```

## 💡 Key Concepts

- **Wezterm Tab** = Project container (visual isolation)
- **Tmux Session** = Project workspace (persists when detached)
- **Tmux Window** = Separates concerns (editor vs build vs logs)
- **Tmux Pane** = Subdivides window (side-by-side or stacked)

## 🔧 Common Tasks

### Start a new project
```bash
tmux-project webapp ~/projects/webapp
# Done! You're in the editor window with nvim running
```

## Common Tasks

### Create Multiple Tool Windows

**Option 1: Named windows (interactive)**
```bash
Ctrl+a T        # Prompts: "Window name?"
# Type: npm
Ctrl+a T        # Prompts: "Window name?"
# Type: tests
# Now you have: editor, npm, tests windows
```

**Option 2: Auto-numbered (fast, no prompts)**
```bash
Ctrl+a M-t      # Creates tool1
Ctrl+a M-t      # Creates tool2
Ctrl+a M-t      # Creates tool3
# Jump with: Ctrl+a 4, Ctrl+a 5, Ctrl+a 6
```

**Option 3: List and search all windows**
```bash
Ctrl+a w        # Shows all windows with numbers
# Can type to search: "npm" finds npm window
# Press Enter to jump there
```

See **DYNAMIC_WINDOWS.md** for comprehensive guide on unlimited window creation.

### Pause work (keep session running)
```bash
Ctrl+a d   # Detach (you're back at shell)
           # Session still running in background!
```

### Resume work later
```bash
tmux attach -t webapp   # Reconnect to session
# or
Ctrl+a s                # List sessions and select
```

### Kill a session when done
```bash
Ctrl+a q   # Quit current session
# or from shell:
tmux kill-session -t webapp
```

## 🎨 Theme

Using **Catppuccin Mocha** to match your wezterm colors.

If theme doesn't load, verify:
```bash
ls ~/.config/tmux/plugins/catppuccin/tmux/
# Should see: catppuccin.tmux, colors, modules, etc.
```

If missing:
```bash
mkdir -p ~/.config/tmux/plugins/catppuccin
git clone -b v2.1.3 https://github.com/catppuccin/tmux.git \
  ~/.config/tmux/plugins/catppuccin/tmux
```

## 🐛 Troubleshooting

### Config won't load
```bash
tmux source ~/.config/tmux/tmux.conf
# Or inside tmux: Ctrl+a r
```

### Colors look wrong
```bash
echo $TERM
# Should output: xterm-256color or similar
# If wrong, wezterm isn't configured correctly
```

### Session won't start
```bash
tmux kill-server      # Reset tmux
tmux new-session -s test
```

### Script not found
```bash
# Ensure ~/.local/bin is in PATH
echo $PATH | grep .local/bin

# If not, add to ~/.config/fish/config.fish:
set -gx PATH ~/.local/bin $PATH
```

### Session already exists
```bash
# Either:
tmux attach -t existing-name   # Use existing
tmux kill-session -t name      # Then create new
```

## ✨ What You Get

✓ Three-layer terminal organization  
✓ Persistent sessions (detach/reattach)  
✓ Project-focused window shortcuts  
✓ Vi-style keybindings (hjkl)  
✓ Mouse support  
✓ Catppuccin Mocha theme  
✓ Helper script for quick setup  
✓ Comprehensive documentation  

## 🚀 Next Steps

1. **Try it**: `tmux-project test`
2. **Explore layouts**: `Ctrl+a L` to see presets
3. **Read docs**: `cat ~/.config/tmux/CHEATSHEET.txt`
4. **Customize**: Edit `~/.config/tmux/tmux.conf` as needed

## 📖 Full Documentation

For complete guides and examples, see:
- `~/.config/tmux/README.md` - Full documentation with workflows
- `~/.config/tmux/CHEATSHEET.txt` - Quick reference with ASCII diagrams

---

Your terminal workflow is now optimized! 🎉
