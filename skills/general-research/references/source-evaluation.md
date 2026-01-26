# Source Evaluation Guide

Jak oceniać wiarygodność i wartość źródeł podczas researchu.

## Hierarchia wiarygodności źródeł

### Tier 1: Najbardziej wiarygodne ⭐⭐⭐
**Oficjalna dokumentacja**
- Dokumentacja producenta/maintainera
- Official API references
- Official guides i tutorials

**Kiedy używać:**
- Dla informacji o features, capabilities, limitations
- Technical specifications
- Official recommendations

**Red flags:**
- Marketing language zamiast technical details
- Brak technical depth
- Nieaktualna (sprawdź version)

---

### Tier 2: Bardzo wiarygodne ⭐⭐
**Recognized experts**
- Blogi known industry experts
- Technical blogs firm (Stripe, Netflix, Uber engineering blogs)
- Conference talks i presentations

**Kiedy używać:**
- Best practices z doświadczenia
- Real-world case studies
- Architectural decisions i trade-offs

**Jak ocenić:**
- Kim jest autor? Jakie ma credentials?
- Czy pracuje w renomowanej firmie?
- Czy inni eksperci go cytują?
- Jak aktywny jest w community?

---

### Tier 3: Wiarygodne ⭐
**Community resources**
- StackOverflow (high-voted answers)
- GitHub discussions i issues
- Reddit (technical subreddits)
- Dev.to, Medium (verified authors)

**Kiedy używać:**
- Practical solutions do specific problems
- Common pitfalls i troubleshooting
- Community consensus

**Jak ocenić:**
- Ile upvotes/reactions?
- Czy odpowiedź jest szczegółowa i dobrze wyjaśniona?
- Czy inni potwierdzają w komentarzach?
- Jak dawno została napisana?

---

### Tier 4: Używaj ostrożnie ⚠️
**General content platforms**
- Medium (unverified authors)
- Personal blogs (unknown authors)
- Tutorial sites (quality varies)
- YouTube tutorials

**Kiedy używać:**
- Gdy brak lepszych źródeł
- Dla alternative perspectives
- Quick tutorials (ale verify!)

**Red flags:**
- Brak author credentials
- Copy-paste content
- Outdated information
- No code examples or shallow explanations

---

### Tier 5: Unikaj ❌
**Niskiej jakości źródła**
- Content farms
- AI-generated articles bez verification
- Clickbait titles
- Sites z plagiarized content

**Jak rozpoznać:**
- Generic, shallow content
- Brak konkretnych examples
- Gramatyczne błędy
- Ads everywhere
- Brak author information

---

## Kryteria oceny źródła

### 1. Authority (Autorytet)
**Pytania:**
- Kim jest autor?
- Jakie ma kwalifikacje w tym obszarze?
- Czy jest recognized expert?
- Czy firma/organizacja jest wiarygodna?

**Scoring:**
- ✅ Znany ekspert / Official source → High authority
- ⚠️ Experienced developer / Known company → Medium authority
- ❌ Unknown author / No credentials → Low authority

---

### 2. Accuracy (Dokładność)
**Pytania:**
- Czy informacje są precyzyjne i szczegółowe?
- Czy zawiera code examples które działają?
- Czy twierdza są wsparte dowodami/referencjami?
- Czy widzę technical depth czy surface-level info?

**Scoring:**
- ✅ Detailed, with examples, references → High accuracy
- ⚠️ Generally correct but lacks depth → Medium accuracy
- ❌ Vague, no examples, questionable claims → Low accuracy

---

### 3. Currency (Aktualność)
**Pytania:**
- Kiedy została opublikowana?
- Czy informacje są aktualne dla current versions?
- Czy autor aktualizuje content?

**Scoring (dla tech topics):**
- ✅ <6 months old OR recently updated → Current
- ⚠️ 6-18 months old, check if still relevant → Possibly outdated
- ❌ >18 months old, tech moves fast → Likely outdated

**Note:** Dla fundamental concepts, wiek ma mniejsze znaczenie.

---

### 4. Coverage (Zakres)
**Pytania:**
- Czy topic jest comprehensive covered?
- Czy pokazuje pros AND cons?
- Czy omawia edge cases i limitations?
- Czy jest balanced czy biased?

**Scoring:**
- ✅ Comprehensive, balanced, shows trade-offs → Good coverage
- ⚠️ Covers main points but misses details → Partial coverage
- ❌ Shallow, one-sided, missing important aspects → Poor coverage

---

### 5. Verification (Weryfikacja)
**Pytania:**
- Czy inne źródła potwierdzają te informacje?
- Czy widzę consensus czy outlier opinion?
- Czy mogę zweryfikować claims?

**Scoring:**
- ✅ Multiple sources confirm → Verified
- ⚠️ Some sources confirm, some differ → Needs more research
- ❌ Contradicts other sources, no confirmation → Unverified

---

## Source types i ich użycie

### Official Documentation
**Strengths:**
- Most accurate for features/APIs
- Up-to-date (usually)
- Authoritative

**Weaknesses:**
- May lack real-world examples
- Sometimes incomplete
- May not cover edge cases

**Best for:**
- What can the tool do?
- How to use specific features?
- API references

---

### Engineering Blogs (Stripe, Netflix, etc.)
**Strengths:**
- Real-world experience
- Shows trade-offs and decisions
- Deep technical insights

**Weaknesses:**
- Specific to their context
- May not apply to all use cases

**Best for:**
- How do they solve [problem]?
- What are architectural patterns?
- What are production lessons?

---

### StackOverflow
**Strengths:**
- Practical solutions
- Community validated (votes)
- Specific problems solved

**Weaknesses:**
- May be outdated
- Context-specific solutions
- Quality varies

**Best for:**
- How to solve [specific error]?
- What's the best way to [X]?
- Common pitfalls

---

### GitHub Issues/Discussions
**Strengths:**
- Real problems and solutions
- Direct from maintainers
- Up-to-date

**Weaknesses:**
- Can be very technical
- May be unresolved issues
- Requires context

**Best for:**
- Known bugs/limitations
- Workarounds
- Feature requests and roadmap

---

### Personal Blogs
**Strengths:**
- Personal experience and insights
- Often detailed tutorials
- Different perspectives

**Weaknesses:**
- Quality varies wildly
- May be biased or incorrect
- Often outdated

**Best for:**
- Tutorials and guides
- Personal opinions
- Alternative approaches

**Must verify:** Cross-reference with other sources!

---

## Red flags w źródłach

### 🚩 Content quality red flags
- ❌ Clickbait titles: "This ONE trick will..."
- ❌ No code examples for technical topics
- ❌ Vague descriptions: "it's very good", "works great"
- ❌ No mention of trade-offs or limitations
- ❌ Copy-paste from documentation bez dodatkowej wartości
- ❌ Grammatical errors and poor writing

### 🚩 Authority red flags
- ❌ No author information
- ❌ Author has no verifiable expertise
- ❌ Site looks like content farm
- ❌ No "About" page or company info

### 🚩 Currency red flags
- ❌ References old versions of tools
- ❌ Last updated years ago
- ❌ Uses deprecated APIs/methods
- ❌ Comments say "this doesn't work anymore"

### 🚩 Bias red flags
- ❌ Only shows positives, no negatives
- ❌ Clearly marketing/promotional
- ❌ Dismisses alternatives without reason
- ❌ Affiliate links everywhere

---

## Best practices dla researchu

### Cross-reference everything
**Rule:** Nie ufaj pojedynczemu źródłu dla important claims.

**Process:**
1. Znajdź informację w źródle A
2. Szukaj potwierdzenia w źródle B (inny typ źródła)
3. Jeśli consensus → zaufaj
4. Jeśli rozbieżność → szukaj więcej lub zaznacz uncertainty

### Triangulacja źródeł
**Strategia:** Używaj różnych typów źródeł dla complete picture.

**Przykład dla "Best state management for React":**
1. Official docs → What are the options?
2. Expert blogs → What do experts recommend?
3. GitHub stars/activity → What's popular and maintained?
4. StackOverflow → What problems do people encounter?
5. Company blogs → What do they use in production?

### Check dates
**Rule:** Zawsze sprawdź publication/update date.

**Dla tech topics:**
- <6 months → Current, trust it
- 6-12 months → Check if still relevant
- 12-24 months → Verify with recent sources
- >24 months → Use with caution, likely outdated

### Verify code examples
**Rule:** Jeśli źródło zawiera code, sprawdź czy jest sensowny.

**Red flags:**
- Deprecated APIs
- Bad practices (security issues, anti-patterns)
- Incomplete code
- No explanation

### Note the context
**Rule:** Zawsze zapisuj context źródła.

**Important context:**
- Company size (startup vs. enterprise)
- Scale (1K users vs. 1M users)
- Use case (MVP vs. production)
- Year (tech changes fast)

---

## Workflow oceny źródła

```
1. Quick scan
   - Title, author, date
   - Does it look relevant?
   - Red flags visible?
   ↓
2. Authority check
   - Who wrote this?
   - What are their credentials?
   - Is source reputable?
   ↓
3. Content evaluation
   - Is it detailed/comprehensive?
   - Are there examples?
   - Is it balanced?
   ↓
4. Currency check
   - When was it published?
   - Is it up-to-date?
   ↓
5. Verification
   - Can I confirm this elsewhere?
   - What do other sources say?
   ↓
6. Use or skip
   - High quality → Use it
   - Medium quality → Use with cross-reference
   - Low quality → Skip or verify heavily
```

---

## Przykłady oceny

### Example 1: Official React docs
- **Authority:** ⭐⭐⭐ Official source
- **Accuracy:** ⭐⭐⭐ Precise, with examples
- **Currency:** ⭐⭐⭐ Regularly updated
- **Coverage:** ⭐⭐⭐ Comprehensive
- **Verdict:** Trust it, use as primary source

### Example 2: Dan Abramov's blog
- **Authority:** ⭐⭐⭐ React core team member
- **Accuracy:** ⭐⭐⭐ Deep technical insights
- **Currency:** ⭐⭐ Some old posts (check date)
- **Coverage:** ⭐⭐ Specific topics, deep but narrow
- **Verdict:** Trust for concepts, verify for current APIs

### Example 3: High-voted StackOverflow answer
- **Authority:** ⭐⭐ Depends on user reputation
- **Accuracy:** ⭐⭐ Community validated
- **Currency:** ⚠️ Check date, may be outdated
- **Coverage:** ⭐ Solves specific problem
- **Verdict:** Good for practical solutions, cross-reference for best practices

### Example 4: Random Medium article
- **Authority:** ⚠️ Unknown author
- **Accuracy:** ⚠️ Needs verification
- **Currency:** ⭐ Recent publication
- **Coverage:** ⚠️ Surface level
- **Verdict:** Use with caution, verify with better sources

---

## Summary: Quick decision tree

```
Is it official documentation?
├─ YES → Trust it (Tier 1)
└─ NO → Continue

Is author a recognized expert?
├─ YES → Likely reliable (Tier 2)
└─ NO → Continue

Is it from reputable company blog?
├─ YES → Reliable (Tier 2)
└─ NO → Continue

Is it high-voted community content?
├─ YES → Use with verification (Tier 3)
└─ NO → Continue

Is it recent and well-written?
├─ YES → Use with heavy verification (Tier 4)
└─ NO → Skip (Tier 5)
```
