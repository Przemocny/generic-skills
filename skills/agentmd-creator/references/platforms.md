# Platform-Specific Guidelines

## Cursor

### File Formats

**Legacy:** `.cursorrules` (deprecated, but still supported)
**Modern:** `.cursor/rules/*.mdc` (recommended)

### .mdc Format Structure

```markdown
---
name: "rule-name"
description: "When to apply this rule"
globs: ["src/**/*.tsx", "src/**/*.jsx"]
alwaysApply: false
version: "1.0"
---

# Rule Content
[Markdown instructions]
```

### Organization Strategy

```
.cursor/
├── index.mdc              # Always-apply core rules
├── rules/
│   ├── 001-core.mdc       # Core standards (001-099)
│   ├── 100-api.mdc        # Integration rules (100-199)
│   └── 200-patterns.mdc   # Pattern rules (200-299)
```

### Key Features
- **Modular**: Multiple small files < 50 lines each
- **Glob patterns**: Apply rules to specific file patterns only
- **AlwaysApply**: Control when rule activates
- **Metadata**: YAML frontmatter with configuration

### Best Practices
- Keep each .mdc file under 50 lines
- Use glob patterns to target specific files
- Number files by category (001-core, 100-integration, 200-patterns)

---

## Claude Code

### File Formats

**Primary:** `CLAUDE.md`
**Universal:** `AGENTS.md`
**Subagents:** `.claude/agents/*.md`

### File Priority Order

1. `.claude/CLAUDE.md`
2. `./CLAUDE.md` (root)
3. `~/.claude/CLAUDE.md` (user-level)
4. `CLAUDE.md.local` (personal, gitignored)

### CLAUDE.md Structure

```markdown
# Project: [Name]
[One-line description]

## Tech Stack
- Framework X version Y
- Library A version B

## Commands
test: npm test
build: npm run build
lint: eslint . --fix

## Architecture
- /app - Application code
- /lib - Utilities

## Important Notes
- Auth in /lib/auth
- Never modify /generated

## Boundaries
### Always Do
- Run tests after changes

### Ask First
- Database schema changes

### Never Do
- Commit secrets
- Use `any` type
```

### Nested CLAUDE.md Strategy

```
/CLAUDE.md                  # Core (30 lines)
/src/components/CLAUDE.md   # Component-specific
/tests/CLAUDE.md           # Testing-specific
```

### Subagents (.claude/agents/*.md)

```markdown
---
name: code-reviewer
description: Expert code review. Use after code changes.
tools: Read, Grep, Glob, Bash
disallowedTools: Write, Edit
model: sonnet
---

# Code Reviewer
[Instructions for subagent]
```

### Best Practices
- Keep CLAUDE.md < 300 lines (ideally < 60)
- Use references (file:line) instead of code copies
- Create subagents for specialized tasks
- Nested CLAUDE.md for directory-specific rules

---

## Windsurf

### File Formats

**Modern:** `.windsurf/rules/*.md`
**Global:** `global_rules.md` (via IDE settings)

### Limits

- Single file: 6000 characters max
- Total (global + local): 12,000 characters max

### Structure

```markdown
# Windsurf Project Rules

## Technology Stack
- Framework with versions

## Code Conventions
- Specific patterns

## Testing Requirements
- What tests are needed

## Boundaries
**Never:**
- Destructive actions
```

### Best Practices
- Keep files under 6000 characters
- Split into multiple files if needed
- Clear boundaries section

---

## Antigravity (Google Gemini)

### File Formats

**Workspace:** `.antigravity/rules.md`
**Global:** `~/.gemini/GEMINI.md`
**Workflows:** `.antigravity/workflows/`

### Structure

```markdown
# Antigravity Directives

## Role Definition
You are [specific role]

## Core Behaviors
1. Mission-First: Read mission.md
2. Plan Alignment: Confirm before action

## Coding Standards
- Type Hints: ALL Python code
- Pydantic models for data

## Capability Scopes
✅ Allowed: [actions]
❌ Forbidden: [actions]
```

### Workflows

Workflows are saved prompts activated with `/command`

---

## JetBrains Junie

### File Formats

**Primary:** `.junie/guidelines.md`
**Universal:** `AGENTS.md`

### Structure

```markdown
# [Framework] Guidelines

## Preferred Coding Styles
- Specific patterns for framework

## Best Practices
- Framework-specific practices

## Common Anti-patterns to Avoid
- What NOT to do

## Real-world Examples
[Code examples]
```

### Best Practices
- Framework-specific guidelines
- Include anti-patterns
- Real code examples

---

## GitHub Copilot

### File Format

**Only:** `.github/copilot-instructions.md`

### Requirements

Must enable: `github.copilot.chat.codeGeneration.useInstructionFiles`

### Structure

```markdown
# GitHub Copilot Instructions

## Code Style
Brief, self-contained statements.

## Testing
Testing approach and coverage.

## Documentation
Documentation requirements.
```

### Best Practices
- Short, self-contained statements
- Natural language
- One statement per line
- Auto-attached to all Copilot requests

---

## Universal: AGENTS.md

### Status

Emerging industry standard (Agentic AI Foundation / Linux Foundation)

### Supported Platforms

- Claude Code
- OpenAI Codex
- Roo Code
- GitHub Copilot (roadmap)
- Cursor (planned)
- Windsurf (planned)

### Structure

```markdown
# AGENTS.md

## Dev Environment
```bash
# Commands with full flags
npm run dev -- --port 3001
```

## Testing
```bash
# Test commands
npm test -- --coverage
```

## PR Instructions
- Title format
- Checklist before merge

## Boundaries
**Always:**
- Required actions

**Ask First:**
- Major changes

**Never:**
- Forbidden actions
```

### Best Practices
- < 300 lines total
- Commands with full flags
- Real examples over explanations
- Explicit boundaries (3-tier)

---

## Platform Selection Guide

| Platform | Primary File | Use When |
|----------|-------------|----------|
| **Cursor** | `.cursor/rules/*.mdc` | Need modular, file-pattern-specific rules |
| **Claude Code** | `CLAUDE.md` + subagents | Need specialized agents or nested context |
| **Windsurf** | `.windsurf/rules/*.md` | Using Windsurf IDE |
| **Antigravity** | `.antigravity/rules.md` | Using Google Gemini agent |
| **Junie** | `.junie/guidelines.md` | Using JetBrains IDEs |
| **GitHub Copilot** | `.github/copilot-instructions.md` | Using GitHub Copilot |
| **Multi-platform** | `AGENTS.md` | Want universal compatibility |

### Recommendation Strategy

1. **Single IDE project**: Use platform-specific format
2. **Team with mixed IDEs**: Start with AGENTS.md, add platform-specific as needed
3. **New project**: AGENTS.md for universality
4. **Existing project**: Keep current format, consider AGENTS.md for compatibility
