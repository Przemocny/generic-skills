# Advanced Features for Skills

Optional advanced features available when creating skills. Use these when basic skill structure isn't enough.

---

## Hooks (Claude Code Only)

Hooks enable lifecycle automation - scripts that run at specific points during skill execution.

### What are Hooks?

Hooks are shell commands executed automatically in response to events like tool calls, prompts, or session lifecycle.

**Available hook events:**
- `PreToolUse` - Before tool execution (can block/modify)
- `PostToolUse` - After tool succeeds (can validate)
- `PostToolUseFailure` - After tool fails (error handling)
- `Stop` - Claude finishes responding (continuation logic)
- `UserPromptSubmit` - User submits prompt (can add context/validate)
- `SessionStart` - Session starts/resumes (setup)
- `SessionEnd` - Session terminates (cleanup)

### Hook Configuration in Frontmatter

**Basic hook:**
```yaml
---
name: skill-name
description: Skill description
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/validate.sh"
          timeout: 30
---
```

### Use Cases

#### 1. Validation Hook

**Validate inputs before execution:**

```yaml
hooks:
  PreToolUse:
    - matcher: "Bash(git commit*)"
      hooks:
        - type: command
          command: "./scripts/validate-commit-msg.sh"
```

**validate-commit-msg.sh:**
```bash
#!/bin/bash

# Read hook input (JSON on stdin)
input=$(cat)
message=$(echo "$input" | jq -r '.tool_input.command')

# Validate conventional commit format
if ! echo "$message" | grep -qE '^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .+'; then
    echo "Commit message doesn't follow conventional commits format" >&2
    exit 2  # Exit 2 blocks the tool call
fi

exit 0  # Exit 0 allows tool call
```

**Why:** Ensures commit messages follow standards before allowing commit.

---

#### 2. Post-Execution Validation

**Validate output quality:**

```yaml
hooks:
  PostToolUse:
    - matcher: "Edit|Write"
      hooks:
        - type: command
          command: "./scripts/run-linter.sh"
```

**run-linter.sh:**
```bash
#!/bin/bash

# Read hook input
input=$(cat)
tool_name=$(echo "$input" | jq -r '.tool_name')

# Run linter on modified files
if ! npm run lint; then
    echo "Linting failed - fix issues before continuing" >&2
    exit 2  # Blocks continuation
fi

exit 0
```

**Why:** Ensures code quality standards maintained after edits.

---

#### 3. Automatic Testing

**Run tests after code changes:**

```yaml
hooks:
  PostToolUse:
    - matcher: "Edit.*\\.py$|Write.*\\.py$"
      hooks:
        - type: command
          command: "./scripts/run-python-tests.sh"
```

**Why:** Catch regressions immediately after code changes.

---

#### 4. Security Checks

**Block dangerous operations:**

```yaml
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/security-check.sh"
```

**security-check.sh:**
```bash
#!/bin/bash

input=$(cat)
command=$(echo "$input" | jq -r '.tool_input.command')

# Block dangerous commands
if echo "$command" | grep -qE "rm -rf /|sudo|format"; then
    echo "Dangerous command blocked: $command" >&2
    exit 2
fi

exit 0
```

**Why:** Prevents accidental dangerous operations.

---

### Hook Input Format

Hooks receive JSON on stdin with information about the event.

**Common fields:**
```json
{
  "session_id": "abc123",
  "cwd": "/current/working/directory",
  "hook_event_name": "PreToolUse"
}
```

**PreToolUse specific:**
```json
{
  "tool_name": "Bash",
  "tool_input": {
    "command": "git commit -m 'message'",
    "description": "Commit changes"
  },
  "tool_use_id": "toolu_01ABC"
}
```

**PostToolUse specific:**
```json
{
  "tool_name": "Write",
  "tool_input": { ... },
  "tool_output": "File written successfully",
  "duration_ms": 123
}
```

### Hook Exit Codes

**Exit 0:** Success
- Stdout shown to user (verbose mode)
- Tool execution proceeds (PreToolUse) / continues (PostToolUse)

**Exit 2:** Blocking error
- Stderr shown to Claude as error
- Blocks tool call (PreToolUse)
- Blocks continuation (PostToolUse)
- Agent receives error and must handle

**Exit 1+:** Non-blocking error
- Stderr shown in verbose mode
- Doesn't block execution

### Hook Output (JSON)

For advanced control, return JSON on stdout (with exit 0):

```json
{
  "continue": false,
  "stopReason": "Must fix failing tests before proceeding",
  "hookSpecificOutput": {
    "hookEventName": "PostToolUse",
    "decision": "block",
    "reason": "Tests failed",
    "additionalContext": "3 tests failed: test_auth, test_login, test_signup"
  }
}
```

---

## Subagent Context (Claude Code Only)

Run skill in isolated subagent with specific tool permissions.

### What is Subagent Context?

Subagent = independent agent spawned to execute task in isolation.

**Benefits:**
- Isolated execution (doesn't affect main session)
- Specific tool permissions (read-only, limited tools)
- Task-focused (receives skill content as its prompt)
- Parallelizable (multiple subagents simultaneously)

### Configuration

```yaml
---
name: skill-name
description: Skill description
context: fork
agent: Explore
allowed-tools: Read Grep Glob
---

Your task: [The skill body becomes the subagent's prompt]

$ARGUMENTS will be replaced with args passed when invoking skill.
```

### Subagent Types

**Explore:**
- Read-only exploration
- Good for: research, code analysis, finding information
- Tools: Read, Grep, Glob, WebFetch

**Plan:**
- Planning and design
- Good for: architecture design, implementation planning
- Tools: Read, Grep, Glob, (no Write/Edit)

**general-purpose:**
- Full capabilities
- Good for: complete tasks, complex operations
- Tools: All tools (unless restricted)

### Use Case: Research Skill

```yaml
---
name: codebase-research
description: Research codebase for patterns and usage
context: fork
agent: Explore
allowed-tools: Read Grep Glob
---

# Codebase Research

Research $ARGUMENTS thoroughly:

1. Find relevant files using Glob and Grep
2. Read and analyze the code
3. Cross-reference between related components
4. Summarize findings with specific file:line references

Focus on understanding complete picture, not individual pieces.
```

**Usage:**
```
/codebase-research authentication system
```

**What happens:**
1. Skill activates with `context: fork`
2. New Explore subagent spawns (isolated)
3. Subagent receives skill body as prompt with "$ARGUMENTS" → "authentication system"
4. Subagent explores with read-only tools
5. Results summarized back to main session

**Why useful:**
- Main session not cluttered with exploration
- Read-only = safe (can't accidentally modify)
- Focused agent = better results

---

## Dynamic Context Injection (Claude Code Only)

Pre-process commands before sending to agent.

### What is Dynamic Context?

Execute commands BEFORE skill loads, inject results into skill content.

**Syntax:** `` !`command` ``

### Example: PR Summary

```yaml
---
name: pr-summary
description: Summarize GitHub pull request
context: fork
agent: Explore
---

# Pull Request Summary

## Context (pre-fetched)

**PR Diff:**
!`gh pr diff`

**PR Description:**
!`gh pr view --json body -q .body`

**Comments:**
!`gh pr view --comments`

## Your Task

Summarize this PR:
1. What changed
2. Why (from description/comments)
3. Potential concerns
```

**What happens:**
1. Before sending to agent, commands execute:
   - `gh pr diff` runs → output captured
   - `gh pr view --json body -q .body` runs → output captured
   - `gh pr view --comments` runs → output captured
2. Placeholders replaced with actual output
3. Agent receives fully-rendered prompt with real data

**Why useful:**
- Fresh data every invocation
- No stale information
- Agent gets complete context immediately

---

## String Substitutions

Variables replaced in skill content and hooks.

### Available Variables

**In skill content:**
- `$ARGUMENTS` - Args passed when invoking skill
- `${CLAUDE_SESSION_ID}` - Current session ID

**In hooks (as environment variables):**
- `$CLAUDE_PROJECT_DIR` - Project root path
- `${CLAUDE_PLUGIN_ROOT}` - Plugin directory (if plugin skill)

### Example Usage

```yaml
---
name: fix-issue
description: Fix GitHub issue by number
---

# Fix Issue $ARGUMENTS

1. Read issue: `gh issue view $ARGUMENTS`
2. Understand requirements
3. Implement fix
4. Create commit referencing #$ARGUMENTS
```

**Usage:**
```
/fix-issue 123
```

**Agent sees:**
```
# Fix Issue 123

1. Read issue: `gh issue view 123`
...
4. Create commit referencing #123
```

---

## Allowed Tools (Pre-Approval)

Pre-approve specific tools to skip permission prompts.

### Configuration

```yaml
---
name: skill-name
description: Skill description
allowed-tools:
  - Read
  - Grep
  - "Bash(git:*)"
  - "Bash(npm:test)"
---
```

### Tool Patterns

**Exact match:**
```yaml
allowed-tools: [Read, Write, Edit]
```

**Pattern match:**
```yaml
allowed-tools: ["Bash(git:*)"]  # Any git command
```

**Specific commands:**
```yaml
allowed-tools: ["Bash(npm:test)", "Bash(npm:lint)"]  # Only these npm commands
```

### Use Cases

#### Read-Only Skill

```yaml
allowed-tools: [Read, Grep, Glob]
```

**Why:** Research/analysis skill that should never modify files.

#### Git Operations Skill

```yaml
allowed-tools: ["Bash(git:*)", Read]
```

**Why:** Git workflow skill, only needs git commands + file reading.

#### Specific Script Execution

```yaml
allowed-tools: ["Bash(python:scripts/validate.py)", Read, Edit]
```

**Why:** Only allow specific script, prevent arbitrary command execution.

### Security Benefit

Pre-approved tools = no permission prompts = smoother experience while maintaining security boundaries.

**Balance:** Approve only what skill actually needs (principle of least privilege).

---

## Model Override (Claude Code Only)

Use different model for specific skill.

### Configuration

```yaml
---
name: quick-skill
description: Quick operation
model: haiku
---
```

### Available Models

- `haiku` - Fast, cheap, good for simple tasks
- `sonnet` - Balanced (default)
- `opus` - Most capable, expensive

### When to Use

**Use haiku for:**
- Simple deterministic operations
- Quick lookups
- Straightforward transformations
- Cost-sensitive skills

**Use opus for:**
- Complex reasoning
- Difficult problems
- Critical accuracy
- Advanced capabilities needed

**Most skills:** Use default (sonnet) - no model override needed.

---

## Disable Auto-Invocation

Make skill manual-only (not automatically triggered).

### Configuration

```yaml
---
name: deploy-production
description: Deploy to production (MANUAL USE ONLY)
disable-model-invocation: true
---
```

### When to Use

**Manual-only skills for:**
- Dangerous operations (deploy, delete, format)
- Explicit intent required (user must type `/deploy`)
- Side-effect heavy operations
- User wants control over timing

**Example:** Deployment skill should NEVER auto-trigger, always require explicit `/deploy` command.

---

## User Invocable Flag

Hide skill from `/` command menu (agent can still load).

### Configuration

```yaml
---
name: internal-helper
description: Internal helper skill
user-invocable: false
---
```

### When to Use

**Hide from menu when:**
- Internal helper skill (used by other skills)
- Automatically triggered only (never manual)
- Debugging skill (not for regular use)

**Note:** Agent can still load if description matches, just hidden from user's `/` menu.

---

## Combining Features

Skills can use multiple advanced features together.

### Example: Secure Deployment Skill

```yaml
---
name: deploy
description: Deploy to production with safety checks
disable-model-invocation: true  # Manual only
model: sonnet  # Need good reasoning
allowed-tools: ["Bash(git:*)", "Bash(npm:*)", Read]
hooks:
  PreToolUse:
    - matcher: "Bash(git push*)"
      hooks:
        - type: command
          command: "./scripts/pre-deploy-check.sh"
  PostToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/verify-deployment.sh"
---

# Deploy to Production

**MANUAL USE ONLY** - Type `/deploy` to execute

## Safety Checks

Pre-deployment hooks verify:
- All tests pass
- No uncommitted changes
- On main branch
- Version bumped

Post-deployment hooks verify:
- Deployment succeeded
- Health checks pass
- No errors in logs

## Workflow

[Deployment steps...]
```

**Why this works:**
- `disable-model-invocation: true` - Never auto-triggers
- Hooks ensure safety at every step
- `allowed-tools` limits what can be executed
- `model: sonnet` - Balanced reasoning for deployment decisions

---

## When to Use Advanced Features

**Use hooks when:**
- Need validation/checks before/after operations
- Want automatic testing
- Security checks required
- Quality gates needed

**Use subagent context when:**
- Task should be isolated
- Want read-only safety
- Parallel execution beneficial
- Task is self-contained

**Use dynamic context when:**
- Need fresh external data
- Working with APIs/services
- Data changes frequently

**Use allowed-tools when:**
- Clear tool requirements
- Want to skip permission prompts
- Security boundaries important

**Use model override when:**
- Task clearly simple (haiku) or complex (opus)
- Cost optimization critical
- Specific capability needed

**Use disable-model-invocation when:**
- Operation is dangerous
- User must explicitly confirm
- Side effects significant

**Most skills don't need these** - basic structure (SKILL.md + optional scripts/references/assets) is sufficient for 90% of cases.

**Use advanced features only when they solve specific problem.**

---

## Testing Advanced Features

**Test hooks:**
```bash
# Trigger hook manually
echo '{"tool_name":"Bash","tool_input":{"command":"git commit"}}' | ./scripts/hook.sh
```

**Test subagent context:**
- Invoke skill, verify subagent spawns
- Check subagent has correct tools only
- Verify isolation (main session not affected)

**Test dynamic context:**
- Invoke skill, verify commands execute
- Check output injected correctly
- Verify fresh data each time

**Test allowed-tools:**
- Invoke skill, verify no permission prompts for allowed tools
- Try disallowed tool, verify permission prompt appears

---

## Documentation

**If using advanced features, document in SKILL.md:**

```markdown
## Advanced Features

This skill uses:
- **Hooks:** Pre-commit validation (scripts/validate-commit.sh)
- **Allowed tools:** Pre-approved git commands
- **Manual invocation:** Use `/deploy` - never auto-triggers

See references/advanced-features.md for details.
```

**Why:** Users should understand skill capabilities and limitations.

---

## Resources

- **Hooks documentation:** https://code.claude.com/docs/en/hooks
- **Skills specification:** https://agentskills.io/specification
- **Claude Code docs:** https://code.claude.com/docs/en/skills
