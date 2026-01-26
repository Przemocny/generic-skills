# Quality Criteria for Skills

Comprehensive checklist do oceny jakości skilla.

---

## 🔴 CRITICAL ISSUES (Priority 1 - MUST FIX)

### 1. Time Estimates
**Rule:** NEVER include time estimates in skills.

**Why:** Per core guidelines - users should judge timing themselves, not be given predictions.

**Check for:**
- ❌ "5-10 minut", "15-45 minut"
- ❌ "This phase takes X minutes"
- ❌ "Should be done in..."
- ❌ Any time predictions

**Exceptions:** NONE. Remove all time estimates.

**How to find:**
```bash
grep -n "minut\|min\|hour\|godzin" SKILL.md
```

---

### 2. Excessive Length
**Rule:** SKILL.md powinien być zwięzły - szczegóły w references.

**Guidelines:**
- ✅ Target: 200-300 linii dla SKILL.md
- ⚠️ Warning: 300-400 linii (może być OK ale review)
- ❌ Too long: >400 linii (definitywnie trzeba skrócić)

**Why:** Długie skille są overwhelming. Main skill to workflow, szczegóły w references.

**Check:**
```bash
wc -l SKILL.md
```

**Common causes of excessive length:**
- Inline templates (move to references/template.md)
- All questions listed in workflow (move to references/questions.md)
- Detailed examples in phases (move to references/examples.md)
- Repeated information

**How to fix:** Move szczegóły do references/, zostaw w SKILL.md tylko:
- Overview i kluczowe zasady
- Phase-by-phase workflow (high level)
- Gdzie szukać szczegółów ("See references/X")
- Special cases (brief)
- Quality checklist i reminders

---

### 3. Missing Core Guidelines Compliance
**Rule:** Skill musi być zgodny z core guidelines.

**Check for:**
- Uses correct tools (WebSearch, WebFetch, nie bash grep/find)
- No overpromising features
- Clear trigger conditions in description
- Proper Polish/English mix

---

### 4. Broken Structure
**Rule:** Workflow musi być jasny i logiczny.

**Red flags:**
- Phases nie następują logicznie
- Unclear kiedy przejść do next phase
- Missing decision points
- Workflow jest circular bez escape

**Fix:** Przepisz workflow żeby był linear lub explicitly iterative.

---

### 5. Critical Functionality Missing
**Rule:** Skill musi robić to co obiecuje w description.

**Check:**
- Czy description matches actual workflow?
- Czy wszystkie triggers są covered?
- Czy output jest delivered?

---

## 🟡 QUALITY ISSUES (Priority 2 - SHOULD FIX)

### 6. Redundant Content
**Rule:** Nie powtarzaj informacji między SKILL.md a references.

**Check for:**
- Same questions w SKILL.md i w references/questions.md
- Same examples w multiple places
- Same guidelines repeated

**Fix:**
- SKILL.md: "Ask core questions (see references/questions.md)"
- References: Full question library

---

### 7. Overwhelming Options
**Rule:** Too many choices = paralysis.

**Check for:**
- >5 pytań w single phase bez prioritization
- Long lists of options bez guidance
- Everything is "important"

**Fix:**
- Prioritize: MUST vs OPTIONAL
- Group related items
- Provide defaults
- "Start with X, add Y if needed"

---

### 8. Unclear Phases
**Rule:** Każda phase powinna mieć jasny cel i outcome.

**Each phase needs:**
- **Cel:** What are you trying to achieve?
- **Kroki:** What to do?
- **Output:** What's the result?
- **Next:** When to move to next phase?

**Check:** Czy każda phase ma te elementy?

---

### 9. Poor Reference Organization
**Rule:** References powinny być helpful, not overwhelming.

**Guidelines:**
- ✅ 3-5 reference files is good
- ⚠️ 6-8 files might be too many
- ❌ >8 files is definitely too many

**Check file lengths:**
```bash
wc -l references/*.md
```

**Compare to other skills:**
```bash
wc -l ../critical-app-brief/references/*.md
```

**Typical reference structure:**
- questions-library.md or templates.md
- examples.md or patterns.md
- guidelines.md or criteria.md
- templates.md (if needed)

**Red flags:**
- Single reference file >600 linii (split it)
- Too many small files (<100 linii each - merge them)

---

### 10. Missing Examples
**Rule:** Concrete examples > abstract descriptions.

**Check for:**
- Workflow phases bez examples
- Guidelines bez "good/bad" examples
- Templates bez filled examples

**Fix:** Add examples to references/ showing:
- Good vs bad approaches
- Typical scenarios
- Edge cases handled well

---

### 11. Inconsistent Tone/Style
**Rule:** Skill powinien mieć consistent tone.

**Check for:**
- Mix formal/informal bez pattern
- Inconsistent use of English/Polish
- Switching between "you" and "user"

---

### 12. No Special Cases
**Rule:** Real-world usage ma edge cases.

**Check:** Czy skill addresses:
- What if user input is vague?
- What if task is too big/small?
- What if expected resources aren't available?
- What if user disagrees with recommendations?

**Should have:** Special Cases section covering common scenarios.

---

## 🟢 ENHANCEMENTS (Priority 3 - NICE TO HAVE)

### 13. Could Be More Actionable
**Check:**
- Are recommendations concrete?
- Are next steps clear?
- Could examples be more specific?

---

### 14. Missing Cross-References
**Check:**
- Does skill reference related skills?
- Are there skills that should hand off to this one?

---

### 15. Could Use Better Formatting
**Check:**
- Good use of **bold** for emphasis?
- Clear section headers?
- Code blocks where appropriate?
- Lists vs paragraphs?

---

## Comparison Metrics

**Good skill benchmarks (from critical-app-brief):**
- SKILL.md: ~280 linii
- References total: ~1500 linii
- References count: 3 files
- Largest reference: ~590 linii
- Clear 5-phase workflow
- Explicit quality checklist
- Strong "DO/DON'T" section

**Use for comparison:**
```bash
# Main skill length
wc -l SKILL.md

# References total
wc -l references/*.md

# Compare to benchmark skill
wc -l ../critical-app-brief/SKILL.md
wc -l ../critical-app-brief/references/*.md
```

---

## Quality Score

Rate skill on each criterion:
- ✅ Pass (no issues)
- ⚠️ Warning (minor issues)
- ❌ Fail (needs fix)

**Priority 1 (CRITICAL):**
- [ ] No time estimates
- [ ] SKILL.md reasonable length
- [ ] Core guidelines compliant
- [ ] Clear structure
- [ ] Delivers promised functionality

**Priority 2 (QUALITY):**
- [ ] No redundancy
- [ ] Not overwhelming
- [ ] Phases well-defined
- [ ] References organized
- [ ] Has examples
- [ ] Consistent style
- [ ] Special cases covered

**Priority 3 (ENHANCEMENTS):**
- [ ] Highly actionable
- [ ] Good cross-references
- [ ] Well formatted

**Scoring:**
- All P1 ✅ = Acceptable skill
- All P1 ✅ + Most P2 ✅ = Good skill
- All P1 ✅ + All P2 ✅ + Some P3 ✅ = Excellent skill

---

## Analysis Template

Use this format when presenting analysis:

```markdown
## Quality Analysis: [Skill Name]

**Metrics:**
- SKILL.md: [X] linii
- References: [Y] linii total ([N] files)
- Comparison: [benchmark skill] ma [Z] linii

**Priority 1 Issues (MUST FIX):**
1. ❌ [Issue] - [specific problem]
2. ❌ [Issue] - [specific problem]

**Priority 2 Issues (SHOULD FIX):**
1. ⚠️ [Issue] - [specific problem]
2. ⚠️ [Issue] - [specific problem]

**Priority 3 Opportunities (NICE TO HAVE):**
1. 💡 [Enhancement] - [how it helps]

**What's Good:**
✅ [Positive aspect]
✅ [Positive aspect]

**Overall Assessment:**
[High-level summary of skill quality]

**Recommended Action:**
[Minor fixes / Moderate refactor / Major overhaul / Rewrite]
```
