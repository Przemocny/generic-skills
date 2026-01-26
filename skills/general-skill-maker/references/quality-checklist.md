# Comprehensive Quality Checklist for Skills

Use this checklist to verify skill quality before finalizing. Organized by priority.

---

## 🔴 Critical Quality (MUST PASS)

These are non-negotiable requirements. Skills failing these checks should not be released.

### 1. No Time Estimates

- [ ] SKILL.md contains zero time estimates
- [ ] References contain zero time estimates (except technical specs like API rate limits)
- [ ] No phrases like: "takes X minutes", "should be done in", "quick 30 second", etc.

**Why critical:** Core guideline - never predict how long tasks take

**How to check:**
```bash
grep -i "minut\|minute\|hour\|godzin\|takes.*time\|should take" SKILL.md references/*.md
```

---

### 2. SKILL.md Length

- [ ] SKILL.md is under 500 lines
- [ ] If over 400 lines, verified that all content is essential
- [ ] Detailed content moved to references/

**Why critical:** Long SKILL.md wastes tokens, overwhelming for agent

**How to check:**
```bash
wc -l SKILL.md
```

**Target:** 200-350 lines ideal, <500 acceptable

---

### 3. Valid YAML Frontmatter

- [ ] `name` field present (hyphen-case, max 64 chars, no leading/trailing hyphens)
- [ ] `description` field present (1-1024 chars, comprehensive)
- [ ] No other required fields missing
- [ ] YAML syntax valid (proper indentation, no tabs)

**How to check:** Try to parse frontmatter, verify name format

---

### 4. Description Quality

- [ ] Contains WHAT skill does (specific capabilities)
- [ ] Contains WHEN to use (triggering contexts)
- [ ] Includes specific KEYWORDS users naturally say
- [ ] Includes trigger phrases (quoted examples)
- [ ] Not too generic ("helps with X" → "does A, B, C when X")

**Example good description:**
```yaml
description: >
  Extracts text and tables from PDF files, fills PDF forms, and merges
  multiple PDFs. Use when working with PDF documents or when the user
  mentions PDFs, forms, or document extraction. Trigger on phrases like
  "extract PDF text", "fill PDF form", "merge PDFs".
```

---

### 5. Workflow Structure

- [ ] Workflow is clear and logical
- [ ] Steps follow logically (no circular loops without escape)
- [ ] Decision points are explicit
- [ ] Next steps are clear at each phase

**Why critical:** Broken workflow = unusable skill

---

### 6. Delivers Promised Functionality

- [ ] Description matches actual workflow
- [ ] All mentioned triggers are handled
- [ ] Expected output is delivered
- [ ] No gaps between promise and delivery

---

## 🟡 High Quality (SHOULD PASS)

Skills should pass most of these for good quality.

### 7. Progressive Disclosure

- [ ] SKILL.md = overview + core workflow
- [ ] Detailed content in references/
- [ ] Clear links from SKILL.md to references/
- [ ] References loaded on-demand (not all at once)

**Check:**
- Does SKILL.md say "See references/X for details"?
- Are references focused (one topic per file)?
- Are references sizes reasonable (<800 lines each)?

---

### 8. Structure Over Prose

- [ ] Uses numbered steps for procedures
- [ ] Uses bullet points for criteria/checklists
- [ ] Clear section headers
- [ ] Examples show input → output
- [ ] Explicit conditionals ("If X, then Y")
- [ ] Minimal wall-of-text paragraphs

**Bad example:**
```markdown
So basically what you want to do is look at the code and figure out
if there are issues and then maybe suggest some fixes...
```

**Good example:**
```markdown
## Code Review Process

1. Read changed files
2. Check for:
   - Security issues
   - Performance problems
3. For each issue:
   - Quote code
   - Explain problem
   - Suggest fix
```

---

### 9. Only Adds What LLM Doesn't Know

- [ ] No basic programming tutorials
- [ ] No explaining common concepts
- [ ] Focus on company-specific conventions
- [ ] Focus on proprietary schemas/APIs
- [ ] Focus on domain expertise

**Bad:**
```markdown
Python is a programming language. Functions use def keyword...
```

**Good:**
```markdown
## Company API Conventions

All endpoints use /api/v2/ prefix
Error responses include errorCode from ERROR_CODES.md
```

---

### 10. Includes Concrete Examples

- [ ] Examples show real usage patterns
- [ ] Examples include input AND output
- [ ] Examples cover common scenarios
- [ ] Examples show good vs bad (where applicable)

**Check:** Does skill have "Examples" or "Usage" section with actual examples?

---

### 11. No Redundancy

- [ ] No duplicate content between SKILL.md and references/
- [ ] No repeated information in multiple places
- [ ] Clear single source of truth for each piece of information

**Pattern:**
- SKILL.md: "Ask core questions (see references/questions.md)"
- references/questions.md: [Full question library]

---

### 12. References Well-Organized

- [ ] 3-5 reference files (not too many)
- [ ] Each reference focused on one topic
- [ ] No single reference >800 lines (split if needed)
- [ ] No many tiny files (<100 lines each - merge if possible)
- [ ] Clear names (what's inside is obvious)

**Typical structure:**
```
references/
├── patterns.md (~400 lines)
├── examples.md (~350 lines)
└── best-practices.md (~300 lines)
```

---

### 13. No Overwhelming Options

- [ ] Phases don't have >5 options without prioritization
- [ ] Defaults provided where appropriate
- [ ] Guidance on when to use what
- [ ] Not everything marked as "important"

**Fix:** Prioritize options (MUST vs OPTIONAL), group related items

---

### 14. Clear Phase Boundaries

For each workflow phase:
- [ ] Clear goal stated
- [ ] Steps to achieve goal listed
- [ ] Expected output defined
- [ ] Transition to next phase explicit

---

### 15. Portable (No Hardcoded Paths)

- [ ] No hardcoded absolute paths (`/Users/john/...`)
- [ ] Uses relative paths or ${baseDir}
- [ ] Scripts work across environments
- [ ] No user-specific assumptions

**Bad:** `/Users/john/projects/app/config.json`
**Good:** `config/config.json` or `${baseDir}/config.json`

---

### 16. Special Cases Covered

- [ ] Handles vague user input
- [ ] Addresses what if task too big/small
- [ ] Covers missing resources scenario
- [ ] Handles user disagreement
- [ ] Edge cases documented

---

### 17. Consistent Tone/Style

- [ ] Consistent formality level
- [ ] Consistent Polish/English usage
- [ ] Consistent voice (you vs user vs agent)
- [ ] Professional but approachable

---

### 18. Scripts Tested (if applicable)

- [ ] All scripts executable (chmod +x)
- [ ] Scripts run without errors
- [ ] Scripts handle errors gracefully
- [ ] Scripts have helpful error messages
- [ ] Dependencies documented

---

## 🟢 Excellence (NICE TO HAVE)

These elevate skill from good to excellent.

### 19. Highly Actionable

- [ ] Recommendations are concrete (not just "fix this")
- [ ] Next steps always clear
- [ ] Examples very specific

---

### 20. Cross-References

- [ ] References related skills when appropriate
- [ ] Hand-off to other skills where logical
- [ ] Mentions complementary skills

**Example:** "After creating skill, use skill-validator to verify quality"

---

### 21. Excellent Formatting

- [ ] Good use of **bold** for emphasis
- [ ] Code blocks for commands/code
- [ ] Lists for clarity
- [ ] Tables where appropriate
- [ ] Emojis for visual markers (✅ ❌ ⚠️ 💡)

---

### 22. Has Complete Examples

- [ ] End-to-end example showing full workflow
- [ ] Example with all variations covered
- [ ] Real-world scenarios, not toy examples

---

### 23. Security Considerations (if applicable)

- [ ] Credentials handling documented
- [ ] No secrets hardcoded
- [ ] Input validation mentioned
- [ ] Security best practices followed

---

### 24. Error Handling Patterns

- [ ] Clear error messages defined
- [ ] Rollback/recovery procedures
- [ ] What to do when things fail

---

### 25. Performance Considerations (if applicable)

- [ ] Token efficiency optimized
- [ ] Progressive disclosure maximized
- [ ] Scripts used where appropriate (vs inline code)
- [ ] Large references split appropriately

---

## Scoring Your Skill

**Count passed checks:**

**Critical (6 checks):**
- All 6 pass: ✅ Acceptable
- Any fail: ❌ Not releasable

**High Quality (12 checks):**
- 10-12 pass: ✅ Good quality
- 7-9 pass: ⚠️ Acceptable, could improve
- <7 pass: ❌ Needs work

**Excellence (7 checks):**
- 5-7 pass: ✅ Excellent skill
- 3-4 pass: ✅ Good skill
- 0-2 pass: ⚠️ Functional but basic

**Overall grades:**
- **Excellent:** All Critical + 10+ High Quality + 5+ Excellence
- **Good:** All Critical + 10+ High Quality
- **Acceptable:** All Critical + 7+ High Quality
- **Needs Work:** Missing Critical or <7 High Quality

---

## Quick Validation Commands

**Line counts:**
```bash
wc -l SKILL.md
wc -l references/*.md
```

**Search time estimates:**
```bash
grep -i "minut\|minute\|hour\|takes.*time" SKILL.md references/*.md
```

**Check frontmatter:**
```bash
head -10 SKILL.md
```

**Find hardcoded paths:**
```bash
grep -n "/Users/\|/home/\|C:\\\\" SKILL.md references/*.md scripts/*
```

**Check script permissions:**
```bash
ls -la scripts/
```

---

## Quality Gate Decision

After running checklist:

**Ship it:** All Critical pass + 10+ High Quality pass
**Needs minor fixes:** All Critical pass + 7-9 High Quality pass
**Needs refactoring:** Any Critical fail OR <7 High Quality pass
**Needs rewrite:** Multiple Critical fail + poor structure

---

## Common Quality Issues & Fixes

### Issue: SKILL.md too long (>500 lines)

**Fix:**
1. Identify sections with most detail
2. Move to references/ (e.g., questions → references/questions.md)
3. Replace with: "See references/X for details"
4. Keep only high-level overview in SKILL.md

---

### Issue: Missing examples

**Fix:**
1. Create references/examples.md
2. Add 3-5 concrete examples
3. Show input → output for each
4. Reference from SKILL.md

---

### Issue: Wall of text

**Fix:**
1. Break into sections with headers
2. Convert prose to numbered steps
3. Use bullet points for lists
4. Add code blocks for examples

---

### Issue: Too many options, overwhelming

**Fix:**
1. Categorize: REQUIRED vs OPTIONAL
2. Provide defaults
3. Group related options
4. Add "Most users start with X" guidance

---

### Issue: Unclear workflow

**Fix:**
1. Define phases explicitly (Phase 1, 2, 3...)
2. Each phase: Goal → Steps → Output → Next
3. Make transitions explicit
4. Add decision points ("If X then Phase A, else Phase B")

---

## Pre-Release Final Check

Before releasing skill to users:

- [ ] Run all validation commands
- [ ] Test skill manually (invoke and try to use)
- [ ] Verify agent loads skill when expected (trigger test)
- [ ] Check all file links work (references/ paths correct)
- [ ] Review examples work as shown
- [ ] Get feedback from one other person (if team skill)
- [ ] Document any known limitations

**Ready to ship!** 🚀
