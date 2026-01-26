# Upgrade Types - Complete Reference

Szczegółowy przewodnik po wszystkich 22 typach ulepszeń dla agent skills.

## How to Use This Reference

**Podczas Phase 2 (Identify Upgrade Opportunities):**
- Przejrzyj każdy typ
- Zadaj pytania assessment dla każdego typu
- Dokumentuj znalezione możliwości
- Priorytetyzuj na podstawie value vs effort

---

## Category 1: Workflow Enhancements

### Type 1: Add New Features

**Description:** Rozszerzenie możliwości skilla w ramach jego obecnej domeny

**When to consider:**
- Skill robi X, ale users często pytają o Y (pokrewne)
- Istnieje naturalne rozszerzenie funkcjonalności
- Feature pasuje do core purpose

**Assessment questions:**
- Czy są pokrewne operacje których skill nie obsługuje?
- Czy users proszą o dodatkowe capability?
- Czy feature zmieści się w scope bez scope creep?

**Examples:**

**PDF skill:** Obecnie ekstraktuje tekst → Dodaj wypełnianie formularzy
**API tester:** Obecnie testuje GET → Dodaj POST/PUT/DELETE z body
**Git helper:** Obecnie commit messages → Dodaj branch naming suggestions

**Red flags:**
- Feature wykracza poza domenę (scope creep)
- Duplikuje inny istniejący skill
- Zbyt złożony dla obecnej struktury

**Implementation approach:**
- Dodaj nową sekcję w workflow
- Zaznacz jako optional (jeśli nie zawsze potrzebny)
- Dodaj przykłady użycia
- Dokumentuj kiedy używać

---

### Type 2: Simplify Workflow

**Description:** Zmniejszenie liczby kroków, eliminacja redundancji

**When to consider:**
- Workflow ma >7 kroków
- Niektóre kroki można połączyć
- Są powtarzające się operacje
- Users zgubią się w procesie

**Assessment questions:**
- Które kroki można połączyć bez utraty clarity?
- Czy są redundantne operacje?
- Czy niektóre kroki można zautomatyzować?
- Czy sekwencja jest optymalna?

**Examples:**

**Before (8 steps):**
```markdown
1. Read file
2. Parse content
3. Validate structure
4. Transform data
5. Validate transformation
6. Format output
7. Validate format
8. Save result
```

**After (4 steps):**
```markdown
1. Read and parse file (with validation)
2. Transform data (with validation)
3. Format and save output (with validation)
4. Verify final result
```

**Implementation approach:**
- Zidentyfikuj kroki które naturalnie należą do siebie
- Połącz operacje + ich walidację
- Zachowaj clarity - nie łącz jeśli stanie się niejasne
- Update examples żeby pokazać prostszy flow

---

### Type 3: Parallel Execution

**Description:** Wykonywanie niezależnych operacji równolegle zamiast sekwencyjnie

**When to consider:**
- Skill ma kroki które nie zależą od siebie
- Sekwencyjne wykonanie jest slow
- Operacje są idempotent (można bezpiecznie równolegle)

**Assessment questions:**
- Które kroki nie mają dependencies między sobą?
- Czy równoległe wykonanie przyspieszy workflow znacząco?
- Czy wszystkie operacje są thread-safe?

**Examples:**

**Code review skill:**
```markdown
Before:
1. Analyze security (file by file)
2. Check performance (file by file)
3. Verify style (file by file)

After:
1. For each file, run in parallel:
   - Security analysis
   - Performance check
   - Style verification
2. Merge results
```

**Multi-file processing:**
```markdown
Before:
For each file:
  - Read
  - Process
  - Save

After:
Read all files (parallel) → Process all (parallel) → Save all (parallel)
```

**Implementation approach:**
- Zidentyfikuj independent operations
- Dodaj instrukcję do równoległego wykonania
- Wyjaśnij jak merge results
- Note any ordering requirements dla output

---

### Type 4: Checkpoints

**Description:** Możliwość wznowienia długich procesów od punktu przerwania

**When to consider:**
- Workflow takes substantial time to complete
- Operacje są "expensive" (API calls, długie obliczenia)
- Failures mogą wystąpić w trakcie
- Users mogą chcieć przerwać i wrócić

**Assessment questions:**
- Czy workflow może trwać długo?
- Czy failure w middle step = restart from beginning?
- Czy są natural break points gdzie można save state?

**Examples:**

**Batch processing skill:**
```markdown
Add checkpoints:

Checkpoint 1: After file discovery (save file list)
Checkpoint 2: After processing 50% (save results so far)
Checkpoint 3: After processing 100% (save full results)

On resume:
- Check for checkpoint files
- Load most recent state
- Continue from that point
```

**Implementation approach:**
- Zidentyfikuj natural save points
- Zapisuj state do `.tasks/skill-[name]-checkpoint.json`
- Na początku workflow check for checkpoint
- If found: load state and resume
- Clear checkpoint on successful completion

---

## Category 2: Quality & Reliability

### Type 5: Better Edge Cases

**Description:** Graceful handling nietypowych scenariuszy

**When to consider:**
- Skill ma "happy path" ale brak error handling
- Users napotykają edge cases który skill nie obsługuje
- Brak guidance co robić gdy coś jest unusual

**Assessment questions:**
- Jakie nietypowe inputs mogą wystąpić?
- Co jeśli file jest empty? Very large? Malformed?
- Co jeśli user permissions są insufficient?
- Co jeśli external dependency jest unavailable?

**Examples:**

**Add edge case handling:**

```markdown
## Edge Cases

**Empty input:**
- Detect empty file/data
- Ask user: "Input is empty. Continue with empty result, or cancel?"

**Very large input (>100MB):**
- Warn user: "Large file detected. Processing may take time."
- Offer batch processing option

**Malformed data:**
- Show specific parse error
- Suggest common fixes
- Offer to continue with partial data

**Network failures:**
- Retry 3 times with exponential backoff
- If still fails: save progress and report error
```

**Implementation approach:**
- List common edge cases
- For each: define detection + handling strategy
- Add to workflow at appropriate points
- Include examples of error messages

---

### Type 6: Early Validation

**Description:** Catching problemów na początku workflow zamiast na końcu

**When to consider:**
- Failures typically happen late in workflow
- Prerequisites można sprawdzić upfront
- Users waste time on doomed workflows

**Assessment questions:**
- Jakie pre-conditions must be true for success?
- Co można zweryfikować przed rozpoczęciem?
- Które failures są predictable?

**Examples:**

**Add Pre-flight Checks section:**

```markdown
## Phase 0: Pre-flight Checks (NEW)

Before starting main workflow:

1. **Verify prerequisites:**
   - [ ] Required tools installed (git, npm, etc.)
   - [ ] Working directory is valid
   - [ ] User has necessary permissions

2. **Validate inputs:**
   - [ ] File paths exist
   - [ ] File formats are correct
   - [ ] Required fields are present

3. **Check environment:**
   - [ ] Network connectivity (if needed)
   - [ ] Sufficient disk space
   - [ ] API keys configured (if needed)

If any check fails:
- Report specific issue
- Suggest fix
- Stop before wasting time on doomed workflow
```

**Implementation approach:**
- Dodaj Phase 0 na początku workflow
- Lista wszystkich preconditions
- Make checks fast (don't slow down happy path much)
- Clear error messages with actionable fixes

---

### Type 7: Self-Checking

**Description:** Skill automatycznie weryfikuje quality swojego outputu

**When to consider:**
- Output ma measurable quality criteria
- Users often find issues after completion
- Quality problems są detectable programmatically

**Assessment questions:**
- Jakie quality criteria można automatycznie sprawdzić?
- Co często wymaga user correction?
- Jakie są common mistakes/oversights?

**Examples:**

**Documentation skill - add quality phase:**

```markdown
## Phase 5: Self-Check Quality (NEW)

Before presenting docs to user:

**Completeness Check:**
- All public functions documented?
- All parameters explained?
- Return types specified?

**Clarity Check:**
- Any jargon without definition?
- Examples for complex features?
- Clear section headings?

**Technical Check:**
- Code examples syntax valid?
- Links resolve?
- Version numbers consistent?

**Action:**
- If any check fails: Fix automatically if possible
- If can't fix: Report to user with specific issues
- Only present to user after passing all checks
```

**Implementation approach:**
- Dodaj quality phase przed finalization
- Lista measurable criteria
- Auto-fix gdy possible
- Report issues które wymagają human decision
- Re-check after fixes

---

### Type 8: Error Recovery

**Description:** Smart handling failures z rollback, retry, repair options

**When to consider:**
- Operations są partially reversible
- Transient failures mogą succeed on retry
- Partial success jest valuable
- Manual cleanup po failure jest painful

**Assessment questions:**
- Co można safely rollback?
- Które failures są transient (worth retry)?
- Czy partial success można save?
- Jak cleanup after failure?

**Examples:**

**Deployment skill - add recovery:**

```markdown
## Error Recovery Strategy (NEW)

**For each operation:**

**Transient failure (network, rate limit):**
1. Wait (exponential backoff)
2. Retry up to 3 times
3. If succeeds: continue
4. If fails: move to rollback

**Non-transient failure (validation, permissions):**
1. Don't retry (won't help)
2. Save progress so far
3. Move to rollback

**Rollback procedure:**
1. Identify what was changed
2. Reverse changes in reverse order
3. Verify rollback successful
4. Report what was restored

**Partial success handling:**
- Save successfully processed items
- Report what succeeded / what failed
- Offer: "Continue from failure point?" or "Rollback all?"
```

**Implementation approach:**
- Classify operations jako reversible/irreversible
- Track changes dla potential rollback
- Implement retry logic dla transient failures
- Clear communication co się stało i co user może zrobić

---

## Category 3: Automation & Intelligence

### Type 9: Automate Manual Steps

**Description:** Replace human intervention z automatic logic

**When to consider:**
- Users repeatedly perform same manual steps
- Decision logic jest deterministic
- Information for decision jest available
- Manual step slows workflow significantly

**Assessment questions:**
- Które kroki wymagają human input ale mogą być automated?
- Czy decision logic jest clear i deterministic?
- Czy automation adds value (not just complexity)?

**Examples:**

**API design skill:**

**Before:**
```markdown
1. Analyze requirements
2. Ask user: "What response format?" (JSON/XML)
3. Ask user: "What error format?" (RFC7807/Custom)
4. Ask user: "What auth?" (OAuth/API Key)
[...many questions...]
```

**After:**
```markdown
1. Analyze requirements
2. Auto-detect patterns:
   - Modern web app → JSON + RFC7807 + OAuth (recommended)
   - Legacy integration → XML + Custom errors + API Key
   - Mobile app → JSON + problem details + JWT
3. Present recommendation with rationale
4. User can accept or customize
```

**Implementation approach:**
- Identify decision patterns
- Create decision logic based on context
- Present as "recommendation" not "mandate"
- Always allow user override
- Explain rationale for auto-decision

---

### Type 10: Context-Awareness

**Description:** Adapt behavior do project type, user preferences, history

**When to consider:**
- Skill używany w różnych contexts (React vs Vue, Python vs JS)
- "One size fits all" jest suboptimal
- Context można auto-detect
- Different contexts have different best practices

**Assessment questions:**
- Jakie contexts ma skill handle?
- Co można auto-detect z project files?
- Jak behavior powinien się różnić per context?

**Examples:**

**Test generation skill:**

```markdown
## Context Detection (NEW)

**Auto-detect project type:**

1. **Check for framework markers:**
   - `package.json` + "react" → React project
   - `requirements.txt` + "pytest" → Python project
   - `pom.xml` → Java project

2. **Adapt test style:**

   **React project:**
   - Use Jest + React Testing Library
   - Focus on component behavior
   - Follow React testing patterns

   **Python project:**
   - Use pytest
   - Follow pytest conventions
   - Consider fixtures pattern

   **Java project:**
   - Use JUnit 5
   - Follow Java naming conventions
   - Consider mocking frameworks

3. **Load appropriate reference:**
   - `references/react-testing.md`
   - `references/python-testing.md`
   - `references/java-testing.md`
```

**Implementation approach:**
- Detect context markers (config files, dependencies)
- Load context-specific guidance from references/
- Adapt recommendations, examples, syntax
- Document detection logic clearly

---

### Type 11: Learning from History

**Description:** Use poprzednich wykonań skilla do improve suggestions

**When to consider:**
- Skill wykonywany repeatedly w tym samym projekcie
- Patterns mogą być learned from previous runs
- User preferences można remember
- Historical data helps make better decisions

**Assessment questions:**
- Co może być learned z previous runs?
- Gdzie store historical data?
- Jak use history without overfitting?

**Examples:**

**Commit message skill:**

```markdown
## History-Based Improvements (NEW)

**Track commit patterns:**

Save to `.tasks/commit-history.json`:
```json
{
  "lastCommits": [
    {"type": "feat", "scope": "api", "pattern": "endpoint"},
    {"type": "fix", "scope": "ui", "pattern": "button"},
    {"type": "docs", "scope": "readme", "pattern": "install"}
  ],
  "commonScopes": ["api", "ui", "database"],
  "preferredStyle": "conventional"
}
```

**Use history:**
- Suggest frequent scopes first
- Match style of recent commits
- Detect project conventions automatically
- Improve suggestions over time
```

**Implementation approach:**
- Define co track (preferences, patterns, choices)
- Save to `.tasks/[skill]-history.json`
- Load history at start
- Use to improve suggestions
- Don't override user's explicit choices

---

### Type 12: Smart Defaults

**Description:** Intelligent initial choices based on patterns and context

**When to consider:**
- Skill asks many configuration questions
- Most answers są similar for similar projects
- Defaults można infer from context
- Users often just hit "enter" on questions

**Assessment questions:**
- Które defaults są hardcoded ale could be smart?
- Co można infer from project structure?
- Jakie są most common user choices?

**Examples:**

**API scaffold skill:**

**Before:**
```markdown
Q: Port? [default: 3000]
Q: Database? [default: postgres]
Q: Auth? [default: jwt]
```

**After:**
```markdown
## Smart Defaults (NEW)

**Detect from project:**
- Check existing services for port conflicts → suggest first available
- Check `docker-compose.yml` for database → match existing
- Check if Keycloak in project → suggest OAuth, else JWT

**Present:**
"Detected: React frontend (port 3000), Postgres (already running)
Suggesting: Backend on port 3001, Postgres connection, JWT auth

Accept defaults or customize?"
```

**Implementation approach:**
- List co można infer
- Check environment/config files
- Calculate smart defaults
- Present with rationale
- Easy to override

---

## Category 4: User Experience

### Type 13: Better Briefing

**Description:** Smarter questions, fewer iterations to get requirements

**When to consider:**
- Current briefing asks too many questions
- Questions są confusing or poorly ordered
- Users often misunderstand pytania
- Iteracje są needed to clarify

**Assessment questions:**
- Które questions są often misunderstood?
- Czy question order jest logical?
- Czy można reduce number of questions?
- Czy można make questions clearer?

**Examples:**

**Before:**
```markdown
Q1: What should this do?
Q2: Any special requirements?
Q3: What output format?
Q4: Should it handle errors?
Q5: What about edge cases?
[...vague, open-ended...]
```

**After:**
```markdown
Q1: What's the main task? [with 3 concrete examples]

Q2: Critical scenarios to handle?
- Normal case (happy path)
- When [specific edge case]
- If [specific error condition]

Q3: Output preference?
[Choice menu with examples of each format]

Q4: Quality standards?
[Quick checklist instead of open question]
```

**Implementation approach:**
- Review current questions for clarity
- Add examples to make concrete
- Use choice menus where possible
- Group related questions
- Order logically (general → specific)

---

### Type 14: Interactive Choices

**Description:** Option menus zamiast open text questions

**When to consider:**
- Questions mają limited set of valid answers
- Users nie są sure co wpisać
- Typing answers jest tedious
- Same questions asked repeatedly

**Assessment questions:**
- Które open questions mają predictable answers?
- Czy można enumerate options?
- Czy options są mutually exclusive?

**Examples:**

**Convert to AskUserQuestion with choices:**

**Before:**
```markdown
Q: What testing framework do you want to use?
[User types answer, might typo, might not know options]
```

**After:**
```markdown
Use AskUserQuestion tool:

Question: Which testing framework?
Options:
- Jest (Recommended for React/Node)
- Pytest (Python projects)
- JUnit (Java projects)
- RSpec (Ruby projects)

Includes descriptions to help choose.
```

**Implementation approach:**
- Identify questions with enumerable options
- Convert to AskUserQuestion with choice menu
- Add descriptions to each option
- Mark recommended option
- Allow multi-select gdzie appropriate

---

### Type 15: Progress Visibility

**Description:** Clear feedback co się dzieje w danym momencie

**When to consider:**
- Long-running operations
- Multi-step workflows
- Users nie są sure if it's working or stuck
- Anxiety o "what's happening now"

**Assessment questions:**
- Które operations take noticeable time?
- Czy users know co się dzieje?
- Czy można show progress?

**Examples:**

**Add progress indicators:**

```markdown
## Workflow with Progress (NEW)

[1/5] ✅ Prerequisites verified
[2/5] ⏳ Analyzing files... (15/50 files)
[3/5] ⏸️  Pending (after analysis)
[4/5] ⏸️  Pending (after validation)
[5/5] ⏸️  Pending (after generation)

For long operations:
"Analyzing security... (file 23/150, 15% complete, ~2 min remaining)"
```

**Implementation approach:**
- Break workflow into visible phases
- Show phase progress (X/Y)
- For long steps: show sub-progress
- Use status icons (✅ ⏳ ⏸️ ❌)
- Update regularly

---

### Type 16: Dry-Run Mode

**Description:** Preview zmian przed commit

**When to consider:**
- Skill modifies files/system
- Changes są significant or risky
- Users want confidence before applying
- Teaching tool (show what would happen)

**Assessment questions:**
- Czy skill modifies files?
- Czy changes są easily reversible?
- Czy users są nervous o automatic changes?

**Examples:**

**Add preview mode:**

```markdown
## Modes (NEW)

**Preview Mode (--dry-run):**
1. Analyze and plan changes
2. Show diffs of all proposed changes
3. Summary of what would change
4. Ask: "Apply these changes?"
   - Yes → execute
   - No → cancel
   - Modify → adjust and preview again

**Direct Mode:**
1. Analyze and apply immediately
2. Report what was changed
3. Changes committed

**Default:** Preview mode for first-time users, Direct mode after trust established
```

**Implementation approach:**
- Add mode selection
- Collect all changes before applying
- Generate previews/diffs
- Show to user for approval
- Execute only after confirmation
- Consider making preview default

---

## Category 5: Integration & Output

### Type 17: Tool Integration

**Description:** Better use of available tools (bash, git, etc.)

**When to consider:**
- Skill manually performs co tool could do
- Multiple bash commands could be combined
- Tool output jest ignored but valuable
- Inefficient tool usage

**Assessment questions:**
- Które tools does skill use?
- Czy są used optimally?
- Czy combined commands would be better?
- Czy tool output zawiera useful info currently ignored?

**Examples:**

**Better git integration:**

**Before:**
```markdown
1. Run: git status
2. Run: git diff
3. Run: git log
4. Parse all outputs manually
```

**After:**
```markdown
1. Run single command:
   git status --short && git diff --stat && git log --oneline -5

2. Use git's structured output:
   git status --porcelain (machine-readable)
   git log --pretty=format:'%h|%s|%an|%ar'

3. Leverage git features:
   git diff --name-status (cleaner than full diff)
```

**Implementation approach:**
- Review bash/tool commands
- Combine gdzie sensible
- Use structured output formats
- Leverage tool-specific features
- Add error handling

---

### Type 18: Reusable Outputs

**Description:** Generate outputs które można use w innych skills/tools

**When to consider:**
- Output jest currently human-only readable
- Other skills could benefit from data
- Manual re-entry of info elsewhere
- Pipeline workflows możliwe

**Assessment questions:**
- Czy output could feed into other processes?
- Jakie format would be most useful?
- Gdzie output może być reused?

**Examples:**

**Make output reusable:**

```markdown
## Output Formats (NEW)

**Human-readable (default):**
Markdown report w `.tasks/[skill]-report.md`

**Machine-readable (NEW):**
JSON output w `.tasks/[skill]-output.json`:
```json
{
  "skill": "code-review",
  "timestamp": "2024-01-15T10:30:00Z",
  "summary": {...},
  "issues": [
    {
      "file": "app.py",
      "line": 45,
      "severity": "critical",
      "type": "security",
      "description": "...",
      "fix": "..."
    }
  ]
}
```

**Benefits:**
- Other skills can read JSON
- Dashboard can display metrics
- CI/CD can parse results
```

**Implementation approach:**
- Generate both human & machine formats
- Use standard schemas
- Document output structure
- Include metadata (timestamp, version)

---

### Type 19: Better Reports

**Description:** More actionable, clearer insights w output

**When to consider:**
- Current reports są unclear or overwhelming
- Users don't know what to do with results
- Important info buried in details
- Lack of prioritization

**Assessment questions:**
- Czy users understand report?
- Czy know what to do next?
- Czy important info stands out?
- Czy format is optimal?

**Examples:**

**Improve report structure:**

**Before:**
```markdown
# Results

Found 45 issues in code.
Issue 1: app.py line 23...
Issue 2: api.py line 15...
[...43 more...]
```

**After:**
```markdown
# Code Review Report

## 🎯 Summary

- **45 issues** found across **12 files**
- **3 CRITICAL** (must fix before merge)
- **15 HIGH** (should fix soon)
- **27 LOW** (nice to have)

## 🔴 Critical Issues (Fix Now)

### 1. SQL Injection Risk
**File:** app.py:23
**Impact:** User data can be stolen
**Fix:** Use parameterized queries
**Example:** [concrete code fix]

[...other critical...]

## 🟡 High Priority

[Grouped by category: Security, Performance, Bugs]

## 🟢 Low Priority

[Collapsed by default, expandable]

## 📊 Metrics

- Code quality score: 7.5/10
- Security score: 6/10 ⚠️
- Files reviewed: 45
- Time: 2.3s
```

**Implementation approach:**
- Start with executive summary
- Prioritize by severity/impact
- Group related items
- Add concrete action items
- Include metrics/context

---

### Type 20: Batch Processing

**Description:** Efficiently handle multiple items naraz

**When to consider:**
- Users repeatedly run skill on individual items
- Processing individual items is inefficient
- Many items share context/setup
- Aggregation across items is valuable

**Assessment questions:**
- Czy skill typically used on one vs many items?
- Co można share across items (setup, context)?
- Czy batch mode would add value?

**Examples:**

**Add batch capability:**

```markdown
## Modes (NEW)

**Single Item Mode:**
Process one file/item

**Batch Mode:**
Process multiple items efficiently:

1. **Discover items:**
   `find . -name "*.py"` or user provides list

2. **Shared setup (once):**
   - Load configuration
   - Initialize tools
   - Set up context

3. **Process all items:**
   - Reuse shared setup
   - Process in parallel where possible
   - Collect results

4. **Aggregate results:**
   - Combined report
   - Cross-item analysis
   - Summary statistics

**Benefits:**
- 10x faster for many items
- Consistent context
- Aggregate insights
```

**Implementation approach:**
- Detect if multiple items provided
- Extract shared setup
- Process efficiently (parallel if possible)
- Aggregate results meaningfully
- Report both individual & aggregate

---

## Category 6: Documentation

### Type 21: More Examples

**Description:** Concrete use cases, templates, patterns

**When to consider:**
- Abstract instructions are unclear
- Users unsure how to start
- Common patterns not documented
- "Show don't tell" would help

**Assessment questions:**
- Czy current examples są sufficient?
- Czy cover common use cases?
- Czy są concrete enough?

**Examples:**

**Expand examples:**

```markdown
## Examples (EXPANDED)

### Example 1: Happy Path
**Input:** [concrete]
**Process:** [step by step]
**Output:** [exact result]

### Example 2: Edge Case A
**Scenario:** [specific situation]
**How to handle:** [concrete steps]
**Expected result:** [what happens]

### Example 3: Integration with Other Skill
**Context:** [when this applies]
**Combined workflow:** [how they work together]
**Output:** [combined result]

### Example 4: Common Mistake
**Wrong approach:** [what not to do]
**Why it fails:** [explanation]
**Correct approach:** [what to do instead]
**Result:** [success]
```

**Implementation approach:**
- Move detailed examples to `references/examples.md`
- Keep 2-3 in main SKILL.md
- Cover: happy path, edge cases, integrations
- Include anti-examples (common mistakes)
- Use real-world scenarios

---

### Type 22: Best Practices

**Description:** Guidance on optimal usage

**When to consider:**
- Multiple ways to use skill
- Some approaches better than others
- Common anti-patterns observed
- Optimization tips not obvious

**Assessment questions:**
- Co są common usage patterns?
- Co to optimal approach per scenario?
- Jakie anti-patterns users fall into?

**Examples:**

**Add best practices guide:**

```markdown
## Best Practices (NEW)

### When to Use This Skill

✅ **Good use cases:**
- [Specific scenario 1] - Why it's good fit
- [Specific scenario 2] - Benefits
- [Specific scenario 3] - Advantages

❌ **Not recommended for:**
- [Scenario A] - Use [other-skill] instead
- [Scenario B] - Manual approach better because...

### Optimization Tips

**For large projects:**
- Use batch mode
- Enable checkpoints
- Consider parallel processing

**For repeated use:**
- Let skill learn from history
- Set up smart defaults
- Create templates

**For collaboration:**
- Use machine-readable output
- Document decisions in report
- Share configuration

### Common Mistakes

**Mistake 1:** [Common error]
**Why it's wrong:** [Explanation]
**Better approach:** [Correct way]

[...more mistakes...]
```

**Implementation approach:**
- Add best practices section to SKILL.md or `references/best-practices.md`
- Document optimal patterns
- Include anti-patterns
- Context-specific guidance
- Link to examples

---

## Priority Assessment Framework

For każdego znalezionego upgrade, ocen:

### Value Score (1-10)

- **10:** Solves major pain point, dramatic improvement
- **7-9:** Significant value, clear benefits
- **4-6:** Moderate value, nice to have
- **1-3:** Minor value, marginal improvement

### Effort Score (1-10)

- **1-3:** Simple change, 1-2 sections
- **4-6:** Moderate change, multiple sections
- **7-10:** Major change, restructuring needed

### Priority Calculation

**High Priority:** Value ≥ 7 AND Effort ≤ 6
**Medium Priority:** Value ≥ 5 AND not High Priority
**Low Priority:** Value < 5 OR Effort > 8

### Final Selection

Present top 3-5 opportunities with highest Priority scores.
