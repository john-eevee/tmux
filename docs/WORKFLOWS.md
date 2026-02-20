# Practical Workflows & Examples

Real-world usage patterns and step-by-step examples for common development tasks.

## Table of Contents

- [Web Development](#web-development)
- [Backend Services](#backend-services)
- [Multi-Language Projects](#multi-language-projects)
- [Debugging & Troubleshooting](#debugging--troubleshooting)
- [DevOps & Infrastructure](#devops--infrastructure)
- [Context Switching](#context-switching)
- [Collaborative Development](#collaborative-development)

---

## Web Development

### React/Vue Frontend Project

**Goal:** Editor, dev server, tests, and logs all visible

```bash
# 1. Create session
$ ~/.config/tmux/bin/session-manager.sh create webapp ~/projects/webapp

# 2. Now in tmux, create windows
Ctrl+a e                    # Editor (nvim) in window 1

Ctrl+a T
npm                         # Window 2: run dev server

Ctrl+a T
tests                       # Window 3: run tests

Ctrl+a T
logs                        # Window 4: monitor logs

# 3. Setup pane layout for monitoring
Ctrl+a 1                    # Go to editor
Ctrl+a |                    # Split for quick commands
Ctrl+a l                    # Right pane
# Now ready for quick npm commands while editing

# 4. Workflow:
Ctrl+a h                    # Left pane: edit code
Ctrl+a l                    # Right pane: npm build
Ctrl+a 2                    # Switch to dev server window
Ctrl+a 4                    # Check logs
```

**Status:**
- Window 1: Editor (nvim) split with quick terminal
- Window 2: `npm run dev` (dev server running)
- Window 3: `npm test` (auto-running tests)
- Window 4: `tail -f logs/app.log` (monitoring)

---

### Full-Stack Project

**Goal:** Frontend, backend, database, and shared logs

```bash
# Create with full-dev template
Ctrl+a M-f
fullstack                   # Session name

# Now you have: edit, build, term, logs windows

# Setup:
Ctrl+a 1 (edit)
# nvim for code editing

Ctrl+a 2 (build)
cd api && npm run dev       # Backend dev server

Ctrl+a 3 (term)
cd frontend && npm run dev  # Frontend dev server

Ctrl+a 4 (logs)
# Tail logs from both services
tail -f api/logs/server.log & tail -f frontend/logs/app.log

# Monitor them all
Ctrl+a 3                    # Switch to active terminal
Ctrl+a 4                    # Check logs
Ctrl+a 1                    # Back to editor
```

---

## Backend Services

### Microservices Architecture

**Goal:** Run 3+ services simultaneously

```bash
# Create session for service deployment
Ctrl+a C
services                    # Session name

# Create windows for each service
Ctrl+a e                    # Editor window (1)

Ctrl+a T
api-gateway                 # Window 2

Ctrl+a T
auth-service                # Window 3

Ctrl+a T
user-service                # Window 4

Ctrl+a T
shared-logs                 # Window 5

# Start services
Ctrl+a 2
cd services/api-gateway && npm start

Ctrl+a 3
cd services/auth-service && npm start

Ctrl+a 4
cd services/user-service && npm start

Ctrl+a 5
# Aggregate logs from all services
tmux send-keys "docker-compose logs -f" Enter

# Development workflow:
Ctrl+a 1                    # Edit code
Ctrl+a 2                    # Check API gateway
Ctrl+a 5                    # See all logs
```

### Database + Server Setup

```bash
# Create session
Ctrl+a C
database-project

# Windows setup
Ctrl+a e                    # Editor (1)

Ctrl+a T
db                          # Window 2: database

Ctrl+a T
server                      # Window 3: application server

Ctrl+a T
shell                       # Window 4: interactive shell

# Start services
Ctrl+a 2
docker-compose up db       # Start database

Ctrl+a 3
npm start                   # Start server

# Interactive queries
Ctrl+a 4
psql -h localhost dbname    # Connect to DB

# While editing in window 1:
Ctrl+a 1 |                  # Split editor
Ctrl+a l
npm run migrate             # Run migrations
```

---

## Multi-Language Projects

### Python + Node.js + Shell Scripts

```bash
# Session for polyglot project
Ctrl+a C
polyglot-app

# Windows for different runtimes
Ctrl+a e                    # Editor (1)

Ctrl+a T
python                      # Window 2: Python environment

Ctrl+a T
node                        # Window 3: Node environment

Ctrl+a T
scripts                     # Window 4: Shell scripts

# Setup Python environment
Ctrl+a 2
python -m venv venv
source venv/bin/activate
python -m pip install -r requirements.txt

# Setup Node environment
Ctrl+a 3
nvm use 18
npm install

# Run both
Ctrl+a 2
python app.py               # Python server

Ctrl+a 3
npm run dev                 # Node server

# Execute scripts
Ctrl+a 4
./scripts/deploy.sh         # Run in window 4
```

---

## Debugging & Troubleshooting

### Step Through Debugger + Logs

**Goal:** Breakpoint debugging with real-time logs

```bash
# Create debug session
Ctrl+a C
debug-session

# Windows
Ctrl+a e                    # Editor with code

Ctrl+a T
debugger                    # Debugger window

Ctrl+a T
logs                        # Real-time logs

Ctrl+a T
shell                       # Quick commands

# Debugging workflow:
Ctrl+a 1                    # Set breakpoint in editor
Ctrl+a 2
node --inspect app.js       # Start debugger

# In another terminal, run:
node inspect localhost:9229

# Monitor side effects in logs:
Ctrl+a 3
tail -f debug.log

# Execute test commands:
Ctrl+a 4
curl localhost:3000/test

# Watch everything:
Ctrl+a 3                    # See logs
Ctrl+a 2                    # Check debugger
```

### Performance Profiling

```bash
# Create session
Ctrl+a C
profile-session

# Layout: editor split with monitor
Ctrl+a 1 |                  # Split editor window
Ctrl+a l

Ctrl+a T
monitor                     # Window 2: system monitor

Ctrl+a T
profile                     # Window 3: profiler

# Run profiler:
Ctrl+a 3
node --prof app.js

# Monitor resources:
Ctrl+a 2
htop                        # See CPU/memory usage

# View flame graph:
Ctrl+a 3
node --prof-process isolate-*.log > profile.txt
cat profile.txt
```

---

## DevOps & Infrastructure

### Docker Development

```bash
# Create DevOps session
Ctrl+a C
docker-dev

# Windows
Ctrl+a e                    # Dockerfile/compose editing (1)

Ctrl+a T
containers                  # Window 2: docker-compose

Ctrl+a T
shell                       # Window 3: container shell

Ctrl+a T
logs                        # Window 4: container logs

# Start containers
Ctrl+a 2
docker-compose up -d

# Shell into container
Ctrl+a 3
docker-compose exec app /bin/bash

# View logs
Ctrl+a 4
docker-compose logs -f

# Development:
Ctrl+a 1                    # Edit Dockerfile
# Make changes
Ctrl+a 2
docker-compose down && docker-compose up   # Rebuild

# Check if it works:
Ctrl+a 3
curl localhost:8000

# See output:
Ctrl+a 4
# Real-time logs
```

### Kubernetes & Helm

```bash
# Create Kubernetes session
Ctrl+a C
k8s-dev

# Windows
Ctrl+a e                    # Edit manifests/values (1)

Ctrl+a T
kubectl                     # Window 2: kubectl commands

Ctrl+a T
helm                        # Window 3: helm operations

Ctrl+a T
pods                        # Window 4: pod logs

# Monitor cluster
Ctrl+a 2
kubectl get pods -w         # Watch pods

Ctrl+a 3
helm list -A                # List releases

# View pod logs
Ctrl+a 4
kubectl logs -f deployment/myapp

# Deploy workflow:
Ctrl+a 1                    # Edit values.yaml
Ctrl+a 3
helm upgrade myrelease ./mychart

# Verify deployment:
Ctrl+a 2                    # See pods
Ctrl+a 4                    # View logs
```

---

## Context Switching

### Multiple Projects in One Session

```bash
# Start with project A
Ctrl+a C
project-a

Ctrl+a e
nvim .                      # Edit in project A

# Work for 30 minutes, then need to switch
Ctrl+a d                    # Detach (project A keeps running)

# Now start project B
Ctrl+a C
project-b

Ctrl+a e
nvim .                      # Edit in project B

# Later, back to project A
Ctrl+a S                    # Fuzzy finder
project-a                   # Select

# Everything exactly as you left it!
```

### Quick Task Interleaving

```bash
# In session "myapp"
Ctrl+a 1                    # Working in editor

# Quick 5-minute task in a different project
Ctrl+a d                    # Detach

# Create temp session
Ctrl+a C
quick-task

Ctrl+a e
vim important_file.txt      # Edit quickly

# Back to main project
Ctrl+a d
Ctrl+a S
myapp                       # Resume where you were

# Continue editing (myapp window 1 still open)
```

### Comparison: Two Branches Side-by-Side

```bash
# Session for comparison
Ctrl+a C
compare

Ctrl+a 1 |                  # Split editor
Ctrl+a h
git checkout main           # Left pane
nvim src/                   # View main branch code

Ctrl+a l
git checkout feature        # Right pane
nvim src/                   # View feature branch code

# Compare implementations side-by-side!
Ctrl+a h                    # Switch between panes to see diffs
Ctrl+a l
```

---

## Collaborative Development

### Pair Programming

```bash
# Create shared session (with tmux sharing)
Ctrl+a C
pair-session

# Both developers attach:
tmux attach -t pair-session

# Same view, synchronized
# Both can type in panes
# Both see each other's actions

# Edit together
Ctrl+a 1
# Both editing same file

# Run commands
Ctrl+a T
npm test                    # Both see output

# Tip: Use read-only mode for one developer
tmux attach -t pair-session -r    # Read-only
```

### Code Review Setup

**Reviewer sees code side-by-side with tests**

```bash
# Create review session
Ctrl+a C
review-pr-42

Ctrl+a 1 |                  # Split editor
Ctrl+a h
git checkout main           # Left: original code

Ctrl+a l
git checkout pr-42          # Right: new code

# Run tests
Ctrl+a T
npm test                    # Window 2: run tests

# As you review:
Ctrl+a h                    # See original
Ctrl+a l                    # See changes
Ctrl+a 2                    # Check test status
```

---

## Advanced Patterns

### Async Task Management

```bash
# Create session with task windows
Ctrl+a C
tasks

# Dedicated windows for async operations
Ctrl+a T
build-watch                 # Window 2: builds running

Ctrl+a T
tests-watch                 # Window 3: tests auto-running

Ctrl+a T
linter-watch                # Window 4: linter auto-running

# Start watchers
Ctrl+a 2
npm run build:watch

Ctrl+a 3
npm run test:watch

Ctrl+a 4
npm run lint:watch

# Edit freely in window 1
Ctrl+a 1
nvim .

# Builds, tests, and linting all run automatically
# Check status by switching to their windows
Ctrl+a 2                    # See build errors
Ctrl+a 3                    # See test failures
Ctrl+a 4                    # See lint warnings
```

### Nested Sessions

```bash
# Main session for project
Ctrl+a C
project-main

# Switch to different server and create nested tmux
Ctrl+a T
remote

Ctrl+a (in new window)
ssh user@remote-server
tmux new-session -s remote-work

# Now in nested tmux
# Use different prefix (or be explicit with: C-b instead of C-a)
# Local: Ctrl+a for outer tmux
# Remote: Ctrl+b for inner tmux

# To send command to inner tmux:
Ctrl+a C-a                  # Send Ctrl+a to inner tmux
# Then: c (creates window in remote tmux)
```

---

## Tips for Effective Workflows

### 1. Name Windows Meaningfully

```bash
# Good:
Ctrl+a T
api-server

# Less helpful:
Ctrl+a T
term1
```

### 2. Use Snapshots for Recurring Setups

```bash
# Create the setup once
Ctrl+a M-l                  # Snapshot current session

# Later, restore it
~/.config/tmux/bin/session-manager.sh restore webapp
# Full setup restored instantly
```

### 3. Session Persistence

```bash
# Never close a tab for a project
Ctrl+a d                    # Detach instead

# Your session keeps running
# All processes continue
# You can attach hours/days later
```

### 4. Create Window Aliases

Add to your shell profile:

```bash
# Create shortcuts
alias dev="tmux new-window -n dev -c $(pwd)"
alias log="tmux new-window -n log -c $(pwd)"
alias test="tmux new-window -n test -c $(pwd)"

# Then in tmux:
dev                         # Creates dev window in current dir
test                        # Creates test window
```

### 5. Use Zooming for Focused Work

```bash
# When you need to focus on one pane
Ctrl+a z                    # Zoom (maximize)

# Do focused work
# When done:
Ctrl+a z                    # Restore layout
```

---

## Summary

The key to effective workflows is:

1. **Leverage sessions** for project isolation
2. **Use windows** to organize concerns (edit, build, logs)
3. **Split panes** for comparison or quick commands
4. **Detach, don't close** to keep everything running
5. **Name clearly** for easy navigation

See [KEYBINDINGS.md](KEYBINDINGS.md) for the complete reference of all available commands.
