# Implementation Patterns

Konkretne wzorce jak implementować każdy typ ulepszenia w SKILL.md i references.

---

## Pattern: Adding New Features

### Where to Add

**Option A: Inline w existing workflow**
```markdown
## Workflow

1. [Existing step]
2. [Existing step]
3. **[NEW FEATURE]** (Optional)
   - [Feature description]
   - When to use: [criteria]
   - How to use: [steps]
4. [Continue existing workflow]
```

**Option B: Separate optional section**
```markdown
## Core Workflow
[Existing workflow...]

## Advanced Features (NEW)

### Feature Name
**When to use:** [scenario]
**How it works:** [description]
**Steps:**
1. [Step 1]
2. [Step 2]
```

### Example Integration

```markdown
Before:
## Workflow
1. Extract text from PDF
2. Format output
3. Save result

After:
## Workflow
1. Extract text from PDF
2. **Form Filling (NEW)** (Optional)
   - If PDF has fillable forms
   - Provide key-value pairs
   - Forms will be filled automatically
3. Format output
4. Save result

## Form Filling Details
See [references/form-filling.md](references/form-filling.md) for supported field types and examples.
```

### Checklist

- [ ] Feature clearly marked as NEW
- [ ] When to use documented
- [ ] Doesn't break existing workflow
- [ ] Examples added
- [ ] Optional vs required明确

---

## Pattern: Simplifying Workflow

### Approach

1. **Identify kombinable steps:**
   - Steps that naturally belong together
   - Same logical phase
   - Share inputs/outputs

2. **Merge without losing clarity:**
   ```markdown
   Before:
   1. Read file
   2. Validate file format
   3. Parse content
   4. Validate parsed data

   After:
   1. Read and validate file (format check + structure validation)
   2. Parse content with validation
   ```

3. **Maintain checkpoints:**
   ```markdown
   After:
   1. Read and validate file
      ✓ File format correct
      ✓ File structure valid
   2. Parse content
      ✓ Content parsed
      ✓ Data validated
   ```

### Warning Signs

❌ **Don't oversimplify:**
- If combining makes instructions unclear
- If error handling becomes complex
- If steps serve different audiences

✅ **Good simplification:**
- Fewer steps, same clarity
- Related operations grouped
- Logical flow maintained

---

## Pattern: Parallel Execution

### Implementation

```markdown
## Workflow

1. [Preparation step]

2. **Process in parallel:**

   Execute simultaneously:
   - **Task A:** [Operation A]
   - **Task B:** [Operation B]
   - **Task C:** [Operation C]

   All tasks use output from Step 1.

3. **Merge results:**
   - Collect outputs from A, B, C
   - Combine into unified result
   - Resolve any conflicts
```

### For File Processing

```markdown
2. **For each file in list, process in parallel:**

   a. Read file
   b. Analyze content
   c. Generate report

   Run multiple files simultaneously using parallel tool calls.

3. **Aggregate:**
   - Combine individual reports
   - Generate summary statistics
```

### Key Points

- Clearly mark what executes in parallel
- Document how to merge results
- Note any ordering requirements
- Explain conflict resolution

---

## Pattern: Adding Checkpoints

### File Structure

```markdown
## Workflow

1. [Step 1]
   **Checkpoint:** Saves to `.tasks/skill-name-checkpoint-1.json`

2. [Step 2]
   **Checkpoint:** Saves to `.tasks/skill-name-checkpoint-2.json`

3. [Step 3]
   **Checkpoint:** Saves to `.tasks/skill-name-checkpoint-3.json`

## Resume from Checkpoint

**At workflow start:**
1. Check for checkpoint files
2. If found: Load most recent and resume from that step
3. If not found: Start from beginning

**Clear checkpoints:**
On successful completion, delete checkpoint files.
```

### Checkpoint Data Structure

```json
{
  "skill": "skill-name",
  "checkpoint": 2,
  "timestamp": "2024-01-15T10:30:00Z",
  "state": {
    "filesProcessed": ["file1.py", "file2.py"],
    "results": {...},
    "nextStep": "Step 3"
  }
}
```

---

## Pattern: Better Edge Cases

### Structure

```markdown
## Edge Cases

### Empty Input
**Detection:** File size = 0 or no data provided
**Handling:**
- Warn user: "Input is empty"
- Ask: "Continue with empty result or cancel?"
- If continue: Generate minimal valid output

### Very Large Input (>100MB)
**Detection:** File size check before processing
**Handling:**
- Warn: "Large file detected. Processing may take time."
- Offer batch processing option
- Show progress during processing

### Malformed Data
**Detection:** Parse error or validation failure
**Handling:**
- Show specific error: "Line 45: Invalid JSON"
- Suggest common fixes
- Offer: "Continue with partial data or fix and retry?"

### Permission Errors
**Detection:** File access denied
**Handling:**
- Show clear error: "Cannot read file: Permission denied"
- Suggest: "Run with elevated permissions or check file ownership"
- Don't retry automatically (won't help)
```

### Implementation

- Add Edge Cases section to SKILL.md
- For each: Detection + Handling + User options
- Integrate checks into workflow where relevant

---

## Pattern: Early Validation

### Pre-flight Checks Section

```markdown
## Phase 0: Pre-flight Checks

**Before starting main workflow, verify:**

### Prerequisites
- [ ] Required tools installed
  ```bash
  which git npm python3
  ```
- [ ] Working directory is git repository
  ```bash
  git rev-parse --git-dir
  ```

### Input Validation
- [ ] All file paths exist
- [ ] File formats correct (check extensions)
- [ ] Required fields present in config

### Environment
- [ ] Network connectivity (if API calls needed)
  ```bash
  ping -c 1 api.example.com
  ```
- [ ] Sufficient disk space (if generating files)
  ```bash
  df -h .
  ```
- [ ] API credentials configured

**If any check fails:**
- Report specific issue
- Suggest fix (e.g., "Install npm: brew install node")
- Exit before starting workflow

**If all pass:**
✅ All checks passed. Proceeding to main workflow.
```

### Integration

Add as Phase 0, before existing workflow Phase 1.

---

## Pattern: Self-Checking

### Quality Verification Phase

```markdown
## Phase N: Self-Check Quality (NEW)

**Before presenting to user, verify output quality:**

### Automated Checks

**Completeness:**
```python
# Example pseudo-code
checks = [
  all_functions_documented(),
  all_params_explained(),
  return_types_specified()
]
```

**Clarity:**
- No jargon without definitions
- Examples for complex features
- Clear section structure

**Technical:**
- Code examples syntax valid
- Links resolve correctly
- Version numbers consistent

### Check Process

1. Run all automated checks
2. **If any fail:**
   - Attempt automatic fix (if possible)
   - Document what was fixed
   - Re-run checks
3. **If can't auto-fix:**
   - Report issues to user
   - Ask for guidance
4. **Only present after all checks pass**

### Verification Log

Save to `.tasks/skill-name-verification.md`:
- What was checked
- What issues found
- What was auto-fixed
- What needs user attention
```

---

## Pattern: Error Recovery

### Recovery Strategy Section

```markdown
## Error Recovery (NEW)

### Failure Classification

**Transient (retry-able):**
- Network timeouts
- Rate limiting
- Temporary resource unavailability

**Non-transient (permanent):**
- Validation errors
- Permission denied
- Invalid configuration

### Recovery Actions

**For transient failures:**
```markdown
1. Detect failure type
2. Wait (exponential backoff: 1s, 2s, 4s)
3. Retry up to 3 times
4. If succeeds: Log and continue
5. If all retries fail: Move to rollback
```

**For non-transient failures:**
```markdown
1. Don't retry (won't help)
2. Save progress to checkpoint
3. Move to rollback procedure
```

### Rollback Procedure

**Track changes:**
Maintain list of all modifications in memory:
```json
[
  {"action": "created", "file": "output.txt"},
  {"action": "modified", "file": "config.json", "backup": ".config.json.bak"},
  {"action": "deleted", "file": "temp.log"}
]
```

**On rollback:**
```markdown
1. Process changes in reverse order
2. For each change:
   - created → delete file
   - modified → restore from backup
   - deleted → restore from trash
3. Verify rollback successful
4. Report what was restored
```
```

---

## Pattern: Context-Awareness

### Detection and Adaptation

```markdown
## Phase 1: Context Detection (NEW)

### Auto-detect Project Type

**Check for framework markers:**

```bash
# React project
if [ -f "package.json" ] && grep -q "react" package.json; then
  PROJECT_TYPE="react"
fi

# Python project
if [ -f "requirements.txt" ] || [ -f "pyproject.toml" ]; then
  PROJECT_TYPE="python"
fi

# Java project
if [ -f "pom.xml" ] || [ -f "build.gradle" ]; then
  PROJECT_TYPE="java"
fi
```

### Load Context-Specific Configuration

**Based on detected type:**

```markdown
**React project detected:**
- Load: [references/react-patterns.md](references/react-patterns.md)
- Use: JSX syntax, React testing patterns
- Follow: React conventions

**Python project detected:**
- Load: [references/python-patterns.md](references/python-patterns.md)
- Use: Python idioms, pytest patterns
- Follow: PEP 8

**Java project detected:**
- Load: [references/java-patterns.md](references/java-patterns.md)
- Use: Java conventions, JUnit patterns
- Follow: Java best practices
```

### Adapt Behavior

Document how behavior differs per context in appropriate reference file.
```

---

## Pattern: Interactive Choices

### Convert Questions to Menus

**Before (open text):**
```markdown
Ask user: "What testing framework do you want to use?"
```

**After (choice menu):**
```markdown
Use AskUserQuestion tool:

questions: [
  {
    question: "Which testing framework should we use?",
    header: "Framework",
    multiSelect: false,
    options: [
      {
        label: "Jest (Recommended)",
        description: "Best for React and Node.js projects. Fast, built-in mocking."
      },
      {
        label: "Pytest",
        description: "Python testing framework. Simple syntax, powerful fixtures."
      },
      {
        label: "JUnit 5",
        description: "Standard for Java. Annotations-based, extensive ecosystem."
      }
    ]
  }
]
```

### Multiple Related Questions

```markdown
Use AskUserQuestion with multiple questions:

questions: [
  {
    question: "What API style fits your needs?",
    header: "API Style",
    multiSelect: false,
    options: [...]
  },
  {
    question: "Which authentication methods do you need?",
    header: "Auth",
    multiSelect: true,  // Can select multiple
    options: [...]
  }
]
```

### When to Use

- Questions have enumerable options (2-4 choices)
- Options are clear and distinct
- Users benefit from seeing all options
- Descriptions help decision-making

---

## Pattern: Progress Visibility

### Add Progress Tracking

```markdown
## Workflow

### Phase 1: Preparation
[1/5] ⏳ Analyzing project structure...

### Phase 2: Processing
[2/5] ⏳ Processing files (23/150 complete, 15%)...

### Phase 3: Validation
[3/5] ⏸️  Pending (after processing)

### Phase 4: Generation
[4/5] ⏸️  Pending (after validation)

### Phase 5: Finalization
[5/5] ⏸️  Pending (after generation)
```

### For Long Operations

```markdown
**During file processing:**

"Processing security analysis...
├─ File 23/150 (15% complete)
└─ Current: src/utils/auth.py"

**Status icons:**
- ✅ Complete
- ⏳ In progress
- ⏸️  Pending
- ❌ Failed
```

### Implementation

- Update status before each phase
- Show sub-progress for long steps
- Show percentage or items completed
- Clear current operation

---

## Pattern: Dry-Run Mode

### Add Mode Selection

```markdown
## Execution Modes

### Preview Mode (Recommended for first use)

**Workflow:**
1. Analyze and plan all changes
2. Generate diffs for each file:
   ```diff
   --- a/file.py
   +++ b/file.py
   @@ -10,3 +10,4 @@
    def function():
   -    old_code()
   +    new_code()
   +    additional_line()
   ```
3. Show summary:
   - X files will be modified
   - Y files will be created
   - Z files will be deleted
4. Ask user: "Apply these changes?"
   - **Apply** → Execute all changes
   - **Cancel** → No changes made
   - **Modify** → Adjust and preview again

### Direct Mode

**Workflow:**
1. Analyze and execute immediately
2. Apply all changes
3. Report what was changed
4. Log saved to `.tasks/skill-name-changes.md`

### Default Behavior

- First-time users → Preview Mode
- Returning users with trust → Direct Mode
- Can override with explicit flag
```

---

## Pattern: Better Reports

### Report Structure Template

```markdown
# [Skill Name] Report

Generated: [timestamp]

## 🎯 Executive Summary

- **Key metric 1:** [value]
- **Key metric 2:** [value]
- **Critical items:** [count] require immediate attention
- **Overall status:** [status]

## 🔴 Critical Issues (Action Required)

### 1. [Issue Title]

**Location:** [file:line]
**Impact:** [what happens if not fixed]
**Priority:** Critical
**Effort:** [Low/Medium/High]

**Problem:**
[Clear description of issue]

**Solution:**
[Concrete fix]

**Example:**
```[language]
# Before
[problematic code]

# After
[fixed code]
```

---

### 2. [Next critical issue]

---

## 🟡 High Priority

[Grouped by category]

### Security Issues (3)
[List with links to details]

### Performance Issues (5)
[List with links to details]

## 🟢 Low Priority

<details>
<summary>View 27 low priority items</summary>

[Collapsed content]

</details>

## 📊 Metrics & Statistics

| Metric | Value | Trend |
|--------|-------|-------|
| Code quality score | 7.5/10 | ↑ |
| Security score | 6/10 | → |
| Performance score | 8/10 | ↓ |

## 📁 Files Analyzed

- Total: 45 files
- Modified: 12 files
- Created: 3 files
- Deleted: 1 file

## ⏱️ Processing Stats

- Analysis time: 2.3s
- Files per second: 19.6
- Total issues found: 45

## 🔄 Next Steps

Recommended actions in priority order:
1. [ ] Fix 3 critical security issues
2. [ ] Review 5 high-priority performance issues
3. [ ] Consider low-priority improvements
```

### Key Principles

- **Start with summary** - busy users can stop after first section
- **Prioritize ruthlessly** - critical first, low-priority collapsed
- **Make actionable** - concrete fixes, not just complaints
- **Add context** - metrics, trends, comparisons
- **Group intelligently** - by category, severity, file

---

## Pattern: Batch Processing

### Add Batch Mode

```markdown
## Processing Modes

### Single Item Mode

Process one file/item:
```bash
/skill-name path/to/file.py
```

### Batch Mode (NEW)

Process multiple items efficiently:

**Input:**
```bash
/skill-name --batch "*.py"
# or
/skill-name --batch file1.py file2.py file3.py
```

**Workflow:**

1. **Discovery:**
   ```bash
   # Find all matching items
   find . -name "*.py" -type f
   ```

2. **Shared Setup (once for all items):**
   - Load configuration
   - Initialize tools
   - Prepare context
   - Set up output directory

3. **Process Items:**
   - **Sequential:** When order matters or resources limited
   - **Parallel:** When items independent (faster)
   ```markdown
   Process items in parallel:
   - Item 1
   - Item 2
   - Item 3
   [All using shared setup]
   ```

4. **Aggregate Results:**
   ```markdown
   ## Individual Results
   - item1.py: ✅ 3 issues found
   - item2.py: ✅ 5 issues found
   - item3.py: ❌ Failed (parse error)

   ## Aggregate Analysis
   - Total items: 45
   - Successful: 44
   - Failed: 1
   - Total issues: 127
   - Most common issue: [type]
   - Worst file: item2.py (12 issues)
   ```

**Benefits:**
- 10x faster (shared setup)
- Consistent context
- Aggregate insights
- Single report
```

---

## Pattern: Adding Examples

### Example Section Structure

```markdown
## Examples

### Example 1: Basic Usage (Happy Path)

**Scenario:** [Common use case]

**Input:**
```[language]
[Concrete input]
```

**Process:**
1. [What skill does - step by step]
2. [Next step]
3. [Final step]

**Output:**
```[language]
[Exact expected output]
```

---

### Example 2: Edge Case - [Specific Case]

**Scenario:** [When this happens]

**Input:**
```[language]
[Edge case input]
```

**How skill handles it:**
1. [Detection]
2. [Special handling]
3. [Result]

**Output:**
```[language]
[Output for edge case]
```

---

### Example 3: Integration - [With Other Tool/Skill]

**Context:** [When you'd use this]

**Combined workflow:**
1. Use [other-skill] to [action]
2. Use this skill to [action]
3. Result: [combined output]

**Why this combination:**
[Explanation of value]

---

### Example 4: Common Mistake

**❌ Wrong approach:**
```[language]
[What not to do]
```

**Why it fails:**
[Explanation]

**✅ Correct approach:**
```[language]
[What to do instead]
```

**Result:**
[Success output]
```

### Where to Put Examples

- **2-3 examples in SKILL.md** - Most common cases
- **Extended examples in references/examples.md** - Edge cases, integrations, anti-patterns

---

## Pattern: Best Practices

### Best Practices Section

```markdown
## Best Practices

### When to Use This Skill

✅ **Recommended for:**
- **[Scenario 1]:** Why it's a good fit
  - Benefit A
  - Benefit B

- **[Scenario 2]:** Specific advantages
  - Works well when [condition]
  - Particularly effective for [use case]

❌ **Not recommended for:**
- **[Scenario A]:** Use [alternative] instead
  - Reason: [explanation]

- **[Scenario B]:** Manual approach better
  - Reason: [explanation]

### Optimization Tips

**For large projects:**
- Enable batch processing mode
- Use checkpoints for resumability
- Consider parallel execution

**For frequent use:**
- Let skill learn from history
- Configure smart defaults
- Save common configurations

**For team collaboration:**
- Use machine-readable outputs
- Document decisions in reports
- Share configuration files

### Common Mistakes & Fixes

#### Mistake 1: [Common Error]

**Symptom:** [What users experience]
**Why it's wrong:** [Explanation]
**Fix:** [Correct approach]
**Example:**
```[language]
# Wrong
[bad code]

# Correct
[good code]
```

#### Mistake 2: [Another Common Error]
[Same structure]

### Performance Tips

- **Tip 1:** [How to make it faster]
- **Tip 2:** [How to reduce resource usage]
- **Tip 3:** [How to improve quality]
```

---

## Implementation Checklist

Dla każdego upgrade:

- [ ] **Integration:** Clearly marked where added (NEW tag)
- [ ] **Documentation:** Explained when/why to use
- [ ] **Examples:** Concrete demonstrations added
- [ ] **Workflow:** Doesn't break existing flow
- [ ] **References:** Detailed docs in references/ if needed
- [ ] **Testing:** Verified implementation works
- [ ] **User value:** Clear benefit delivered

---

## Common Mistakes to Avoid

❌ **Don't:**
- Break existing functionality
- Add features without clear use case
- Over-complicate simple workflows
- Forget to update examples
- Leave TODO comments in final version

✅ **Do:**
- Test upgrade thoroughly
- Keep changes atomic
- Update all relevant sections
- Add concrete examples
- Document reasoning in implementation log
