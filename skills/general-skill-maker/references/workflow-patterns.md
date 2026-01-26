# Workflow Patterns for Multi-Step Skills

Patterns for designing skills with sequential processes, conditional logic, and complex workflows.

## When to Use This Reference

Load this when creating skills with:
- Multi-step processes with dependencies
- Conditional branching logic
- State management across steps
- Verification/validation workflows
- Sequential tasks that build on each other

## Core Workflow Patterns

### Pattern 1: Linear Sequential Workflow

**Use when:** Steps must execute in order, each depends on previous completion.

**Structure:**
```markdown
## Workflow

1. [Preparation step]
   - Verify prerequisites
   - Gather inputs

2. [Main operation step 1]
   - Execute core operation
   - Capture output

3. [Main operation step 2]
   - Use output from step 1
   - Transform/process

4. [Validation step]
   - Verify results
   - Check quality

5. [Finalization step]
   - Save results
   - Clean up
```

**Example - Code Review Workflow:**
```markdown
## Code Review Process

1. **Identify Changes**
   - Run: `git diff --name-only main...HEAD`
   - List all modified files

2. **Analyze Each File**
   - Read file content
   - Check against security checklist
   - Check against performance patterns
   - Check against style guidelines

3. **Categorize Issues**
   - 🔴 CRITICAL: Security vulnerabilities, breaking changes
   - 🟡 WARNING: Performance issues, code smells
   - 🟢 INFO: Style suggestions, minor improvements

4. **Generate Report**
   - Group issues by severity
   - Include file:line references
   - Provide fix suggestions

5. **Summary**
   - Count issues by severity
   - Highlight critical items requiring immediate attention
```

### Pattern 2: Conditional Branching Workflow

**Use when:** Different paths based on conditions, not all steps always execute.

**Structure:**
```markdown
## Workflow

1. [Analysis step]
   - Examine input
   - Determine type/category

2. **Branch based on result:**

   **If [condition A]:**
   - Execute pathway A steps
   - [A-specific operations]

   **If [condition B]:**
   - Execute pathway B steps
   - [B-specific operations]

   **Otherwise:**
   - Execute default pathway
   - [Default operations]

3. [Convergence step]
   - Common finalization for all paths
```

**Example - File Processing Workflow:**
```markdown
## File Processing Workflow

1. **Identify File Type**
   - Check file extension
   - Verify file exists and is readable

2. **Process Based on Type:**

   **If PDF:**
   - Extract text using `scripts/extract_pdf.py`
   - Parse for tables and forms
   - See references/pdf-processing.md for details

   **If DOCX:**
   - Extract text and formatting
   - Preserve tracked changes if present
   - See references/docx-processing.md for details

   **If Image (PNG/JPG):**
   - Run OCR if text extraction requested
   - Extract metadata
   - See references/image-processing.md for details

   **Otherwise:**
   - Treat as plain text
   - Read with standard file operations

3. **Post-Processing**
   - Validate extracted content
   - Format output consistently
   - Save to user-specified location
```

### Pattern 3: Iterative Processing Workflow

**Use when:** Operating on collections, repeating steps for each item.

**Structure:**
```markdown
## Workflow

1. [Preparation]
   - Identify collection of items
   - Set up processing context

2. **For each item in collection:**

   a. [Pre-process item]
      - Validate item
      - Load item data

   b. [Process item]
      - Execute core operation
      - Handle errors gracefully

   c. [Post-process item]
      - Save/store results
      - Log outcome

3. [Aggregation]
   - Combine results from all items
   - Generate summary report
```

**Example - Batch File Analysis:**
```markdown
## Batch File Analysis

1. **Setup**
   - Get list of files: `find . -name "*.py"`
   - Initialize results storage

2. **For each Python file:**

   a. **Validate File**
      - Check file is readable
      - Skip if file is in exclusion list

   b. **Analyze File**
      - Count lines of code
      - Identify functions and classes
      - Check for security patterns
      - Calculate complexity metrics

   c. **Store Results**
      - Add to results collection
      - Note any errors or warnings

3. **Generate Summary**
   - Total files analyzed
   - Summary statistics (LOC, functions, classes)
   - Files with issues
   - Export to JSON/CSV if requested
```

### Pattern 4: Verification/Validation Workflow

**Use when:** Each step must pass validation before proceeding, with rollback on failure.

**Structure:**
```markdown
## Workflow with Validation

1. [Step 1]
   - Execute operation
   - **Verify:** Check condition
   - **If failed:** Roll back and stop

2. [Step 2]
   - Execute operation
   - **Verify:** Check condition
   - **If failed:** Undo step 1, roll back and stop

3. [Step 3]
   - Execute operation
   - **Verify:** Check condition
   - **If failed:** Undo steps 1-2, roll back and stop

4. [Finalization]
   - All verifications passed
   - Commit changes
```

**Example - Deployment Workflow:**
```markdown
## Safe Deployment Process

1. **Run Tests**
   - Execute: `npm test`
   - **Verify:** All tests pass
   - **If failed:** Stop deployment, report failing tests

2. **Build Application**
   - Execute: `npm run build`
   - **Verify:** Build succeeds, no errors
   - **If failed:** Stop deployment, show build errors

3. **Run Security Checks**
   - Execute: `npm audit`
   - **Verify:** No critical vulnerabilities
   - **If failed:** Stop deployment, list vulnerabilities

4. **Deploy to Staging**
   - Push to staging environment
   - **Verify:** Staging health check passes
   - **If failed:** Rollback, restore previous version

5. **Smoke Tests on Staging**
   - Run critical path tests
   - **Verify:** All smoke tests pass
   - **If failed:** Rollback staging

6. **Deploy to Production**
   - Push to production environment
   - Monitor for errors
   - Keep previous version for quick rollback

7. **Post-Deployment Verification**
   - Health checks
   - Monitor logs for 5 minutes
   - **If issues detected:** Immediate rollback
```

### Pattern 5: State Machine Workflow

**Use when:** Process has distinct states, transitions between states are well-defined.

**Structure:**
```markdown
## State Machine

**States:** [State A] → [State B] → [State C] → [Complete]

**Transitions:**

**From State A:**
- **Action X** → Moves to State B
- **Action Y** → Stays in State A (retry)
- **Action Z** → Moves to Error State

**From State B:**
- **Action M** → Moves to State C
- **Action N** → Returns to State A

[Continue for all states...]

## Workflow

1. Start in [Initial State]
2. Check current state
3. Determine available actions
4. Execute action
5. Transition to new state
6. Repeat until [Complete] or [Error]
```

**Example - Pull Request Workflow:**
```markdown
## PR State Machine

**States:** Draft → Ready for Review → Changes Requested → Approved → Merged

### State: Draft

**Available actions:**
- **Mark ready for review** → Move to "Ready for Review"
- **Close PR** → Move to "Closed"

### State: Ready for Review

**Available actions:**
- **Request changes** → Move to "Changes Requested"
- **Approve** → Move to "Approved"
- **Convert to draft** → Move to "Draft"

### State: Changes Requested

**Available actions:**
- **Push new commits** → Stay in "Changes Requested"
- **Re-request review** → Move to "Ready for Review"
- **Close PR** → Move to "Closed"

### State: Approved

**Available actions:**
- **Merge PR** → Move to "Merged"
- **Push new commits** → Move to "Ready for Review" (resets approvals)

### State: Merged

**Terminal state** - No further actions

## Workflow Implementation

1. **Check current state:** `gh pr view --json state`
2. **Determine available actions** based on state
3. **Execute chosen action**
4. **Verify state transition**
5. **Update tracking/notifications**
```

## Best Practices for Workflow Skills

### 1. Make Dependencies Explicit

**Clear about what each step needs:**
```markdown
## Step 2: Analyze Code

**Prerequisites:**
- Step 1 must be completed
- File list available from Step 1
- Working directory must be project root

**Inputs:**
- List of changed files
- Base branch for comparison

**Outputs:**
- Analysis results for each file
- Issues categorized by severity
```

### 2. Handle Errors Gracefully

**Define error handling for each step:**
```markdown
## Step 3: Run Tests

Execute: `npm test`

**Success criteria:**
- Exit code 0
- All tests pass
- No timeouts

**On failure:**
- Display failed test output
- Do NOT proceed to Step 4
- Ask user: "Tests failed. Fix issues and retry, or skip tests and continue?"
```

### 3. Provide Progress Indicators

**Let users know where they are:**
```markdown
## Workflow Progress

[1/5] ✅ Preparation complete
[2/5] ⏳ Analyzing files...
[3/5] ⏸️  Pending...
[4/5] ⏸️  Pending...
[5/5] ⏸️  Pending...
```

### 4. Enable Checkpoints and Resume

**For long workflows, allow resumption:**
```markdown
## Resume Points

**Checkpoint 1:** After file analysis (saves analysis.json)
**Checkpoint 2:** After validation (saves validation.json)
**Checkpoint 3:** After reporting (saves report.json)

**To resume:**
If checkpoint file exists, load state and continue from that step.
```

### 5. Document Exit Conditions

**Be clear about when workflow ends:**
```markdown
## Completion Criteria

Workflow is complete when:
- ✅ All files processed
- ✅ Report generated
- ✅ No critical errors encountered
- ✅ Results saved to specified location

Workflow exits early if:
- ❌ Critical error in any step
- ❌ User cancels operation
- ❌ Validation fails and user chooses not to continue
```

## Advanced Workflow Patterns

### Pattern 6: Parallel Processing with Merge

**Use when:** Multiple independent operations can run simultaneously, then merge.

```markdown
## Parallel Workflow

1. **Setup**
   - Identify parallel tasks

2. **Execute in Parallel:**

   **Task A:** [Independent operation A]
   **Task B:** [Independent operation B]
   **Task C:** [Independent operation C]

3. **Wait for All to Complete**
   - Check all tasks finished
   - Collect results from each

4. **Merge Results**
   - Combine outputs
   - Resolve conflicts if any
   - Generate unified result
```

### Pattern 7: Recursive Workflow

**Use when:** Same operation applied recursively (e.g., directory traversal).

```markdown
## Recursive Processing

**For directory D:**

1. **Process files in D:**
   - Apply operation to each file

2. **Process subdirectories:**
   - For each subdirectory S in D:
     - Recursively process S (go to step 1)

3. **Aggregate results:**
   - Combine results from files
   - Combine results from subdirectories
   - Return aggregated result
```

### Pattern 8: Pipeline Workflow

**Use when:** Data flows through transformation stages.

```markdown
## Pipeline

Input → Stage 1 → Stage 2 → Stage 3 → Output

**Stage 1: Extract**
- Input: Raw data
- Process: Parse and extract structured data
- Output: Structured data

**Stage 2: Transform**
- Input: Structured data from Stage 1
- Process: Clean, normalize, enrich
- Output: Transformed data

**Stage 3: Load**
- Input: Transformed data from Stage 2
- Process: Validate and save
- Output: Final result

**Each stage:**
- Validates its input
- Performs transformation
- Validates its output
- Passes to next stage
```

## Testing Workflows

**Test each pattern type:**

1. **Linear:** Execute all steps in order
2. **Conditional:** Test all branches
3. **Iterative:** Test with 0, 1, and many items
4. **Validation:** Test failure at each step
5. **State Machine:** Test all state transitions
6. **Parallel:** Test with varying completion times
7. **Recursive:** Test with deep nesting
8. **Pipeline:** Test with valid and invalid data

**Common test cases:**
- Happy path (all steps succeed)
- Early failure (step 1, 2, 3... fails)
- Empty inputs
- Maximum inputs (stress test)
- Invalid inputs at each stage
- Interruption/cancellation mid-workflow
