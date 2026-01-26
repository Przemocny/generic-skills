# Common Mistakes When Creating Skills

Detailed guide to avoiding common pitfalls. Based on real anti-patterns from Agent Skills research.

---

## 🔴 Critical Mistakes (Will Break Skill)

### 1. The Encyclopedia

**Mistake:** Putting all documentation in monolithic SKILL.md

**Symptom:**
- SKILL.md is 2000+ lines
- Contains complete API reference
- Has 50+ examples inline
- Includes all edge cases
- Everything in one file

**Why it's bad:**
- Wastes massive context window tokens
- Overwhelming for agent to parse
- Loads unnecessary information every time
- Makes skill slow and expensive

**Example:**
```
skill-name/
└── SKILL.md (5000 lines)
    ├── Complete API docs
    ├── All examples
    ├── All edge cases
    ├── Troubleshooting guide
    └── Everything else
```

**How to fix:**
```
skill-name/
├── SKILL.md (300 lines)
│   ├── Overview
│   ├── Core workflow
│   └── "See references/ for details"
└── references/
    ├── api-reference.md (loaded when needed)
    ├── examples.md (loaded when needed)
    └── troubleshooting.md (loaded when needed)
```

**Refactoring steps:**
1. Identify sections with most detail (API docs, examples, edge cases)
2. Create focused reference files
3. Move detailed content there
4. In SKILL.md, replace with brief overview + link
5. Verify SKILL.md under 500 lines

---

### 2. The Everything Bagel

**Mistake:** Skill tries to apply to every single task

**Symptom:**
```yaml
description: General development best practices that apply to all coding tasks
```

**Why it's bad:**
- If skill applies everywhere, it should be a RULE (in AGENTS.md)
- Agent loads it for every task (wastes tokens)
- Too generic to be helpful
- Lacks focus

**Example bad:**
```yaml
name: best-practices
description: Best practices for software development
```

**How to fix:**
- If applies everywhere → Move to AGENTS.md or .claude/rules.md as always-on context
- If applies to specific domain → Focus skill on that domain
- If multiple domains → Split into focused skills

**Example good:**
```yaml
name: python-best-practices
description: Python coding standards and conventions for our codebase. Use when writing or reviewing Python code.

name: api-design-patterns
description: REST API design patterns and conventions. Use when creating or modifying API endpoints.
```

**Rule:** One skill = one domain. If it's a rule for everything, it's not a skill.

---

### 3. The Secret Handshake

**Mistake:** Agent never loads the skill because description is bad

**Symptom:**
- You know skill exists
- Skill does useful things
- But Claude never uses it
- Must manually invoke with `/skill-name`

**Why it happens:**
```yaml
# Bad description - too abstract
description: Helper utilities

# Bad description - no keywords
description: Performs various operations

# Bad description - wrong keywords
description: PDF tool
# (But users say "extract text from document" not "PDF tool")
```

**How to fix:**
1. Think: How do users ACTUALLY talk about this task?
2. Include those EXACT phrases in description
3. Add specific keywords users naturally say
4. Explain WHEN to use, not just WHAT it does

**Before:**
```yaml
description: Helps with PDFs
```

**After:**
```yaml
description: >
  Extracts text and tables from PDF files, fills PDF forms, and merges
  multiple PDFs. Use when working with PDF documents or when the user
  mentions PDFs, forms, or document extraction. Trigger on phrases like
  "extract PDF text", "fill PDF form", "merge PDFs", "get text from document".
```

**Test:** Say task naturally to Claude - does skill load? If not, fix description.

---

### 4. The Fragile Skill

**Mistake:** Hardcoded absolute paths that break on other machines

**Symptom:**
```markdown
Read config from /Users/john/projects/myapp/config/settings.json
Run tests: cd /Users/john/projects/myapp && npm test
Use script: python /Users/john/scripts/process.py
```

**Why it's bad:**
- Breaks on anyone else's machine
- Breaks when directory moves
- Not portable
- Team can't use it

**How to fix:**

**Bad:**
```bash
/Users/john/projects/app/config.json
```

**Good:**
```bash
# Relative path
config/config.json

# Or with variable
${baseDir}/config/config.json

# Or with environment variable
$PROJECT_ROOT/config/config.json
```

**For scripts:**
```bash
# Bad
python /Users/john/scripts/process.py

# Good
python scripts/process.py

# Or
python ${SKILL_DIR}/scripts/process.py
```

**Rule:** No hardcoded absolute paths. Use relative paths or environment variables.

---

### 5. Time Estimates Anywhere

**Mistake:** Including ANY time predictions

**Symptom:**
```markdown
This analysis should take about 5-10 minutes
Quick scan (2-3 minutes)
Phase 1 (15-45 minutes)
```

**Why it's FORBIDDEN:**
- Core guideline violation
- Users should judge timing themselves
- Predictions are always wrong
- Creates false expectations

**How to fix:** Remove ALL time references

**Before:**
```markdown
Step 1: Quick validation (2 minutes)
Step 2: Deep analysis (15-30 minutes)
Step 3: Report generation (5 minutes)
```

**After:**
```markdown
Step 1: Validation
Step 2: Deep analysis
Step 3: Report generation
```

**Rule:** NEVER include time estimates. Zero tolerance.

---

## 🟡 Quality Mistakes (Reduce Effectiveness)

### 6. The Wall of Text

**Mistake:** Unstructured prose instead of clear instructions

**Symptom:**
```markdown
So basically what you want to do is kind of look at the user's request
and figure out what they're trying to achieve, and then you know, use
your best judgment to help them out with their code review needs and
maybe check for issues and suggest some fixes if you think they would
be helpful and also look at performance and security...
```

**Why it's bad:**
- LLMs follow structured instructions better
- Hard to parse
- Easy to miss important points
- Unclear what to do

**How to fix:**

**Before (prose):**
```markdown
When reviewing code you should check for security issues like SQL injection
and also performance problems and make sure the code follows our style guide
and then report any issues you find with suggestions for fixes.
```

**After (structured):**
```markdown
## Code Review Process

1. Read changed files
2. Check for:
   - Security issues (SQL injection, XSS, hardcoded secrets)
   - Performance problems (N+1 queries, inefficient loops)
   - Style violations (naming, formatting)
3. For each issue found:
   - Quote problematic code
   - Explain the problem
   - Suggest a fix
4. Generate summary with severity levels
```

**Rule:** Structure > Prose. Use headers, numbered steps, bullet points.

---

### 7. Teaching What LLM Already Knows

**Mistake:** Explaining basic concepts LLM already understands

**Symptom:**
```markdown
Python is a programming language. Functions are defined using the def
keyword. Variables can store data. Loops allow you to repeat code...
```

**Why it's bad:**
- Wastes tokens
- No value added
- Makes skill longer for no reason

**What to include instead:**
- Company-specific conventions
- Proprietary schemas
- Custom business logic
- Internal tool usage
- Domain expertise LLM doesn't have

**Before (waste):**
```markdown
## Python Basics

Functions use def keyword. Classes use class keyword. For loops iterate...
```

**After (valuable):**
```markdown
## Company Python Conventions

- All API endpoints must use `/api/v2/` prefix
- Error responses must include `errorCode` field from ERROR_CODES.md
- Authentication uses custom JWT claims: `orgId`, `roleLevel`, `permissions`
```

**Rule:** Only add what LLM doesn't already know.

---

### 8. Missing Examples

**Mistake:** Abstract descriptions without concrete examples

**Symptom:**
- Skill describes what to do
- But no examples showing how
- No input → output patterns
- No good vs bad comparisons

**Why it's bad:**
- Hard to understand in practice
- Easy to misinterpret
- Less actionable

**How to fix:**

**Before (no examples):**
```markdown
Generate commit messages following our standards.
```

**After (with examples):**
```markdown
Generate commit messages following our standards.

## Examples

**Good:**
```
feat(auth): add JWT token refresh

Implement automatic token refresh when access token expires.
```

**Bad:**
```
fixed stuff  ❌ (too vague)
Added new features for authentication system  ❌ (no type)
```
```

**Rule:** Include concrete examples showing input → output.

---

### 9. Redundant Content

**Mistake:** Same information repeated in multiple places

**Symptom:**
- Same questions in SKILL.md AND references/questions.md
- Same examples everywhere
- Duplicate guidelines

**Why it's bad:**
- Wastes tokens
- Maintenance nightmare
- Confusion about source of truth

**How to fix:**

**Before (redundant):**
```
SKILL.md:
  Questions: Q1, Q2, Q3, Q4, Q5 (full text)

references/questions.md:
  Questions: Q1, Q2, Q3, Q4, Q5 (same full text)
```

**After (single source):**
```
SKILL.md:
  "Ask core questions. See references/questions.md for complete library."

references/questions.md:
  Questions: Q1, Q2, Q3, Q4, Q5 (full text with examples)
```

**Rule:** Single source of truth. SKILL.md = pointers, references/ = details.

---

### 10. Overwhelming Options

**Mistake:** Too many choices without guidance

**Symptom:**
- 15 questions in one phase
- Every option equally important
- No defaults
- No prioritization

**Why it's bad:**
- Decision paralysis
- Overwhelming for user
- Hard to know where to start

**How to fix:**

**Before (overwhelming):**
```markdown
Choose from these 15 options:
1. Option A
2. Option B
...
15. Option O
```

**After (prioritized):**
```markdown
**Required:**
1. Option A
2. Option B

**Optional (choose if needed):**
3. Option C (use when X)
4. Option D (use when Y)

**Start with:** Options A and B, add others only if you need them.
```

**Rule:** Prioritize (MUST vs OPTIONAL), provide defaults, guide choices.

---

### 11. Listing Every Tool

**Mistake:** `allowed-tools` contains all possible tools

**Symptom:**
```yaml
allowed-tools: Bash Read Write Edit Glob Grep WebFetch WebSearch Task ...
```

**Why it's bad:**
- Defeats permission system
- Security risk
- No principle of least privilege

**How to fix:**

**Before (everything):**
```yaml
allowed-tools: Bash Read Write Edit Glob Grep WebFetch Task
```

**After (minimal):**
```yaml
# Read-only skill
allowed-tools: Read Grep

# Git operations only
allowed-tools: Bash(git:*) Read

# PDF processing
allowed-tools: Bash(python:scripts/process_pdf.py) Read Write
```

**Rule:** Only list tools THIS skill actually needs. Principle of least privilege.

---

### 12. No Special Cases

**Mistake:** Skill only handles happy path

**Symptom:**
- No guidance for edge cases
- No error handling
- Assumes everything works perfectly
- No "what if" scenarios

**Why it's bad:**
- Breaks in real-world usage
- No guidance when things go wrong
- Frustrating for users

**How to fix:**

Add "Special Cases" section covering:
- What if user input is vague?
- What if task is too big/small?
- What if resources aren't available?
- What if user disagrees with recommendations?
- What if external API fails?

**Example:**
```markdown
## Special Cases

**If user input is vague:**
- Ask clarifying questions
- Provide examples to help narrow scope

**If task is too large:**
- Suggest breaking into smaller skills
- Focus on most important part first

**If external API fails:**
- Retry with exponential backoff
- Fall back to manual instructions if still fails
- Report error clearly to user
```

**Rule:** Handle edge cases, don't just happy path.

---

## 🟢 Polish Mistakes (Missing Excellence)

### 13. Poor Formatting

**Mistake:** No visual hierarchy, everything looks the same

**Why it matters:**
- Hard to scan
- Important points not emphasized
- Cognitive load higher

**How to improve:**
- Use **bold** for emphasis
- Use headers for sections
- Use code blocks for commands/code
- Use tables for comparisons
- Use emojis for visual markers (✅ ❌ ⚠️ 💡)

---

### 14. No Cross-References

**Mistake:** Skill exists in isolation, doesn't reference related skills

**Why it matters:**
- Missed opportunities for skill chaining
- User doesn't know what to do next

**How to improve:**
```markdown
## Related Skills

After creating skill, consider:
- `/skill-validator` - Validate skill quality
- `/skill-publisher` - Publish skill to team

Before using this skill:
- `/skill-planner` - Plan skill architecture
```

---

### 15. Inconsistent Tone

**Mistake:** Mixing formal/informal, Polish/English randomly

**Why it matters:**
- Feels unprofessional
- Confusing for users

**How to improve:**
- Choose tone and stick to it
- If mixing languages, have clear pattern (e.g., questions in Polish, technical terms in English)
- Be consistent with "you" vs "user" vs "agent"

---

## Anti-Pattern Summary Table

| Anti-Pattern | Symptom | Fix | Priority |
|--------------|---------|-----|----------|
| Encyclopedia | 2000+ line SKILL.md | Move to references/ | 🔴 Critical |
| Everything Bagel | Applies to all tasks | Make it a rule, not skill | 🔴 Critical |
| Secret Handshake | Agent never loads | Fix description keywords | 🔴 Critical |
| Fragile Skill | Hardcoded paths | Use relative paths | 🔴 Critical |
| Time Estimates | Any time predictions | Remove all estimates | 🔴 Critical |
| Wall of Text | Unstructured prose | Add structure (headers, lists) | 🟡 Quality |
| Teaching Basics | Explaining Python 101 | Only add domain knowledge | 🟡 Quality |
| Missing Examples | No concrete examples | Add input → output examples | 🟡 Quality |
| Redundancy | Repeated information | Single source of truth | 🟡 Quality |
| Overwhelming | Too many options | Prioritize, provide defaults | 🟡 Quality |
| Listing All Tools | Every tool allowed | Minimal tool set | 🟡 Quality |
| No Edge Cases | Happy path only | Add special cases section | 🟡 Quality |
| Poor Formatting | No visual hierarchy | Use bold, headers, emojis | 🟢 Polish |
| No Cross-References | Isolated skill | Link to related skills | 🟢 Polish |
| Inconsistent Tone | Random style mixing | Choose and stick to tone | 🟢 Polish |

---

## Before/After Examples

### Example 1: The Encyclopedia → Progressive Disclosure

**Before (Encyclopedia):**
```
skill-name/
└── SKILL.md (3000 lines)
    ├── Overview (50 lines)
    ├── Core workflow (100 lines)
    ├── Complete API reference (1200 lines)
    ├── 50 examples (800 lines)
    ├── All edge cases (500 lines)
    └── Troubleshooting (350 lines)
```

**After (Progressive Disclosure):**
```
skill-name/
├── SKILL.md (350 lines)
│   ├── Overview
│   ├── Core workflow
│   ├── Quick examples (3-5 key examples)
│   └── "See references/ for details"
└── references/
    ├── api-reference.md (1200 lines, loaded when needed)
    ├── examples.md (800 lines, loaded when needed)
    ├── edge-cases.md (500 lines, loaded when needed)
    └── troubleshooting.md (350 lines, loaded when needed)
```

**Impact:** SKILL.md tokens: 3000 → 350 (88% reduction)

---

### Example 2: Wall of Text → Structure

**Before:**
```markdown
When you want to review code you should check for security issues
like SQL injection and XSS and also check for performance problems
like N+1 queries and inefficient loops and also make sure the code
follows our style guide and then report any issues with suggestions...
```

**After:**
```markdown
## Code Review Process

1. **Analyze files**
   - Read changed files
   - Identify patterns

2. **Check for issues:**
   - Security: SQL injection, XSS, hardcoded secrets
   - Performance: N+1 queries, inefficient loops
   - Style: Naming, formatting per style guide

3. **Report findings:**
   - Quote problematic code
   - Explain issue
   - Suggest fix with example

4. **Generate summary**
   - Group by severity (Critical/Warning/Info)
   - Count issues by category
```

---

### Example 3: Bad Description → Good Description

**Before:**
```yaml
description: PDF helper tool
```

**After:**
```yaml
description: >
  Extracts text and tables from PDF files, fills PDF forms, merges
  multiple PDFs, and rotates pages. Use when working with PDF documents
  or when user mentions PDFs, documents, forms, or extraction. Trigger
  on phrases like "extract PDF text", "fill PDF form", "merge PDFs",
  "rotate PDF pages", "get text from document".
```

**Impact:** Agent now recognizes skill when user says natural phrases.

---

## Refactoring Workflow

When you find these mistakes:

1. **Identify** the anti-pattern
2. **Assess** severity (Critical/Quality/Polish)
3. **Plan** the fix (what to change, where to move)
4. **Implement** systematically (one fix at a time)
5. **Verify** quality improved (run checklist)

**Don't try to fix everything at once** - prioritize Critical first, then Quality, then Polish.

---

## Prevention Checklist

Before finalizing a new skill, check:

- [ ] Description has keywords users naturally say
- [ ] SKILL.md under 500 lines
- [ ] Details in references/, not SKILL.md
- [ ] Structured format (headers, lists, steps)
- [ ] Concrete examples included
- [ ] No time estimates anywhere
- [ ] No hardcoded paths
- [ ] No redundancy
- [ ] Special cases covered
- [ ] Related skills cross-referenced

**Prevention is easier than fixing!**
