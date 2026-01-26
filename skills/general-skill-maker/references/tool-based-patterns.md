# Tool-Based Patterns for Skills

Patterns for designing skills around concrete tools, scripts, and executable operations.

## When to Use This Reference

Load this when creating skills that:
- Wrap specific tools or commands
- Execute scripts as core operations
- Provide deterministic, repeatable operations
- Focus on tool invocation rather than procedural knowledge
- Need low freedom (narrow path with guardrails)

## Core Tool-Based Patterns

### Pattern 1: Simple Script Executor

**Use when:** Skill primarily executes a single script with parameters.

**Structure:**
```markdown
## [Skill Name]

### Quick Start

```bash
scripts/[script-name].py [args]
```

### Usage

**Basic:**
```bash
scripts/[script-name].py --input file.txt
```

**With options:**
```bash
scripts/[script-name].py --input file.txt --format json --output result.json
```

### Parameters

- `--input` (required): Input file path
- `--output` (optional): Output file path (default: stdout)
- `--format` (optional): Output format: json|csv|txt (default: json)

### Examples

**Example 1:**
```bash
scripts/process.py --input data.csv --format json
```
Output: Processed data in JSON format

**Example 2:**
```bash
scripts/process.py --input data.csv --output results.json
```
Output: Results saved to results.json

### Error Messages

**"File not found"** - Input file doesn't exist, check path
**"Invalid format"** - Format must be json, csv, or txt
**"Permission denied"** - Output location not writable
```

**Example - PDF Rotation Skill:**
```markdown
## PDF Rotation

Rotate PDF pages using deterministic script.

### Quick Start

```bash
scripts/rotate_pdf.py input.pdf --angle 90
```

### Usage

**Rotate entire PDF:**
```bash
scripts/rotate_pdf.py document.pdf --angle 90 --output rotated.pdf
```

**Rotate specific pages:**
```bash
scripts/rotate_pdf.py document.pdf --angle 180 --pages 1,3,5-7
```

**Rotate clockwise vs counter-clockwise:**
```bash
scripts/rotate_pdf.py document.pdf --angle 90   # Clockwise
scripts/rotate_pdf.py document.pdf --angle -90  # Counter-clockwise
```

### Parameters

- `input` (required): PDF file to rotate
- `--angle` (required): Rotation angle (90, 180, 270, or negatives)
- `--output` (optional): Output filename (default: adds "_rotated" suffix)
- `--pages` (optional): Page numbers to rotate (default: all pages)
  - Format: "1,3,5" or "1-5" or "1,3-5,7"

### Examples

**Example 1: Rotate all pages 90° clockwise**
```bash
scripts/rotate_pdf.py report.pdf --angle 90
```
Output: report_rotated.pdf

**Example 2: Rotate pages 1-3 by 180°**
```bash
scripts/rotate_pdf.py report.pdf --angle 180 --pages 1-3 --output flipped.pdf
```
Output: flipped.pdf with pages 1-3 rotated

### Error Messages

**"Invalid angle"** - Must be 90, 180, 270, -90, -180, or -270
**"Invalid page range"** - Pages don't exist in PDF
**"Corrupted PDF"** - Input file is not a valid PDF
```

### Pattern 2: Multi-Script Workflow

**Use when:** Skill uses multiple scripts in sequence or conditionally.

**Structure:**
```markdown
## [Skill Name]

### Scripts

- `scripts/prepare.py` - Preprocessing
- `scripts/process.py` - Main operation
- `scripts/validate.py` - Validation
- `scripts/finalize.py` - Post-processing

### Workflow

**Step 1: Prepare**
```bash
scripts/prepare.py --input [source] --output prepared/
```
- Validates input
- Normalizes format
- Outputs to prepared/ directory

**Step 2: Process**
```bash
scripts/process.py --input prepared/ --output processed/
```
- Core processing logic
- Uses prepared data from Step 1

**Step 3: Validate**
```bash
scripts/validate.py --input processed/
```
- Checks output quality
- Exit code 0 = success, non-zero = failure

**Step 4: Finalize**
```bash
scripts/finalize.py --input processed/ --output final/
```
- Cleanup and formatting
- Generates final output

### Error Handling

If any step fails (non-zero exit code):
- Stop workflow
- Display error message from script
- Do not proceed to next step
```

**Example - Image Batch Processing:**
```markdown
## Batch Image Processing

Process multiple images with resize, watermark, and optimization.

### Scripts

- `scripts/validate_images.py` - Check images are valid
- `scripts/resize_images.py` - Resize to target dimensions
- `scripts/add_watermark.py` - Add watermark overlay
- `scripts/optimize_images.py` - Compress for web

### Workflow

**Step 1: Validate**
```bash
scripts/validate_images.py --dir images/ --formats jpg,png
```
- Verifies all files are valid images
- Checks formats are supported
- Exit 0 if all valid, exit 1 if any invalid

**Step 2: Resize**
```bash
scripts/resize_images.py --input images/ --output resized/ --width 800 --height 600
```
- Maintains aspect ratio
- Outputs to resized/ directory

**Step 3: Add Watermark**
```bash
scripts/add_watermark.py --input resized/ --output watermarked/ --logo assets/logo.png --position bottom-right
```
- Adds logo from assets/
- Position: top-left, top-right, bottom-left, bottom-right, center

**Step 4: Optimize**
```bash
scripts/optimize_images.py --input watermarked/ --output final/ --quality 85
```
- Compresses images
- Quality: 1-100 (85 recommended for web)

### Quick Run All Steps

```bash
# Run complete pipeline
scripts/validate_images.py --dir images/ && \
scripts/resize_images.py --input images/ --output resized/ --width 800 && \
scripts/add_watermark.py --input resized/ --output watermarked/ --logo assets/logo.png && \
scripts/optimize_images.py --input watermarked/ --output final/ --quality 85
```

### Parameters

**resize_images.py:**
- `--width`: Target width (maintains aspect ratio if height omitted)
- `--height`: Target height (maintains aspect ratio if width omitted)
- `--mode`: resize (default) | crop | fit

**add_watermark.py:**
- `--logo`: Path to watermark image
- `--position`: Watermark placement
- `--opacity`: 0.0-1.0 (default: 0.5)

**optimize_images.py:**
- `--quality`: JPEG quality 1-100 (default: 85)
- `--progressive`: Enable progressive JPEG (default: true)
```

### Pattern 3: Tool Wrapper with Context

**Use when:** Wrapping a tool but adding context, conventions, or guardrails.

**Structure:**
```markdown
## [Tool Name] Skill

### About

This skill wraps `[tool-name]` with [organization/project]-specific conventions and safety checks.

### Prerequisites

**Installation:**
```bash
[installation commands]
```

**Verify:**
```bash
[tool-name] --version
```

### Standard Operations

**Operation 1: [Name]**

Our convention:
```bash
[tool-name] [flags per our standards]
```

Standard `[tool-name]` would use:
```bash
[tool-name] [different flags]
```

**Why our convention:**
[Explanation of why we do it this way]

**Operation 2: [Name]**
[Similar pattern...]

### Safety Checks

Before running operations:
1. [Check 1]
2. [Check 2]
3. [Check 3]

If any check fails, stop and report issue.

### Examples

[Concrete examples following conventions]
```

**Example - Git Commit Skill:**
```markdown
## Git Commit with Conventions

Wrap git commit with company conventional commit standards.

### Prerequisites

Git installed and repository initialized.

### Commit Format

We use Conventional Commits specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only
- `style`: Code style (formatting, semicolons, etc.)
- `refactor`: Code refactoring
- `test`: Add or update tests
- `chore`: Maintenance (deps, config, etc.)

**Scope:** Component affected (auth, api, ui, etc.)

**Subject:** Imperative mood, no period, max 50 chars

### Workflow

**Step 1: Review Changes**
```bash
git status
git diff
```

**Step 2: Stage Files**
```bash
git add [files]
```
Only stage files related to this commit. Never `git add .` without reviewing.

**Step 3: Write Commit Message**

Follow format:
```bash
git commit -m "feat(auth): add JWT token refresh

Implement automatic token refresh when access token expires.
Adds refresh endpoint and client-side refresh logic.

Closes #123"
```

**Step 4: Verify Commit**
```bash
git log -1 --pretty=format:"%h - %s"
```

### Safety Checks

Before committing:
1. ✓ Tests pass: `npm test`
2. ✓ Linter passes: `npm run lint`
3. ✓ No console.log left in code
4. ✓ No TODO comments without ticket number
5. ✓ No sensitive data (API keys, credentials)

### Commit Message Validation

Script validates commit messages:
```bash
scripts/validate-commit-msg.sh
```

Checks:
- Type is valid (feat, fix, docs, etc.)
- Subject ≤ 50 characters
- Subject starts with lowercase
- Subject doesn't end with period
- Body lines ≤ 72 characters

### Examples

**Feature with scope:**
```bash
git commit -m "feat(api): add user profile endpoint"
```

**Bug fix with issue reference:**
```bash
git commit -m "fix(auth): prevent token expiry race condition

Adds mutex lock around token refresh to prevent multiple
simultaneous refresh requests.

Fixes #456"
```

**Documentation update:**
```bash
git commit -m "docs(readme): update installation instructions"
```

### Pre-commit Hook

Install hook to validate automatically:
```bash
cp scripts/pre-commit .git/hooks/
chmod +x .git/hooks/pre-commit
```

Hook runs:
- Linter
- Tests
- Commit message validation
```

### Pattern 4: Parameterized Script Templates

**Use when:** Script needs to be configured per use case but structure is same.

**Structure:**
```markdown
## [Skill Name]

### Configuration

Edit config before first use:
```bash
cp scripts/config.template.json scripts/config.json
# Edit scripts/config.json with your settings
```

**Config fields:**
- `field1`: [Description, example values]
- `field2`: [Description, example values]

### Usage

After configuration:
```bash
scripts/run.py --config scripts/config.json [additional-args]
```

### Configuration Examples

**Example config for use case A:**
```json
{
  "field1": "valueA",
  "field2": "valueA"
}
```

**Example config for use case B:**
```json
{
  "field1": "valueB",
  "field2": "valueB"
}
```
```

### Pattern 5: Script with Hooks

**Use when:** Need to validate or react to script execution.

**Structure:**
```markdown
## [Skill Name]

### Core Script

```bash
scripts/main_operation.py [args]
```

### Hooks

**Pre-execution validation:**
```bash
scripts/pre_check.sh
```
- Verifies prerequisites
- Exit 0 to continue, exit 2 to block

**Post-execution validation:**
```bash
scripts/post_check.sh
```
- Verifies output quality
- Exit 0 if valid, exit 2 if invalid

### Workflow with Hooks

1. Run pre-check
2. If passes, run main operation
3. Run post-check
4. If passes, commit results
5. If fails, rollback and report

### Hook Configuration (Claude Code)

```yaml
hooks:
  PreToolUse:
    - matcher: "Bash(scripts/main_operation.py*)"
      hooks:
        - type: command
          command: "scripts/pre_check.sh"
  PostToolUse:
    - matcher: "Bash(scripts/main_operation.py*)"
      hooks:
        - type: command
          command: "scripts/post_check.sh"
```
```

## Best Practices for Tool-Based Skills

### 1. Make Scripts Self-Contained

**Scripts should:**
- Have clear entry points
- Document dependencies in docstring
- Include usage message (`--help`)
- Return meaningful exit codes (0 = success, non-zero = error)
- Print errors to stderr, results to stdout

**Example script header:**
```python
#!/usr/bin/env python3
"""
Process CSV files and generate JSON output.

Dependencies:
- pandas >= 1.3.0
- numpy >= 1.20.0

Usage:
    process.py --input data.csv --output results.json
    process.py --input data.csv --format json  # Output to stdout

Exit codes:
    0 - Success
    1 - Invalid arguments
    2 - Input file not found
    3 - Processing error
"""
```

### 2. Provide Clear Error Messages

**Script errors should be actionable:**

❌ **Bad:**
```
Error: Process failed
```

✅ **Good:**
```
Error: Input file not found: data.csv
Check that file exists and path is correct.
Expected location: /path/to/data.csv
```

### 3. Test Scripts Thoroughly

**Test scripts with:**
- Valid inputs (happy path)
- Invalid inputs (error handling)
- Edge cases (empty files, very large files)
- Missing dependencies
- Insufficient permissions

**Include tests in scripts/ directory:**
```
scripts/
├── process.py
├── test_process.py
└── README.md  # How to run tests
```

### 4. Document Script Output Format

**SKILL.md should show exact output format:**

```markdown
## Output Format

**Success:**
```json
{
  "status": "success",
  "processed": 42,
  "output_file": "results.json"
}
```

**Error:**
```json
{
  "status": "error",
  "error_code": "FILE_NOT_FOUND",
  "message": "Input file not found: data.csv"
}
```
```

### 5. Use Consistent Parameter Naming

**Follow conventions:**
- `--input` / `-i` for input files
- `--output` / `-o` for output files
- `--format` / `-f` for format selection
- `--verbose` / `-v` for verbose output
- `--help` / `-h` for help
- `--version` for version info

### 6. Handle Paths Properly

**Scripts should:**
- Accept both relative and absolute paths
- Expand `~` for home directory
- Validate paths exist before processing
- Create output directories if needed
- Use `os.path.join()` or `pathlib.Path` for cross-platform compatibility

### 7. Log for Debugging

**Include optional verbose mode:**
```python
if args.verbose:
    logging.basicConfig(level=logging.DEBUG)
else:
    logging.basicConfig(level=logging.INFO)

logging.debug(f"Processing file: {input_file}")
logging.info(f"Processed {count} items")
```

### 8. Graceful Degradation

**If optional features unavailable, continue with core functionality:**
```python
try:
    import optional_library
    HAS_OPTIONAL = True
except ImportError:
    HAS_OPTIONAL = False
    logging.warning("optional_library not found, advanced features disabled")

# Later in code
if HAS_OPTIONAL:
    # Use optional features
else:
    # Fall back to basic implementation
```

## Script Organization

### Directory Structure

```
skills/skill-name/
├── SKILL.md
├── scripts/
│   ├── main.py           # Primary script
│   ├── helper.py         # Helper utilities
│   ├── validate.py       # Validation script
│   ├── config.template.json  # Config template
│   ├── requirements.txt  # Python dependencies
│   └── README.md         # Script documentation
├── references/
│   └── api-docs.md       # Additional documentation
└── assets/
    └── template.json     # Template files
```

### Script Documentation

**Each script should have:**

**README.md in scripts/:**
```markdown
# Scripts Documentation

## main.py
Primary processing script.

**Installation:**
```bash
pip install -r requirements.txt
```

**Usage:**
```bash
python main.py --input file.csv
```

## helper.py
Utility functions used by main.py. Not meant to be run directly.

## validate.py
Validates output from main.py.

**Usage:**
```bash
python validate.py --input results.json
```
```

## Testing Tool-Based Skills

**Test checklist:**

1. **Script execution:**
   - [ ] Scripts run without errors
   - [ ] Exit codes are correct
   - [ ] Output format matches documentation

2. **Parameter validation:**
   - [ ] Required parameters enforced
   - [ ] Invalid parameters rejected
   - [ ] Help message displays correctly

3. **Error handling:**
   - [ ] Missing files handled gracefully
   - [ ] Invalid input rejected with clear message
   - [ ] Permissions errors reported clearly

4. **Edge cases:**
   - [ ] Empty input files
   - [ ] Very large input files
   - [ ] Special characters in filenames
   - [ ] Concurrent executions (if applicable)

5. **Dependencies:**
   - [ ] Missing dependencies reported
   - [ ] Version requirements checked
   - [ ] Installation instructions accurate

6. **Output:**
   - [ ] Output files created successfully
   - [ ] Output format is valid
   - [ ] Output location respects parameter

## When NOT to Use Scripts

**Don't create scripts when:**
- Agent can easily write the code inline
- Logic varies significantly per use case
- Operation is simple and non-repetitive
- Flexibility is more important than consistency

**Use instructions instead:**
```markdown
## Process CSV File

Read CSV and extract column data:

```python
import pandas as pd
df = pd.read_csv('data.csv')
result = df['column_name'].tolist()
```

This is simple enough that agent can write it each time.
```

**Create script when:**
- Same operation repeated frequently
- Complex error handling required
- Performance optimization needed
- Deterministic output critical
- Agent keeps rewriting similar code
