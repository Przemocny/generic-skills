# Refactoring Patterns

Konkretne wzorce jak poprawiać problemy w skillach.

---

## Pattern: Remove Time Estimates

**Problem:** Skill ma time estimates w phases.

**Before:**
```markdown
### Phase 1: Understanding (5-10 minut)
### Phase 2: Questions (10-15 minut)
### Phase 3: Research (15-45 minut)
```

**After:**
```markdown
### Phase 1: Understanding
### Phase 2: Questions
### Phase 3: Research
```

**Steps:**
1. Find all time estimates:
   ```bash
   grep -n "minut\|min\|hour" SKILL.md
   ```
2. Remove from phase headers
3. Remove from any descriptions
4. Verify no estimates remain

**Alternative (if you want to indicate effort):**
```markdown
### Phase 1: Understanding (brief)
### Phase 3: Research (comprehensive)
```

---

## Pattern: Extract Template to Reference

**Problem:** SKILL.md ma 100+ linii inline template.

**Before (SKILL.md):**
```markdown
### Phase 4: Create report

Use this template:

```markdown
# Report Template
[100+ lines of template]
```

Fill in each section...
```

**After (SKILL.md):**
```markdown
### Phase 4: Create report

**Choose template based on scope:**
- **Minimal:** Simple topics
- **Standard:** Most cases (default)
- **Extended:** Complex topics

**Full templates:** See `references/report-template.md`

**Steps:**
1. Choose appropriate template
2. Fill in systematically
3. Add analysis, don't just copy data
```

**After (references/report-template.md):**
```markdown
# Report Templates

## Minimal Template
[full template]

## Standard Template
[full template]

## Extended Template
[full template]

## Template Selection Guide
[guidance when to use which]
```

**Benefits:**
- SKILL.md: 376 → 237 linii (-37%)
- Template easier to maintain
- Multiple template options possible
- Clear separation of concerns

---

## Pattern: Consolidate Questions with Core/Optional

**Problem:** Phase 2 ma 15+ pytań listed inline.

**Before (SKILL.md):**
```markdown
### Phase 2: Questions

**1. Zakres:**
- Question 1
- Question 2
- Question 3

**2. Kierunek:**
- Question 4
- Question 5
- Question 6

[8 more questions...]
```

**After (SKILL.md):**
```markdown
### Phase 2: Questions

**CORE QUESTIONS (zawsze zadaj):**

1. **Zakres:** "Jak głęboko chcesz wejść w ten temat?"
2. **Kierunek:** "Czy są konkretne źródła które uwzględnić?"
3. **Kontekst:** "Jak zamierzasz użyć tych informacji?"
4. **Ograniczenia:** "Co mogę pominąć?"

**Potwierdzenie:**
"Rozumiem że chcesz [X] żeby [Y], skupiając się na [Z]. Zgadza się?"

**For additional questions:** Consult `references/question-templates.md` for:
- Specific research types
- Follow-up clarifications
- When scope is unclear
```

**After (references/question-templates.md):**
```markdown
# Question Templates

## CORE QUESTIONS (zawsze zadaj)

[4 core questions with explanations]

## OPTIONAL QUESTIONS (use when needed)

### 1. Zakres i głębokość (dodatkowe)
[Additional depth questions]

### 2. Specific use cases
[Context-specific questions]

[etc.]
```

**Benefits:**
- Clear prioritization (MUST vs OPTIONAL)
- Agent knows where to start
- Flexibility for complex cases
- SKILL.md stays focused

---

## Pattern: Add Iterative Workflow Note

**Problem:** Research/analysis skill jest presented jako linear ale should be iterative.

**Before:**
```markdown
## Workflow

### Phase 1: Understand
### Phase 2: Questions
### Phase 3: Research
### Phase 4: Report
```

**After:**
```markdown
## Workflow

**Important:** Research jest iteracyjny - możesz wracać do wcześniejszych phases gdy odkryjesz nowe informacje lub zrozumiesz że scope wymaga korekty.

### Phase 1: Understand
### Phase 2: Questions
### Phase 3: Research

**If scope is too wide:** Stop and ask user to narrow down. Don't waste time on unfocused research.

### Phase 4: Report
```

**Add to Special Cases:**
```markdown
### Discovering new questions podczas researchu
- To normalne - research jest iteracyjny
- Zapisz nowe pytania
- Zapytaj użytkownika czy poszerzyć scope czy zostać przy oryginalnym
```

**Benefits:**
- Sets correct expectations
- Agent feels empowered to iterate
- Handles real-world complexity

---

## Pattern: Add Explicit Phase Goals

**Problem:** Phases nie mają clear cel statements.

**Before:**
```markdown
### Phase 2: Ask questions

Ask the user clarifying questions about the topic.
```

**After:**
```markdown
### Phase 2: Questions

**Cel:** Zawęzić zakres i kierunek researchu poprzez kluczowe pytania.

**CORE QUESTIONS:**
[...]

**Output:** Clear understanding of scope, sources to check, and success criteria.
```

**Template:**
```markdown
### Phase X: [Name]

**Cel:** [What you're trying to achieve - outcome focused]

**Steps:**
[What to do]

**Output:** [What you produce / what you know after this phase]
```

**Benefits:**
- Clear success criteria
- Know when phase is done
- Better phase transitions

---

## Pattern: Split Monolithic Reference File

**Problem:** Single reference file >600 linii z multiple topics.

**Before:**
```
references/
  everything.md (800 linii)
    - Questions
    - Examples
    - Guidelines
    - Templates
    - Tips
```

**After:**
```
references/
  question-templates.md (190 linii)
  examples.md (200 linii)
  guidelines.md (180 linii)
  templates.md (230 linii)
```

**Steps:**

1. **Identify logical sections** w monolithic file:
   ```bash
   grep "^## " references/everything.md
   ```

2. **Group related sections:**
   - Questions → question-templates.md
   - Examples → examples.md
   - Guidelines/Criteria → guidelines.md
   - Templates → templates.md

3. **Create new files** - copy relevant sections

4. **Update SKILL.md references:**
   ```markdown
   Before: "See references/everything.md"
   After: "See references/question-templates.md for questions"
   ```

5. **Delete old monolithic file**

6. **Verify:**
   ```bash
   wc -l references/*.md
   # Each file <500 linii?
   ```

**Benefits:**
- Easier to find specific info
- Better maintainability
- Focused content

---

## Pattern: Merge Reference Sprawl

**Problem:** 10+ tiny reference files.

**Before:**
```
references/
  questions-tech.md (60 linii)
  questions-business.md (55 linii)
  questions-comparison.md (50 linii)
  examples-tech.md (40 linii)
  examples-business.md (45 linii)
  [5 more small files...]
```

**After:**
```
references/
  question-templates.md (180 linii)
    - Core questions
    - Tech questions
    - Business questions
    - Comparison questions
  examples.md (150 linii)
    - Tech examples
    - Business examples
```

**Steps:**

1. **Group by purpose:**
   - All question files → one questions file
   - All example files → one examples file

2. **Use section headers** for organization:
   ```markdown
   # Question Templates

   ## Core Questions
   [...]

   ## Tech Questions
   [...]

   ## Business Questions
   [...]
   ```

3. **Update SKILL.md:**
   - Single reference instead of multiple

4. **Delete old files**

**Benefits:**
- Fewer files to manage
- Easier navigation
- Related content together

---

## Pattern: Add WebSearch Integration

**Problem:** Research skill używa tylko WebFetch, pomija WebSearch.

**Before:**
```markdown
### Phase 3: Research

**1. Identify sources:**
- Official documentation
- Expert blogs
- Community resources

**2. Extract information (WebFetch):**
For each source, use WebFetch...
```

**After:**
```markdown
### Phase 3: Research

**1. Find sources (WebSearch):**

Start by using **WebSearch** to identify the best sources:
- Search: "[topic] 2025" for current information
- Search: "[topic] best practices" for expert content
- Search: "[topic] [specific aspect]" to narrow down
- Identify: Official docs, expert blogs, case studies

**2. Prioritize sources (Tier system):**
[Tier 1-5 from source-evaluation.md]

**3. Extract information (WebFetch):**
For each source, use WebFetch with specific prompts...
```

**Benefits:**
- Uses right tool for discovery
- WebSearch better for finding sources
- WebFetch better for extracting from known URLs
- More efficient workflow

---

## Pattern: Add Scope Clarification

**Problem:** Skill overlaps with another bez clear boundary.

**Before (description):**
```markdown
description: Przeprowadza research poprzez analizę źródeł.
```

**After (description):**
```markdown
description: Przeprowadza research internetowy poprzez analizę wielu źródeł. Gromadzi informacje z internetu (nie codebase), identyfikuje wzorce i zapisuje wyniki.
```

**Add to Overview:**
```markdown
## Overview

[...]

**Important:** Ten skill jest dla researchu INTERNETOWEGO.
- Use this: Gdy potrzebujesz informacji z internetu (dokumentacja, best practices, porównania)
- NOT this: Gdy potrzebujesz explore codebase (użyj Explore agent)
```

**Benefits:**
- Clear when to use this vs other skills
- Prevents confusion
- Better skill selection

---

## Pattern: Create Template Selection Guide

**Problem:** Multiple templates bez guidance kiedy którego użyć.

**Before:**
```markdown
### Phase 4: Create report

Here are templates:
[Template A]
[Template B]
[Template C]
```

**After:**
```markdown
### Phase 4: Create report

**Choose template based on scope:**

**Minimal (simple topics):**
- Single focused question
- Quick overview needed
- 3-5 sources

**Standard (most cases):**
- Multiple aspects
- Best practices needed
- 5-8 sources
- Decision-making context

**Extended (complex topics):**
- Multi-faceted topic
- Critical decisions
- Comprehensive analysis
- 8+ sources

**Full templates:** See `references/report-template.md`
```

**In references/report-template.md:**
```markdown
# Report Templates

## Minimal Template
[template]

## Standard Template
[template]

## Extended Template
[template]

## Template Selection Guide

**Use Minimal when:**
- [criteria]

**Use Standard when:**
- [criteria]

**Use Extended when:**
- [criteria]
```

**Benefits:**
- Clear decision criteria
- Avoids over/under-engineering
- Flexible for different scopes

---

## Pattern: Improve Quality Checklist

**Problem:** Vague checklist items.

**Before:**
```markdown
✅ Research is complete
✅ Report is good
✅ User will be happy
```

**After:**
```markdown
✅ **Wystarczająco źródeł:** Minimum 5-8 różnych źródeł
✅ **Cross-reference:** Informacje potwierdzone z wielu źródeł
✅ **Analiza, nie tylko kolekcja:** Wyciągnąłeś wnioski i wzorce
✅ **Praktyczne rekomendacje:** Konkretne, actionable next steps
✅ **Wszystkie źródła udokumentowane:** Każde twierdzenie ma referencję
```

**Pattern:**
- ✅ **[Category]:** [Specific, verifiable criterion]

**Good checklist items:**
- Include metrics ("minimum 5-8")
- Verifiable (can check yes/no)
- Specific ("documented" not "good")
- Outcome-focused

---

## Pattern: Add Special Cases Section

**Problem:** Skill assumes happy path tylko.

**Add section:**
```markdown
## Special Cases

### [Scenario 1: common problem]
- Symptom: [how to recognize]
- Action: [what to do]
- Example response: [concrete example]

### [Scenario 2: edge case]
[...]

### [Scenario 3: user disagreement]
[...]
```

**Common scenarios to cover:**
- User input is vague/unclear
- Task scope too wide/narrow
- Expected resources unavailable
- User disagrees with recommendations
- Discovering issues mid-workflow

**Benefits:**
- Handles real-world usage
- Agent knows what to do when stuck
- Better user experience

---

## Refactoring Workflow

**Standard approach:**

1. **Identify issue** (use common-issues.md)
2. **Select pattern** (this file)
3. **Apply pattern:**
   - Make changes
   - Test result
   - Verify improvement
4. **Update references** if needed
5. **Check metrics:**
   ```bash
   wc -l SKILL.md
   # Shorter? ✅
   ```

**Atomic changes:**
- Fix one issue at a time
- Verify after each change
- Don't break other things

**Testing:**
- SKILL.md syntax OK
- References exist
- Links work
- Grep tests pass

---

## Before/After Examples

### Example 1: general-research refactoring

**Before:**
- SKILL.md: 376 linii
- No WebSearch usage
- Time estimates everywhere
- Template inline (116 linii)
- All questions inline

**After:**
- SKILL.md: 237 linii (-37%)
- WebSearch as primary discovery tool
- Zero time estimates
- Template in references/report-template.md
- 4 core questions, rest in references

**Patterns applied:**
1. Remove Time Estimates
2. Extract Template to Reference
3. Consolidate Questions (Core/Optional)
4. Add WebSearch Integration
5. Add Iterative Note

---

## Quick Wins vs Time Sinks

**Quick wins (do these first):**
- ✅ Remove time estimates (find & replace)
- ✅ Add explicit phase goals
- ✅ Create template selection guide
- ✅ Add iterative workflow note
- ✅ Improve quality checklist items

**Medium effort:**
- ⚠️ Extract template to reference
- ⚠️ Consolidate questions with core/optional
- ⚠️ Add WebSearch integration
- ⚠️ Split monolithic file

**Time sinks (only if necessary):**
- ⚠️⚠️ Restructure entire workflow
- ⚠️⚠️ Write examples from scratch
- ⚠️⚠️ Complete content rewrite

**Prioritize:**
1. Fix P1 issues first (critical)
2. Quick wins from P2
3. Medium effort P2
4. P3 only if time

---

## Verification Checklist

After applying patterns, verify:

```bash
# No time estimates
grep -n "minut\|hour" SKILL.md
# Should return nothing

# Reasonable length
wc -l SKILL.md
# <300 linii dla most skills

# References exist
ls references/
# 3-5 files, each <500 linii

# Quality improved
# Run through quality-criteria.md checklist
```

**Manual checks:**
- Workflow makes sense
- Phase goals are clear
- Examples are helpful
- Quality checklist is specific
- Special cases covered
