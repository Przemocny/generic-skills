# Best Practices for Agent Skills

Universal principles for creating effective, maintainable agent skills.

## Core Principles

### 1. Concise is Key

**Context window is a public good** - Skills share it with system prompt, conversation history, other skills, and user request.

**Default assumption:** Agent is already very capable.

**Rule:** Only add context the agent doesn't already have.

**Challenge each section:**
- "Does the agent really need this explanation?"
- "Does this paragraph justify its token cost?"

**Examples:**

❌ **Waste of tokens:**
```markdown
Python is a programming language. Functions are defined with `def`.
Variables store data. Loops repeat code...
```

✅ **Efficient:**
```markdown
## Company-Specific Conventions

- All API endpoints must use `/api/v2/` prefix
- Error responses must include `errorCode` from ERROR_CODES.md
- Auth uses JWT claims: `orgId`, `roleLevel`, `permissions`
```

### 2. Progressive Disclosure

**3-level loading system:**

1. **Metadata** (name + description) - Always in context (~100 words)
2. **SKILL.md body** - When skill triggers (<5k words, ideally <500 lines)
3. **Supporting files** - As needed by agent (unlimited, scripts can execute without loading)

**Keep SKILL.md under 500 lines:**
- Overview + core workflow
- Links to references/
- Essential procedural instructions

**Move to references/:**
- Detailed API documentation
- Extensive examples
- Edge cases and troubleshooting
- Domain-specific details

**Pattern:**
```
SKILL.md (300 lines)
├── Overview
├── Core workflow
└── "For complete API reference, see references/api.md"

references/api.md (loaded only when needed)
├── All endpoints
├── Request/response schemas
└── Error codes
```

### 3. Description Field is King

**Most important field** - Agent uses it to decide when to load skill.

❌ **Poor:**
```yaml
description: Helps with PDFs.
```

✅ **Good:**
```yaml
description: Extracts text and tables from PDF files, fills PDF forms, and merges multiple PDFs. Use when working with PDF documents or when the user mentions PDFs, forms, or document extraction.
```

**Guidelines:**
1. Describe WHAT it does (concrete capabilities)
2. Describe WHEN to use (triggering contexts)
3. Include KEYWORDS users naturally say
4. Use 1-1024 characters wisely
5. Think semantic matching (language understanding, not regex)

### 4. Structure Over Prose

**LLMs follow structured instructions better than text walls.**

❌ **Bad:**
```markdown
So basically what you want to do is look at the user's request and
figure out what they're trying to achieve, and then use your best
judgment to help them with their code review needs...
```

✅ **Good:**
```markdown
## Code Review Process

1. Read the changed files
2. Check for:
   - Security vulnerabilities (SQL injection, XSS, secrets)
   - Performance issues (N+1 queries, inefficient loops)
   - Code style violations
3. For each issue:
   - Quote problematic code
   - Explain the problem
   - Suggest a fix
4. Generate summary with severity levels
```

**Use:**
- Numbered steps for procedures
- Bullet points for criteria/checklists
- Headers for sections
- Examples showing input → output
- Explicit conditionals ("If X, then Y")

### 5. One Skill = One Domain

**Scope appropriately.**

❌ **Too broad:**
```yaml
name: developer-tools
description: Helps with development tasks
```

✅ **Focused:**
```yaml
name: git-helper
description: Git workflow automation - commit messages, branch management, conflict resolution

name: code-review
description: Review code for security, performance, and style issues

name: api-tester
description: Test API endpoints using curl/httpie, validate responses
```

### 6. Include Concrete Examples

**Examples > abstract descriptions**

```markdown
## Example Input
/skill-name analyze path/to/file.py

## Example Output

**For security issue:**
```
🔴 CRITICAL: SQL Injection vulnerability
File: app/queries.py:45
Code: cursor.execute(f"SELECT * FROM users WHERE id = {user_id}")
Fix: Use parameterized query: cursor.execute("SELECT * FROM users WHERE id = ?", (user_id,))
```

**For performance issue:**
```
🟡 WARNING: N+1 query detected
File: app/views.py:23
Problem: Loading related objects in loop (100 queries for 100 items)
Fix: Use .select_related() or .prefetch_related()
```
```

### 7. Portable and Relative Paths

**Never hardcode absolute paths.**

❌ **Fragile:**
```markdown
Read config from /Users/john/projects/myapp/config/settings.json
Run tests: cd /Users/john/projects/myapp && npm test
```

✅ **Portable:**
```markdown
Read config from config/settings.json (relative to project root)
Run tests: npm test (uses project package.json)
Use ${baseDir} for absolute paths when needed
```

### 8. Scripts vs Instructions

**When to use scripts:**

✅ **Create script when:**
- Same code repeatedly rewritten
- Deterministic reliability needed
- Operation is fragile/error-prone
- Output is more important than code details

✅ **Use instructions when:**
- Agent can easily write the code
- Flexibility needed for different contexts
- Code varies significantly per use case

**Script efficiency:**
- Script CODE never loads to context
- Only script OUTPUT consumes tokens
- Self-contained scripts save tokens

### 9. Reference Supporting Files Clearly

**In SKILL.md, describe what files contain:**

```markdown
## Additional Resources

- **references/api-reference.md** - Complete REST API documentation with all endpoints
- **references/examples.md** - 20+ usage examples for common scenarios
- **references/troubleshooting.md** - Solutions for common errors and edge cases

Load these files only when you need specific information.
```

**Benefits:**
- Agent knows files exist
- Agent knows when to load them
- Files not loaded consume zero tokens

### 10. Split by Domain/Framework

**For skills supporting multiple domains or frameworks, organize by variant:**

```
bigquery-skill/
├── SKILL.md (overview and navigation)
└── references/
    ├── finance.md (revenue, billing metrics)
    ├── sales.md (opportunities, pipeline)
    ├── product.md (API usage, features)
    └── marketing.md (campaigns, attribution)
```

**When user asks about sales metrics, agent only reads sales.md.**

Similarly for multi-framework skills:

```
cloud-deploy/
├── SKILL.md (workflow + provider selection)
└── references/
    ├── aws.md (AWS deployment patterns)
    ├── gcp.md (GCP deployment patterns)
    └── azure.md (Azure deployment patterns)
```

## Anti-Patterns to Avoid

### The Encyclopedia

**Problem:** Monolithic SKILL.md with all documentation

**Symptom:** 5000+ line SKILL.md file

**Fix:** Split into SKILL.md (overview) + references/ (details)

### The Everything Bagel

**Problem:** Skill applies to every task

**Symptom:**
```yaml
description: General development best practices for all coding tasks
```

**Fix:** If it applies everywhere, it's a RULE (in AGENTS.md or .claude/rules.md), not a skill

### The Secret Handshake

**Problem:** Agent never loads the skill

**Symptom:** User knows skill exists, but Claude never uses it

**Fix:** Rewrite description to match how users actually talk about tasks

### The Fragile Skill

**Problem:** Breaks every time repo structure changes

**Symptom:** Hardcoded absolute paths

**Fix:** Use relative paths and environment variables

### The Wall of Text

**Problem:** Unstructured prose

**Fix:** Structure with headers, lists, numbered steps

### Listing Every Tool

**Problem:** `allowed-tools` contains all tools

**Symptom:**
```yaml
allowed-tools: Bash Read Write Edit Glob Grep WebFetch WebSearch Task ...
```

**Security issue:** Defeats permission system

**Fix:** Only list tools actually needed for THIS skill
```yaml
allowed-tools: Read Grep  # Read-only skill
allowed-tools: Bash(git:*) Read  # Git operations only
```

### Documentation Decay

**Problem:** Skill quality degrades over time

**Prevention:**
- Regular reviews
- Delete outdated sections
- Split growing skills
- Version control + changelog

## Quality Checklist

Before finalizing a skill:

**Description:**
- [ ] Contains keywords users naturally say
- [ ] Explains WHEN to use, not just WHAT
- [ ] 1-1024 characters, uses space wisely
- [ ] Includes trigger phrases

**SKILL.md:**
- [ ] Under 500 lines
- [ ] Structured (headers, lists, steps)
- [ ] Includes concrete examples
- [ ] Only adds what LLM doesn't know
- [ ] Uses imperative form

**Supporting Files:**
- [ ] Scripts tested and working
- [ ] References focused (one topic per file)
- [ ] Assets documented in SKILL.md
- [ ] Clear guidance on when to load references

**Portability:**
- [ ] No hardcoded absolute paths
- [ ] Works across different user environments
- [ ] Dependencies documented

**Progressive Disclosure:**
- [ ] SKILL.md = overview + core workflow
- [ ] Detailed content in references/
- [ ] Clear links with "when to load" guidance

**Security:**
- [ ] `allowed-tools` minimal (only needed tools)
- [ ] Scripts validate inputs
- [ ] No sensitive information hardcoded

## Writing Style Guidelines

**Use imperative/infinitive form:**
- ✅ "Run tests"
- ✅ "Check for errors"
- ❌ "Running tests"
- ❌ "The agent should check for errors"

**Be specific:**
- ✅ "Use Salesforce API v52.0, credentials in 1Password: Sales CRM"
- ❌ "Use CRM"

**Show, don't tell:**
- ✅ Include code examples
- ❌ Describe what code should do

**Focus on "why" and "when":**
- ✅ "Use parameterized queries to prevent SQL injection"
- ❌ "Always use parameterized queries"

## Testing Your Skill

**Test triggering:**
```bash
# Try natural phrasing
"I need to [task description]"  # Should trigger skill

# Check skill is registered
/skills

# Test directly
/skill-name [args]
```

**Test with edge cases:**
- Normal inputs
- Empty inputs
- Very long inputs
- Special characters
- Path traversal attempts

**Test progressive disclosure:**
- Does agent load references only when needed?
- Are scripts executed without loading code to context?

## Maintenance

**Regular reviews:**
- Remove outdated sections
- Update examples
- Split if growing too large
- Test with current agent version

**Version control:**
- Track changes
- Document breaking changes
- Maintain changelog for team skills

**User feedback:**
- Monitor when skill triggers incorrectly
- Listen for requests about missing features
- Iterate based on real usage
