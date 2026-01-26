# Upgrade Examples - Before & After

Rozszerzone przykłady konkretnych ulepszeń skillów z pełnym before/after comparison.

---

## Example 1: Adding Dry-Run Mode (Type #16)

### Analyzed Skill: code-refactor

**Business Goal:** Automatically refactor code to improve quality without breaking functionality

**Current Problem:**
- Users nervous about automatic code changes
- No way to preview what will change
- Trust issues with first-time use
- Teaching opportunity missed

**Upgrade Proposal:**

**Type:** #16 Dry-run mode
**Value:** High - Addresses major user anxiety
**Effort:** Medium - Need to collect and preview changes
**Priority:** HIGH

### Implementation

**Before SKILL.md:**
```markdown
## Workflow

1. Analyze code for refactoring opportunities
2. Generate refactored code
3. Apply changes to files
4. Report what was changed
```

**After SKILL.md:**
```markdown
## Workflow

### Mode Selection (NEW)

**Preview Mode (Recommended for first use):**
1. Analyze code for refactoring opportunities
2. Generate refactored code
3. Show diffs of all proposed changes:
   ```diff
   --- a/app.py
   +++ b/app.py
   @@ -15,7 +15,8 @@
    def process_user(user_id):
   -    user = db.query(f"SELECT * FROM users WHERE id={user_id}")
   +    # Fixed: SQL injection vulnerability
   +    user = db.query("SELECT * FROM users WHERE id=?", (user_id,))
   ```
4. Summary of changes:
   - 3 files will be modified
   - 8 security issues fixed
   - 12 code smells removed
5. **Ask user:** "Apply these changes?"
   - **Apply** → Execute all changes and proceed to step 6
   - **Cancel** → No changes made
   - **Modify** → Adjust specific changes and preview again

**Direct Mode:**
1. Analyze code for refactoring opportunities
2. Generate and apply refactored code immediately
3. Report what was changed

### Default Behavior

- First-time users → Preview Mode automatically
- Can override with: `/code-refactor --direct`

6. Verify changes work (tests still pass)
7. Report completion
```

**Value Delivered:**
- ✅ Users see exactly what will change
- ✅ Can review before committing
- ✅ Trust built through transparency
- ✅ Teaching tool (shows what good refactoring looks like)
- ✅ Safety net for mistakes

---

## Example 2: Simplifying Question Flow (Type #7 + #14)

### Analyzed Skill: api-design

**Business Goal:** Design well-structured APIs following best practices

**Current Problem:**
- 8 sequential open-ended questions
- Users unsure what each question affects
- Fatigue from answering many questions
- Unclear dependencies between questions

**Upgrade Proposal:**

**Type:** #7 Simplify workflow + #14 Interactive choices
**Value:** High - Major UX improvement
**Effort:** Medium - Restructure briefing
**Priority:** HIGH

### Implementation

**Before SKILL.md:**
```markdown
## Phase 1: Requirements Gathering

Ask user:
1. What authentication method do you want to use? (open text)
2. Should we use API versioning? (yes/no)
3. If yes, what versioning scheme? (open text)
4. REST or GraphQL? (open text)
5. What HTTP status codes should we use? (open text)
6. What error response format? (open text)
7. Should we include rate limiting? (yes/no)
8. What documentation format? (open text)

After gathering answers, proceed to Phase 2...
```

**After SKILL.md:**
```markdown
## Phase 1: Requirements Gathering (IMPROVED)

### Briefing Strategy

Ask 2-3 focused questions using interactive choice menus instead of 8 open-ended questions.

**Question 1: API Architecture**

Use AskUserQuestion:
```markdown
Question: "What API architecture fits your needs?"
Header: "API Style"
Options:
- REST with OpenAPI (Recommended for most web apps)
  → Includes: REST endpoints, OpenAPI 3.0 spec, Swagger UI
  → Auth: JWT tokens, OAuth2 support
  → Versioning: URL-based (/v1/)
  → Error format: RFC 7807 Problem Details

- GraphQL with Schema-first
  → Includes: GraphQL schema, type definitions, resolvers
  → Auth: Context-based, JWT integration
  → Versioning: Schema evolution (no explicit versions)
  → Error format: GraphQL errors array

- Minimal REST (simple projects)
  → Includes: Basic CRUD endpoints, simple JSON responses
  → Auth: API keys
  → Versioning: None (single version)
  → Error format: Simple {error: "message"}
```

**Question 2: Scale & Requirements**

Use AskUserQuestion:
```markdown
Question: "What features do you need?"
Header: "Features"
MultiSelect: true (can select multiple)
Options:
- Rate limiting (recommended for public APIs)
- Request validation with schemas
- API documentation generation
- Request/response logging
- Caching layer
```

**Benefits of this approach:**
- 8 questions → 2 questions
- Open text → Clear choices with context
- Each choice bundles related decisions
- Users see trade-offs clearly
- Faster completion
- Better decisions
```

**Value Delivered:**
- ✅ 75% fewer questions (8 → 2)
- ✅ Faster briefing (~2 min vs ~8 min)
- ✅ Clearer decisions with context
- ✅ Bundled related choices
- ✅ Better UX through menus vs text input

---

## Example 3: Adding Self-Checking (Type #7)

### Analyzed Skill: documentation-writer

**Business Goal:** Generate comprehensive, accurate documentation for codebases

**Current Problem:**
- Docs generated but quality varies
- Common issues: missing examples, broken links, inconsistent style
- User spends time reviewing and fixing
- No automated quality gates

**Upgrade Proposal:**

**Type:** #7 Self-checking
**Value:** High - Significant quality improvement
**Effort:** Medium - Add quality verification phase
**Priority:** HIGH

### Implementation

**Before SKILL.md:**
```markdown
## Workflow

1. Analyze codebase structure
2. Extract docstrings and comments
3. Generate documentation sections
4. Format in markdown
5. Save to docs/ folder
6. Report completion to user
```

**After SKILL.md:**
```markdown
## Workflow

1. Analyze codebase structure
2. Extract docstrings and comments
3. Generate documentation sections
4. Format in markdown

5. **Self-Check Quality (NEW)**

   Before presenting to user, run automated quality checks:

   **Completeness Check:**
   - [ ] All public functions documented?
   - [ ] All class methods documented?
   - [ ] All parameters explained?
   - [ ] Return types specified?
   - [ ] Exceptions documented?

   **Clarity Check:**
   - [ ] No jargon without definition?
   - [ ] Examples for complex features?
   - [ ] Clear section headings?
   - [ ] Logical flow?
   - [ ] Code examples present?

   **Technical Check:**
   - [ ] Code examples syntax valid? (run through linter)
   - [ ] Links resolve correctly? (check internal links)
   - [ ] Version numbers consistent?
   - [ ] API signatures match current code?

   **Style Check:**
   - [ ] Consistent terminology?
   - [ ] Proper markdown formatting?
   - [ ] Table of contents generated?
   - [ ] Code blocks have language specified?

   **Action on failures:**

   If any check fails:
   - **Attempt automatic fix:**
     - Broken links → Fix relative paths
     - Invalid code syntax → Correct based on linter output
     - Missing TOC → Generate automatically
     - Inconsistent terminology → Standardize using most common term

   - **If can't auto-fix:**
     - Document specific issues
     - Highlight sections needing review
     - Explain what's wrong and why

   - **Re-check after fixes**

   Only proceed to step 6 after passing all checks or documenting unfixable issues.

6. **Present to user:**
   - Documentation in docs/ folder
   - Quality report: "✅ All checks passed" or "⚠️ 3 items need review"
   - List any issues that need manual attention

7. Report completion
```

**New file: references/quality-criteria.md**
```markdown
# Documentation Quality Criteria

Detailed explanation of each quality check and how to fix common issues.

## Completeness Checks

### All Public Functions Documented

**Check:** Every function in public API has docstring
**How to detect:**
```python
def find_undocumented_functions():
    # Parse code, find public functions without docstrings
    pass
```

**Auto-fix:** Generate basic docstring template
**Manual review needed:** If function is complex

[...detailed criteria for all checks...]
```

**Value Delivered:**
- ✅ Higher quality docs (fewer user corrections needed)
- ✅ Catches 90% of common issues automatically
- ✅ Many issues auto-fixed
- ✅ Clear report of what needs attention
- ✅ Users can trust output more
- ✅ Saves user review time

---

## Example 4: Adding Context-Awareness (Type #10)

### Analyzed Skill: test-generator

**Business Goal:** Generate appropriate tests for codebase

**Current Problem:**
- Generic test templates regardless of framework
- User must manually adapt tests to their stack
- Inconsistent with project conventions
- Doesn't leverage framework-specific features

**Upgrade Proposal:**

**Type:** #10 Context-awareness
**Value:** Very High - Tests actually work without modification
**Effort:** High - Multiple framework patterns to support
**Priority:** HIGH

### Implementation

**Before SKILL.md:**
```markdown
## Workflow

1. Analyze function to test
2. Generate generic test template
3. User adapts to their framework
```

Generic output:
```python
# Generic test (doesn't match any real framework well)
def test_function():
    # Arrange
    input = prepare_input()
    # Act
    result = function(input)
    # Assert
    assert result == expected
```

**After SKILL.md:**
```markdown
## Workflow

1. **Detect Project Context (NEW)**

   Auto-detect framework and conventions:

   **Check package files:**
   - `package.json` → Node.js project
     - Has "react"? → React project
     - Has "jest"? → Jest testing
   - `requirements.txt` or `pyproject.toml` → Python project
     - Has "pytest"? → Pytest
     - Has "unittest"? → Python unittest
   - `pom.xml` or `build.gradle` → Java project
     - JUnit version from dependencies

   **Check existing tests:**
   - Test file naming convention (test_*.py vs *_test.py vs *.spec.ts)
   - Import patterns (what testing libs used)
   - Fixture patterns
   - Mocking approach

   **Determine conventions:**
   - Test structure (AAA, Given-When-Then, etc.)
   - Naming patterns
   - Directory structure

2. **Load Framework-Specific Patterns**

   Based on detection, load appropriate reference:

   **React + Jest detected:**
   - Load: [references/react-jest-patterns.md](references/react-jest-patterns.md)
   - Use: React Testing Library patterns
   - Follow: Jest conventions

   **Python + Pytest detected:**
   - Load: [references/pytest-patterns.md](references/pytest-patterns.md)
   - Use: Pytest fixtures, parametrize decorators
   - Follow: Pytest conventions

   **Java + JUnit 5 detected:**
   - Load: [references/junit5-patterns.md](references/junit5-patterns.md)
   - Use: @Test annotations, assertions
   - Follow: JUnit conventions

3. Analyze function to test

4. **Generate Framework-Specific Test**

   Using detected patterns and conventions.
```

**After - Framework-Specific Outputs:**

**React project output:**
```typescript
// Matches project's React + Jest setup
import { render, screen, fireEvent } from '@testing-library/react';
import { UserProfile } from './UserProfile';

describe('UserProfile', () => {
  it('displays user information correctly', () => {
    const user = { name: 'Alice', email: 'alice@example.com' };

    render(<UserProfile user={user} />);

    expect(screen.getByText('Alice')).toBeInTheDocument();
    expect(screen.getByText('alice@example.com')).toBeInTheDocument();
  });
});
```

**Python project output:**
```python
# Matches project's Pytest setup
import pytest
from myapp.user_service import get_user_profile

@pytest.fixture
def sample_user():
    return {"name": "Alice", "email": "alice@example.com"}

def test_get_user_profile_returns_correct_data(sample_user):
    # Arrange
    user_id = 123

    # Act
    result = get_user_profile(user_id)

    # Assert
    assert result["name"] == sample_user["name"]
    assert result["email"] == sample_user["email"]
```

**New files:**

`references/react-jest-patterns.md`:
```markdown
# React + Jest Testing Patterns

## Standard Structure

```typescript
describe('ComponentName', () => {
  it('describes expected behavior', () => {
    // Test implementation
  });
});
```

## Common Patterns

### Testing Component Rendering
[...patterns...]

### Testing User Interactions
[...patterns...]

### Mocking API Calls
[...patterns...]
```

**Value Delivered:**
- ✅ Tests work immediately without modification
- ✅ Match project conventions automatically
- ✅ Use framework-specific best practices
- ✅ Consistent with existing tests
- ✅ Leverage framework features (fixtures, hooks, etc.)
- ✅ 10x better developer experience

---

## Example 5: Adding Batch Processing (Type #20)

### Analyzed Skill: file-analyzer

**Business Goal:** Analyze files for quality, security, performance issues

**Current Problem:**
- Must run separately for each file
- Repeated setup overhead
- No aggregate insights across files
- Slow for large projects

**Upgrade Proposal:**

**Type:** #20 Batch processing
**Value:** High - 10x performance improvement for many files
**Effort:** Medium - Add batch mode and aggregation
**Priority:** HIGH

### Implementation

**Before SKILL.md:**
```markdown
## Usage

```bash
/file-analyzer path/to/file.py
```

## Workflow

1. Read file
2. Analyze content
3. Generate report
4. Save to `.tasks/file-analyzer-[filename].md`
```

**After SKILL.md:**
```markdown
## Usage

**Single File Mode:**
```bash
/file-analyzer path/to/file.py
```

**Batch Mode (NEW):**
```bash
# Analyze all Python files
/file-analyzer --batch "**/*.py"

# Analyze specific files
/file-analyzer --batch file1.py file2.py file3.py

# Analyze files from list
/file-analyzer --batch @files.txt
```

## Workflow

### Single File Mode

1. Read file
2. Analyze content
3. Generate report
4. Save to `.tasks/file-analyzer-[filename].md`

### Batch Mode (NEW)

1. **Discovery**
   - Expand glob patterns
   - Read file list
   - Count total files: "Found 150 files to analyze"

2. **Shared Setup (once for all files)**
   - Load quality rules
   - Initialize analysis tools
   - Prepare output directory
   - Configure linters

   **Performance benefit:** Setup once instead of 150 times

3. **Process Files**

   **Option A: Sequential (default for <20 files)**
   ```markdown
   For each file:
   - Analyze (reusing shared setup)
   - Collect results
   - Show progress: "[23/150] Analyzing src/utils.py..."
   ```

   **Option B: Parallel (for 20+ files)**
   ```markdown
   Process files in parallel (batches of 10):
   - Batch 1: Files 1-10 (parallel)
   - Batch 2: Files 11-20 (parallel)
   - ...
   - Show progress: "[50/150] 33% complete, ~5 min remaining"
   ```

4. **Aggregate Results**

   **Individual Reports:**
   - Save each file's report to `.tasks/file-analyzer/reports/[filename].md`

   **Aggregate Report (NEW):**
   ```markdown
   # Batch Analysis Report

   Generated: 2024-01-15 10:30:00

   ## Summary

   - **Files analyzed:** 150
   - **Total issues:** 342
   - **Critical:** 12
   - **High:** 45
   - **Medium:** 127
   - **Low:** 158

   ## Top Issues by Category

   1. **Security (23 issues across 12 files)**
      - SQL injection: 8 occurrences
      - XSS vulnerabilities: 7 occurrences
      - Hardcoded secrets: 8 occurrences

   2. **Performance (67 issues across 34 files)**
      - N+1 queries: 23 occurrences
      - Inefficient loops: 28 occurrences
      - Missing indexes: 16 occurrences

   ## Worst Files (most issues)

   1. `src/api/auth.py` - 23 issues (4 critical)
   2. `src/db/queries.py` - 19 issues (3 critical)
   3. `src/utils/helpers.py` - 15 issues (1 critical)

   ## Files by Status

   ✅ Clean (0 issues): 45 files
   ⚠️  Minor issues (1-5): 78 files
   🔴 Needs attention (6+): 27 files

   ## Detailed Reports

   See `.tasks/file-analyzer/reports/` for individual file reports.
   ```

5. **Performance Stats**
   ```markdown
   **Batch processing performance:**
   - Total time: 45 seconds
   - Files per second: 3.3
   - Compared to sequential single-file: ~25 minutes saved
   ```
```

**Value Delivered:**
- ✅ 10x faster (shared setup, parallel processing)
- ✅ Aggregate insights (see patterns across codebase)
- ✅ Find worst offenders easily
- ✅ Track trends over time
- ✅ Better for CI/CD integration
- ✅ Single comprehensive report + detailed per-file reports

---

## Example 6: Adding Early Validation (Type #6)

### Analyzed Skill: deploy-to-production

**Business Goal:** Deploy applications safely to production

**Current Problem:**
- Failures happen late in deployment (after 15 minutes)
- Prerequisites not checked upfront
- Wasted time on doomed deployments
- Unclear error messages when something missing

**Upgrade Proposal:**

**Type:** #6 Early validation
**Value:** Very High - Prevents wasted time and failed deploys
**Effort:** Low - Add pre-flight checks
**Priority:** HIGH

### Implementation

**Before SKILL.md:**
```markdown
## Workflow

1. Build application
   [10 minutes later...] → Fails if npm not installed

2. Run tests
   [5 minutes later...] → Fails if test DB not running

3. Deploy to server
   [3 minutes later...] → Fails if SSH keys not configured

Total wasted time on typical failure: 18 minutes
```

**After SKILL.md:**
```markdown
## Workflow

### Phase 0: Pre-flight Checks (NEW)

**Run before starting deployment to catch issues early:**

1. **Required Tools**
   ```bash
   # Check all tools exist
   which node npm git docker ssh

   # Check versions
   node --version    # Need >= 18.0.0
   npm --version     # Need >= 9.0.0
   docker --version  # Need >= 20.0.0
   ```

   **If missing:**
   - ❌ "Node.js not found. Install with: brew install node"
   - ❌ "Docker not running. Start with: open -a Docker"

2. **Environment Validation**
   ```bash
   # Check environment variables
   [ -z "$DATABASE_URL" ] && echo "Missing DATABASE_URL"
   [ -z "$API_KEY" ] && echo "Missing API_KEY"

   # Check files exist
   [ -f ".env.production" ] || echo "Missing .env.production"
   [ -f "docker-compose.yml" ] || echo "Missing docker-compose.yml"
   ```

3. **Credentials & Access**
   ```bash
   # Test SSH access
   ssh -o BatchMode=yes user@production-server 'echo OK' 2>&1

   # Test Docker registry access
   docker login registry.example.com --username test --password-stdin

   # Test database connection
   psql $DATABASE_URL -c "SELECT 1" 2>&1
   ```

4. **Resource Checks**
   ```bash
   # Sufficient disk space (need 5GB)
   df -h . | awk 'NR==2 {print $4}'

   # Server has capacity
   ssh user@server 'free -h && df -h'
   ```

5. **Code Quality Gates**
   ```bash
   # Lint passes
   npm run lint

   # No known vulnerabilities
   npm audit --audit-level=high

   # Tests exist
   [ -d "test" ] || [ -d "tests" ] || echo "No tests found"
   ```

**Results:**

✅ **All checks passed**
```
✅ All required tools installed
✅ Environment variables configured
✅ Credentials valid
✅ Sufficient resources
✅ Code quality passed

Proceeding to deployment...
Total pre-flight time: 15 seconds
```

❌ **Checks failed**
```
❌ Pre-flight checks failed (3 issues):

1. Docker not running
   Fix: open -a Docker
   Then re-run deployment

2. Missing environment variable: DATABASE_URL
   Fix: Add to .env.production or export DATABASE_URL=...

3. SSH key not configured for production server
   Fix: ssh-copy-id user@production-server

Fix these issues and run again.
No changes were made to production.
```

### Phase 1: Build Application

[...continues with actual deployment after checks pass...]
```

**Value Delivered:**
- ✅ Failures detected in 15 seconds instead of 18 minutes
- ✅ Clear, actionable error messages
- ✅ No wasted time on doomed deployments
- ✅ Prevents partial failed deployments
- ✅ Better developer experience
- ✅ Can fix all issues before retry

---

## Summary: Impact Metrics

### Before Upgrades

| Skill | Main Issue | User Pain |
|-------|------------|-----------|
| code-refactor | No preview | Anxiety about changes |
| api-design | 8 questions | Fatigue, confusion |
| documentation-writer | Variable quality | Time spent fixing |
| test-generator | Generic tests | Manual adaptation needed |
| file-analyzer | One at a time | Slow for projects |
| deploy-to-production | Late failures | Wasted time |

### After Upgrades

| Skill | Improvement | Measurable Benefit |
|-------|-------------|-------------------|
| code-refactor | Dry-run mode | 100% confidence, teaching tool |
| api-design | 2 questions | 75% fewer questions, 60% faster |
| documentation-writer | Self-checking | 90% fewer corrections |
| test-generator | Context-aware | Tests work immediately |
| file-analyzer | Batch mode | 10x faster (25 min → 2.5 min) |
| deploy-to-production | Pre-flight | Failures at 15s not 18min |

### Common Patterns

**Most impactful upgrade types:**
1. **#16 Dry-run mode** - Builds trust
2. **#14 Interactive choices** - Better UX
3. **#7 Self-checking** - Higher quality
4. **#10 Context-awareness** - Works without modification
5. **#20 Batch processing** - Performance improvement
6. **#6 Early validation** - Prevents waste

**Key success factors:**
- ✅ Solves real user pain
- ✅ Measurable improvement
- ✅ Doesn't complicate workflow
- ✅ Clear value proposition
- ✅ Works with existing features
