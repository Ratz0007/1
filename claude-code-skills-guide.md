# Claude Code - Complete Skills Guide & Best Practices

> A comprehensive reference for maximizing Claude Code capabilities

---

## Table of Contents
1. [What is Claude Code](#what-is-claude-code)
2. [Core Capabilities](#core-capabilities)
3. [Top Skills & Commands](#top-skills--commands)
4. [CLAUDE.md - Project Memory](#claudemd---project-memory)
5. [Prompt Engineering for Claude Code](#prompt-engineering-for-claude-code)
6. [Agentic & Multi-Step Tasks](#agentic--multi-step-tasks)
7. [Tool Use Mastery](#tool-use-mastery)
8. [Best Practices & Tips](#best-practices--tips)
9. [Common Use Cases](#common-use-cases)
10. [Dos and Don'ts](#dos-and-donts)

---

## What is Claude Code

Claude Code is Anthropic's agentic coding tool that runs in the terminal and understands your entire codebase. It can edit files, run commands, write and execute code, search the web, manage git, and complete complex multi-step engineering tasks autonomously.

---

## Core Capabilities

| Capability | Description |
|---|---|
| File Read/Write | Read, edit, create, and delete files across your project |
| Terminal Commands | Run bash, shell scripts, build tools, test runners |
| Code Search | Search codebase with grep, ripgrep, find |
| Git Operations | Commit, branch, diff, merge, push, pull |
| Web Search | Search the internet for docs and solutions |
| Multi-file Editing | Refactor across many files simultaneously |
| Test Execution | Run tests and fix failures automatically |
| Dependency Management | Install packages, update configs |

---

## Top Skills & Commands

### 1. Starting Claude Code
```bash
# Install
npm install -g @anthropic-ai/claude-code

# Start in your project directory
cd your-project
claude

# Start with a task immediately
claude "Fix all TypeScript errors in this project"

# Start with a file as context
claude --file context.md "Refactor based on these instructions"
```

### 2. Key CLI Flags
```bash
claude --print          # Non-interactive, print response and exit (great for scripts)
claude --output-format json   # JSON output for automation
claude --verbose        # Show detailed tool usage
claude --no-auto-approve     # Ask before each action (safer mode)
claude --dangerously-skip-permissions  # Auto-approve all (use with caution)
```

### 3. Slash Commands (Inside Claude Code)
```
/help                   # Show all available commands
/clear                  # Clear conversation context
/compact                # Compress context to save tokens
/config                 # View and edit configuration
/cost                   # Show token usage and cost so far
/doctor                 # Check installation health
/init                   # Create CLAUDE.md for your project
/login                  # Switch Anthropic account
/logout                 # Log out
/memory                 # Edit global memory file
/model                  # Switch AI model
/permissions            # View or update tool permissions
/pr_comments            # View pull request comments
/review                 # Request code review
/terminal-setup         # Install shell integration (Shift+Enter for newlines)
/vim                    # Toggle vim keybindings
```

### 4. Keyboard Shortcuts
```
Enter               # Send message
Shift+Enter         # New line in message (after /terminal-setup)
Up Arrow            # Previous message in history
Ctrl+C              # Cancel current operation
Ctrl+L              # Clear terminal screen
```

---

## CLAUDE.md - Project Memory

This is the MOST IMPORTANT file for getting best results. Claude Code reads `CLAUDE.md` automatically at the start of every session.

### What to put in CLAUDE.md
```markdown
# Project Name

## Project Overview
Brief description of what this project does.

## Tech Stack
- Language: Python 3.11 / Node.js 20 / etc.
- Framework: FastAPI / Next.js / etc.
- Database: PostgreSQL / MongoDB / etc.
- Key Libraries: list them here

## Project Structure
```
src/
  api/          # API route handlers
  models/       # Data models
  services/     # Business logic
tests/          # Test files
```

## Development Commands
```bash
npm install     # Install dependencies
npm run dev     # Start dev server
npm test        # Run tests
npm run build   # Build for production
```

## Code Style & Conventions
- Use TypeScript strict mode
- Follow ESLint config
- Write tests for all new functions
- Use conventional commits

## Important Notes
- Never commit .env files
- API keys stored in environment variables
- Database migrations must be run after schema changes

## Common Pitfalls
- Always run migrations before testing
- Use `npm ci` not `npm install` in CI
```

### CLAUDE.md Locations
```
~/CLAUDE.md              # Global memory (applies to all projects)
./CLAUDE.md              # Project root (committed to repo)
./.claude/CLAUDE.md      # Sub-directory specific instructions
```

---

## Prompt Engineering for Claude Code

### Be Specific and Goal-Oriented
```
BAD:  "Fix the bug"
GOOD: "Fix the null pointer exception in src/api/users.ts line 47 
       that occurs when user.profile is undefined"

BAD:  "Improve performance"
GOOD: "The /api/products endpoint takes 3 seconds. Profile it, 
       identify bottlenecks, and optimize to under 500ms"
```

### Provide Context Upfront
```
"I'm building a multi-tenant SaaS app with Next.js 14 and Prisma.
The auth is handled by NextAuth. Now add a billing module using 
Stripe that supports monthly and annual subscriptions."
```

### Use Checkpoints for Long Tasks
```
"Step 1: Analyze the existing database schema and list all tables
Step 2: Propose a migration plan for adding multi-tenancy
Step 3: Wait for my approval before making any changes"
```

### Reference Specific Files
```
"Look at src/models/user.ts and src/api/auth.ts, then add 
JWT refresh token support matching the existing code style."
```

---

## Agentic & Multi-Step Tasks

### How to Trigger Agentic Mode
Simply give Claude Code a high-level goal. It will break it into steps and execute:

```
"Set up a complete REST API for a todo app with:
- Express.js + TypeScript
- PostgreSQL with Prisma ORM
- JWT authentication
- CRUD operations for todos
- Unit tests with Jest
- Dockerized with docker-compose"
```

### Headless/Automated Mode (CI/CD)
```bash
# Run in automated pipelines
claude --print --output-format json "Run tests and fix any failures"

# Use as a git hook
claude --print "Review the staged changes for security issues"
```

### Sub-Agents & Parallel Work
Claude Code can spawn sub-agents for parallel tasks:
```
"Simultaneously:
1. Write unit tests for all functions in src/utils/
2. Add JSDoc comments to all exported functions
3. Update README with new API documentation"
```

---

## Tool Use Mastery

### File Operations
```
# Reading
"Read and summarize all files in the /src/api directory"

# Editing
"In all .tsx files, replace className='btn' with className='button'"

# Creating
"Create a complete MVC structure for a blog module"
```

### Terminal & Shell
```
"Run the test suite, capture the failures, fix each one, 
then run again until all tests pass"

"Check if port 3000 is in use, if so kill the process, 
then start the dev server"
```

### Git Integration
```
"Review the diff between main and this branch,
write a meaningful commit message, and commit"

"Look at the last 10 commits and generate a CHANGELOG entry"

"Find which commit introduced the bug where users can't login"
```

### Web Search
```
"Search for the latest Next.js 15 breaking changes 
and update our codebase accordingly"

"Find the official Stripe webhook setup docs 
and implement it in our Express server"
```

---

## Best Practices & Tips

### 1. Use Git Before Every Session
```bash
git commit -am "checkpoint before claude session"
# If something goes wrong: git checkout .
```

### 2. Start with /init
Run `/init` in a new project to let Claude Code analyze your codebase and auto-generate a `CLAUDE.md`.

### 3. Keep Context Focused
- Use `/clear` when switching to a different task
- Use `/compact` when context gets too large
- Break large tasks into focused sessions

### 4. Review Before Committing
- Use `--no-auto-approve` for sensitive codebases
- Review diffs before every commit
- Use `/review` to get a code review before merging

### 5. Iterative Refinement
```
First pass:  "Implement basic user authentication"
Second pass: "Add input validation and error handling"
Third pass:  "Write comprehensive tests"
Fourth pass: "Add security hardening (rate limiting, CSRF)"
```

### 6. Leverage Custom Commands
Create reusable prompt templates in `.claude/commands/`:
```markdown
# .claude/commands/review.md
Review the current branch changes for:
- Security vulnerabilities
- Performance issues
- Code style violations
- Missing error handling
- Missing tests
Provide a structured report.
```
Use it with: `/project:review`

### 7. Model Selection
```
/model claude-opus-4-5   # Best for complex reasoning & architecture
/model claude-sonnet-4-5 # Best balance of speed and quality (default)
/model claude-haiku-3-5  # Fastest, best for simple/repetitive tasks
```

---

## Common Use Cases

### Bug Fixing
```
"The login fails with a 401 error when using special characters 
in the password. Find the root cause and fix it."
```

### Code Refactoring
```
"Refactor the UserService class to follow SOLID principles. 
Break it into smaller, single-responsibility classes."
```

### Adding Features
```
"Add dark mode support to the entire app using CSS variables 
and a toggle in the header. Persist preference in localStorage."
```

### Writing Tests
```
"Analyze the untested functions and write comprehensive unit tests 
with edge cases. Aim for 90%+ coverage."
```

### Documentation
```
"Generate complete API documentation in OpenAPI 3.0 format 
based on all Express route handlers."
```

### Security Audits
```
"Audit the entire codebase for OWASP Top 10 vulnerabilities. 
List findings with severity and fix each one."
```

### Database Work
```
"Analyze current query performance, add appropriate indexes, 
and rewrite N+1 queries using proper JOINs or eager loading."
```

---

## Dos and Don'ts

### DO:
- Always have a `CLAUDE.md` with project context
- Commit to git before long autonomous sessions
- Give high-level goals, not micro-instructions
- Use `/compact` to save tokens on long sessions
- Specify the expected output format clearly
- Test incrementally on complex tasks
- Use headless mode (`--print`) for CI/CD pipelines
- Set up shell integration for multi-line prompts

### DON'T:
- Don't give vague instructions like "make it better"
- Don't skip reviewing changes in sensitive projects
- Don't run with `--dangerously-skip-permissions` on production code
- Don't use for tasks that need human judgment (business decisions)
- Don't forget to `/clear` context between unrelated tasks
- Don't let context grow too large - it reduces quality

---

## Quick Reference Card

```
COMMAND                          PURPOSE
-------                          -------
claude                           Start interactive session
claude "task"                    Start with immediate task
claude --print "task"            Non-interactive output
/init                            Generate CLAUDE.md
/clear                           Reset conversation
/compact                         Compress context
/cost                            Check token usage
/model                           Switch models
/memory                          Edit global memory
Ctrl+C                           Stop current task
git checkout .                   Undo all Claude's changes
```

---

## Resources

- Official Docs: https://docs.anthropic.com/en/docs/claude-code
- GitHub: https://github.com/anthropics/claude-code
- Changelog: https://docs.anthropic.com/en/docs/claude-code/changelog
- Community: https://discord.gg/anthropic

---

*Last updated: March 2026 | Claude Code Version: Latest*
