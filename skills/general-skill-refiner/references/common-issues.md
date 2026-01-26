# Common Issues in Skills

Katalog typowych problemów w skillach i jak je rozpoznać.

---

## STRUCTURAL ISSUES

### Issue: "Template Bloat"
**Symptom:** SKILL.md zawiera full template inline (100+ linii template code).

**Example:**
```markdown
### Phase 4: Create report

Use this template:

```markdown
# Report Title
[200 lines of template]
```
```

**Why it's a problem:**
- Makes SKILL.md unnecessarily long
- Template belongs in references/
- Hard to maintain (update in two places)

**How to detect:**
```bash
grep -A 50 "```markdown" SKILL.md | head -100
```

**Fix:**
- Move template to `references/template.md`
- In SKILL.md: "See `references/template.md` for full structure"
- Show minimal example only

**Priority:** 🔴 P1 if template >50 linii, 🟡 P2 if >20 linii

---

### Issue: "Question Explosion"
**Symptom:** Phase 2 lists 15+ pytań inline bez prioritization.

**Example:**
```markdown
### Phase 2: Ask questions

Ask these questions:
1. Question 1
2. Question 2
...
15. Question 15
```

**Why it's a problem:**
- Overwhelming for agent
- No guidance on what's critical vs optional
- Makes workflow unclear

**How to detect:**
- Count questions in a single phase
- Look for numbered lists >10 items

**Fix:**
- Identify 3-4 CORE questions
- Move rest to `references/question-templates.md`
- Add guidance: "Start with core questions, use references for follow-ups"

**Priority:** 🟡 P2

---

### Issue: "No Clear Scope Boundary"
**Symptom:** Skill nie ma jasnego "out of scope".

**Example:**
- general-research skill bez clarification że to "internet research, not codebase"
- Skill overlaps z innym bez distinction

**Why it's a problem:**
- Agent nie wie kiedy użyć tego vs innego skilla
- Scope creep during execution
- Confusion for user

**How to detect:**
- Read description - czy mówi wyraźnie co jest in/out?
- Check for overlap z innymi skillami
- Look for "Explore vs Research" type ambiguities

**Fix:**
- Add clarification do description: "(not codebase)"
- Add section in Overview: "Use this skill for X, not Y"
- Cross-reference related skills

**Priority:** 🟡 P2

---

### Issue: "Redundant Phases"
**Symptom:** Multiple phases robią to samo lub mogłyby być merged.

**Example:**
```markdown
### Phase 1: Initial questions
Ask basic questions

### Phase 2: Follow-up questions
Ask more questions

### Phase 3: Final clarification
Ask remaining questions
```

**Why it's a problem:**
- Unnecessary complexity
- Unclear when phase ends
- Could be single "Question Phase"

**How to detect:**
- Look for phases z similar names
- Check if phases have distinct outcomes
- See if phases could logically merge

**Fix:**
- Merge related phases
- Create sub-steps within single phase
- Clear transition criteria

**Priority:** 🟡 P2

---

## CONTENT ISSUES

### Issue: "Everywhere Time Estimates"
**Symptom:** Skill ma time estimates w każdej phase.

**Example:**
```markdown
### Phase 1: Understanding (5-10 minut)
### Phase 2: Questions (10-15 minut)
### Phase 3: Analysis (20-30 minut)
```

**Why it's a problem:**
- Violates core guidelines
- Sets wrong expectations
- Tasks take as long as they take

**How to detect:**
```bash
grep -n "minut\|min\|hour\|godzin\|minute" SKILL.md
```

**Fix:**
- Remove ALL time estimates
- Focus on outcomes, not duration
- Use effort indicators if needed: "brief" vs "comprehensive"

**Priority:** 🔴 P1 (critical violation)

---

### Issue: "Vague Phase Goals"
**Symptom:** Phase nie ma clear "Cel:" statement.

**Example:**
```markdown
### Phase 2: Questions

Ask the user questions.
```

**Why it's a problem:**
- Unclear what you're trying to achieve
- Hard to know when phase is complete
- No clear success criteria

**How to detect:**
- Look for phases without "Cel:" or "Goal:"
- Phases without clear outcome statement

**Fix:**
- Add explicit goal: "**Cel:** Zawęzić zakres researchu do actionable scope"
- Add outcome: "**Output:** Clear list of 3-4 core requirements"

**Priority:** 🟡 P2

---

### Issue: "Example Drought"
**Symptom:** Skill opisuje workflow ale bez concrete examples.

**Example:**
```markdown
Ask clarifying questions to understand scope.
[No examples of good vs bad questions]
```

**Why it's a problem:**
- Abstract descriptions are hard to follow
- No guidance on what "good" looks like
- Missing anti-patterns

**How to detect:**
- Search for "Example:" in skill - ile jest?
- Look for "Good:" / "Bad:" comparisons
- Check references for examples

**Fix:**
- Add examples to references/
- Show good vs bad in workflow
- Provide filled templates

**Priority:** 🟡 P2 jeśli skill jest complex

---

### Issue: "Analysis Without Action"
**Symptom:** Skill analizuje ale nie daje recommendations.

**Example:**
```markdown
### Phase 3: Analyze code
Identify patterns and issues.

[No guidance on what to DO with findings]
```

**Why it's a problem:**
- Leaves user hanging
- Analysis bez action = wasted effort
- Unclear value

**Fix:**
- Add clear recommendation section
- Include "so what?" after findings
- Provide actionable next steps

**Priority:** 🟡 P2

---

## ORGANIZATION ISSUES

### Issue: "Monolithic References"
**Symptom:** Single reference file >600 linii z multiple unrelated topics.

**Example:**
```
references/
  everything.md (800 linii)
```

**Why it's a problem:**
- Hard to find specific info
- Maintenance nightmare
- Overwhelming to load

**How to detect:**
```bash
wc -l references/*.md
# Look for files >600 linii
```

**Fix:**
- Split by logical topics
- Create focused files:
  - questions-library.md
  - examples.md
  - guidelines.md
- Each file <500 linii ideal

**Priority:** 🟡 P2 if >600 linii, 🔴 P1 if >1000 linii

---

### Issue: "Reference Sprawl"
**Symptom:** 10+ small reference files.

**Example:**
```
references/
  questions-1.md (50 linii)
  questions-2.md (40 linii)
  questions-3.md (35 linii)
  ...
  questions-10.md (45 linii)
```

**Why it's a problem:**
- Too many files to navigate
- Unclear where to look
- Logical grouping broken

**How to detect:**
```bash
ls references/ | wc -l
# >8 files is suspicious
```

**Fix:**
- Merge related files
- Target 3-5 reference files
- Each serves clear purpose

**Priority:** 🟡 P2

---

### Issue: "Orphaned References"
**Symptom:** References nie są referenced w SKILL.md.

**Example:**
- `references/advanced-techniques.md` exists
- SKILL.md never mentions it

**Why it's a problem:**
- Dead code
- Unclear if still relevant
- Maintenance burden

**How to detect:**
```bash
# For each reference file:
grep -l "references/advanced-techniques.md" SKILL.md
# No output = orphaned
```

**Fix:**
- Either reference it in SKILL.md
- Or delete it if outdated

**Priority:** 🟢 P3 (cleanup)

---

## WORKFLOW ISSUES

### Issue: "Linear When Should Be Iterative"
**Symptom:** Research/analysis skill ma linear phases ale real workflow jest iterative.

**Example:**
```markdown
Phase 1 → Phase 2 → Phase 3 → Done
[No way to go back when you discover new info]
```

**Why it's a problem:**
- Real work isn't linear
- Need to adapt based on findings
- Agent feels constrained

**How to detect:**
- Check if skill involves discovery/research
- Look for "if X then go back to phase Y"
- Missing iteration guidance

**Fix:**
- Add note: "Research is iterative - możesz wracać do earlier phases"
- Add decision points
- Special case: "If scope too wide, return to Phase 2"

**Priority:** 🟡 P2 for research/analysis skills

---

### Issue: "Missing Exit Conditions"
**Symptom:** Workflow nie mówi kiedy stop.

**Example:**
```markdown
### Phase 3: Gather information
Keep gathering information from sources.
[When do I stop?]
```

**Why it's a problem:**
- Could go on forever
- No success criteria
- Unclear when "good enough"

**Fix:**
- Add explicit exit criteria
- Quality checklist
- "When you have X, move to next phase"

**Priority:** 🟡 P2

---

### Issue: "No Error Handling"
**Symptom:** Workflow assumes everything goes well.

**Example:**
```markdown
Phase 1: Ask questions
Phase 2: Do the work
Phase 3: Deliver results
[What if user doesn't answer questions? What if work fails?]
```

**Why it's a problem:**
- Real usage has problems
- Agent doesn't know what to do when stuck
- Poor user experience

**Fix:**
- Add Special Cases section
- Handle common failures
- "What if..." scenarios

**Priority:** 🟡 P2

---

## QUALITY CHECKLIST ISSUES

### Issue: "Weak Quality Checklist"
**Symptom:** Quality checklist jest vague lub incomplete.

**Example:**
```markdown
✅ Work is done
✅ User is happy
```

**Why it's a problem:**
- Not actionable
- Can't verify
- Doesn't ensure quality

**Good example:**
```markdown
✅ **Cross-referenced:** Minimum 5 sources, key claims verified
✅ **Sources documented:** Each claim has reference
✅ **Analysis added:** Not just data collection, insights extracted
```

**Fix:**
- Make each item specific and verifiable
- Use metrics where possible
- Focus on outcomes, not activities

**Priority:** 🟡 P2

---

## STYLE ISSUES

### Issue: "Inconsistent English/Polish Mix"
**Symptom:** No clear pattern gdzie używać English vs Polish.

**Why it's a problem:**
- Looks unprofessional
- Hard to maintain consistency
- Confusing for readers

**Pattern that works:**
- Polish: Instructions, explanations, descriptions
- English: Technical terms, tool names, proper nouns
- Code/commands: English

**Fix:**
- Review and standardize
- Użyj Polish dla narrative
- Keep technical terms in English

**Priority:** 🟢 P3 (polish)

---

### Issue: "Wall of Text"
**Symptom:** Long paragraphs bez formatting.

**Why it's a problem:**
- Hard to scan
- Key points buried
- Poor readability

**Fix:**
- Break into shorter paragraphs
- Use bullet lists
- **Bold** for key points
- Add section headers
- Use code blocks for commands

**Priority:** 🟢 P3 (polish)

---

## Issue Detection Workflow

**Systematic approach:**

1. **Metrics first:**
   ```bash
   wc -l SKILL.md references/*.md
   ```
   - SKILL.md >400? → Look for Template Bloat, Question Explosion
   - Any reference >600? → Monolithic Reference issue

2. **Guideline violations:**
   ```bash
   grep -n "minut\|hour" SKILL.md
   ```
   - Any matches? → Time Estimate issue (P1)

3. **Structure check:**
   - Count phases - >7? Might be too many
   - Each phase has clear Cel? If not → Vague Phase Goals
   - Any phases with similar names? → Redundant Phases

4. **Content check:**
   - Search for "Example:" - <3 found? → Example Drought
   - Search for "Special Case" - none? → No Error Handling
   - Quality checklist vague? → Weak Quality Checklist

5. **Organization check:**
   - Count reference files - >8? → Reference Sprawl
   - For each reference, grep SKILL.md - not found? → Orphaned Reference

6. **Cross-reference check:**
   - Is scope clear vs other skills? If not → No Clear Scope Boundary
   - Does workflow match description? If not → Scope mismatch

---

## Priority Summary

**Always check for:**
- 🔴 P1: Time estimates, excessive length (>400), broken structure
- 🟡 P2: Redundancy, overwhelming options, vague goals, missing examples
- 🟢 P3: Style issues, orphaned files, formatting

**Quick wins:**
- Remove time estimates (find & replace)
- Add explicit goals to phases
- Move templates to references
- Split monolithic files

**Time sinks:**
- Restructuring workflow
- Writing examples from scratch
- Major content reorganization
