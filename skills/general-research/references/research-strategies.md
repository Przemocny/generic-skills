# Research Strategies

Strategie i taktyki researchu dla różnych typów tematów i pytań.

## Typy researchu

### 1. Technology/Tool Research
**Cel:** Zrozumienie konkretnej technologii lub narzędzia.

**Strategia:**
1. **Official Documentation** (must)
   - Features, capabilities
   - Getting started guide
   - API reference
   - Best practices section

2. **GitHub Repository** (must)
   - README dla overview
   - Issues dla known problems
   - Discussions dla community insights
   - Stars/activity jako popularity indicator

3. **Expert Blogs** (2-3 sources)
   - Search: "[tech] best practices"
   - Search: "[tech] in production"
   - Look for: Company engineering blogs

4. **Community** (2-3 sources)
   - StackOverflow: "[tech]" tag, sort by votes
   - Reddit: r/programming, specific subreddits
   - Dev.to or Hashnode

5. **Comparison resources** (if applicable)
   - "[tech] vs [alternative]"
   - "Why we chose [tech]"

**WebFetch prompts:**
- Docs: "Extract key features, capabilities, limitations, and use cases"
- Blogs: "Summarize main benefits, drawbacks, and production experience"
- Comparisons: "Extract comparison criteria, pros/cons of each option"

---

### 2. Comparison Research
**Cel:** Porównanie kilku rozwiązań/narzędzi.

**Strategia:**
1. **Create comparison matrix** (features/criteria)
   - Performance
   - Ease of use
   - Ecosystem/community
   - Documentation quality
   - Cost
   - Learning curve
   - Maintenance/updates

2. **For each option, gather:**
   - Official docs → Features
   - GitHub → Stars, activity, issues
   - Expert opinions → Production experiences
   - Community → Common problems

3. **Look for:**
   - "[A] vs [B] vs [C]"
   - "Migrating from [A] to [B]" (tells you why)
   - "Why we chose [X]" case studies

4. **Cross-reference:**
   - What do multiple sources agree on?
   - Where are disagreements?
   - What context affects recommendations?

**WebFetch prompts:**
- "Compare [A] vs [B] in terms of features, performance, ease of use"
- "Extract pros and cons of [technology]"
- "What are the main differences between [A] and [B]"

---

### 3. Best Practices Research
**Cel:** Znalezienie proven practices dla konkretnego obszaru.

**Strategia:**
1. **Official guidelines** (if exist)
   - Framework/library docs
   - Security guidelines (OWASP, etc.)

2. **Company engineering blogs** (primary source)
   - Stripe, Netflix, Uber, Airbnb engineering blogs
   - Search: "[topic] best practices [company]"
   - Look for: Architecture decisions, lessons learned

3. **Expert articles**
   - Search: "[topic] best practices 2024"
   - Filter by recognized authors
   - Look for detailed explanations of "why"

4. **Anti-patterns** (equally important!)
   - Search: "[topic] common mistakes"
   - Search: "[topic] anti-patterns"
   - StackOverflow common issues

5. **Case studies**
   - Real-world implementations
   - What worked, what didn't
   - Lessons learned

**WebFetch prompts:**
- "Extract best practices and recommendations"
- "What are the dos and don'ts for [topic]"
- "Summarize lessons learned and key takeaways"

---

### 4. Problem-Solution Research
**Cel:** Znalezienie rozwiązania konkretnego problemu.

**Strategia:**
1. **StackOverflow** (first stop)
   - Search exact error message
   - Search problem description
   - Sort by votes
   - Check multiple answers

2. **GitHub Issues**
   - Search in relevant repos
   - Look for: Closed issues with solution
   - Look for: Workarounds

3. **Documentation**
   - Troubleshooting sections
   - Common issues
   - Migration guides

4. **Recent blog posts/articles**
   - Search: "how to solve [problem] 2024"
   - Personal blogs often have detailed solutions

5. **Community discussions**
   - Reddit, Discord, Slack channels
   - Often have recent issues discussed

**WebFetch prompts:**
- "Extract the solution and explain how it works"
- "What are the steps to fix [problem]"
- "Summarize the root cause and resolution"

---

### 5. Architectural Research
**Cel:** Zrozumienie architectural patterns i decisions.

**Strategia:**
1. **High-quality engineering blogs** (must)
   - How companies architect systems
   - Search: "[company] architecture [system]"
   - Look for: System design posts

2. **Architecture documentation**
   - Official architecture guides
   - Framework patterns
   - Cloud provider best practices (AWS Well-Architected, etc.)

3. **Conference talks**
   - InfoQ, QCon presentations
   - Company tech talks
   - Usually detailed and authoritative

4. **Books and long-form content**
   - Martin Fowler's blog
   - Architecture patterns resources

5. **Case studies**
   - "How [company] scales [system]"
   - Migration stories

**WebFetch prompts:**
- "Extract the architectural pattern and design decisions"
- "What are the key components and how do they interact"
- "Summarize the trade-offs and reasoning behind decisions"

---

### 6. Market/Business Research
**Cel:** Zrozumienie market landscape, trends, business aspects.

**Strategia:**
1. **Industry reports**
   - Gartner, Forrester (if available)
   - GitHub Octoverse
   - Stack Overflow surveys

2. **Market analysis**
   - Competitor websites
   - Product Hunt, G2 reviews
   - Pricing pages

3. **Trends and predictions**
   - Tech news sites (TechCrunch, The Verge)
   - Industry-specific publications
   - Conference keynotes

4. **User feedback**
   - Reviews (G2, Capterra)
   - Reddit discussions
   - Twitter/X sentiment

5. **Financial and growth data**
   - Company blogs (announcements)
   - Funding news
   - User numbers if public

**WebFetch prompts:**
- "Extract market trends and key insights"
- "Summarize competitive landscape and main players"
- "What are the growth indicators and market position"

---

### 7. Learning/Tutorial Research
**Cel:** Zgromadzenie learning resources i tutorials.

**Strategia:**
1. **Official tutorials** (always start here)
   - Getting started guides
   - Official courses

2. **High-quality tutorial platforms**
   - freeCodeCamp
   - Official documentation tutorials
   - University courses (MIT OCW, etc.)

3. **Video content**
   - Conference talks for concepts
   - Well-reviewed courses (Udemy, Pluralsight)
   - YouTube channels (Fireship, etc.)

4. **Interactive learning**
   - Official playgrounds
   - CodeSandbox examples
   - Interactive tutorials

5. **Learning paths**
   - Roadmap.sh
   - Community-curated learning paths

**WebFetch prompts:**
- "Extract learning path and key concepts to master"
- "What are the prerequisite skills and what order to learn"
- "Summarize the curriculum and main topics covered"

---

## Research tactics

### Tactic 1: Pyramida źródeł
**Start broad, go deep.**

```
Level 1: Overview (15 min)
- Official docs homepage
- Wikipedia/General overview
- Top Google result
→ Goal: Understand basics

Level 2: Depth (30 min)
- Official docs deep dive
- 2-3 expert blogs
- GitHub repo/issues
→ Goal: Understand details

Level 3: Edges (20 min)
- Common problems
- Edge cases
- Community discussions
→ Goal: Understand pitfalls

Level 4: Synthesis (15 min)
- Compare findings
- Identify patterns
- Create recommendations
→ Goal: Actionable insights
```

---

### Tactic 2: Multi-angle approach
**Patrz na temat z różnych perspektyw.**

```
Technical angle:
- How does it work?
- What are the internals?

Practical angle:
- How do I use it?
- What are best practices?

Experience angle:
- What do people say?
- What problems occur?

Business angle:
- What does it cost?
- Is it maintained?
- What's the ecosystem?
```

---

### Tactic 3: Chronological insight
**Zrozum ewolucję tematu.**

```
1. Current state (2024-2025)
   - What's the current recommendation?

2. Recent history (2022-2024)
   - What changed?
   - Why did it change?

3. Lessons learned
   - What didn't work before?
   - What problems were solved?
```

---

### Tactic 4: Consensus building
**Zidentyfikuj agreement vs. disagreement.**

```
After reading 5+ sources:

Strong consensus:
- 4+ sources agree
→ High confidence

Partial consensus:
- 2-3 sources agree
→ Note the majority view + alternatives

No consensus:
- Sources disagree
→ Explain the disagreement and context
```

---

### Tactic 5: Context extraction
**Zawsze wyciągaj context z recommendations.**

```
When source says: "Use [X]"
Extract:
- For what use case?
- At what scale?
- With what constraints?
- Why not [Y]?

Context matters:
- Startup vs. Enterprise
- 100 users vs. 1M users
- MVP vs. Production
- Solo dev vs. Team
```

---

## WebFetch optimization

### Effective prompts
**DO:**
- ✅ "Extract key points about [specific aspect]"
- ✅ "Summarize pros and cons"
- ✅ "What are the main recommendations"
- ✅ "List the steps to [do X]"

**DON'T:**
- ❌ "Tell me about this page" (too vague)
- ❌ "Is this good?" (subjective)
- ❌ "Summarize" (too general)

### Structured extraction
**Template prompts:**

For documentation:
"Extract: 1) Key features, 2) Limitations, 3) Use cases, 4) Getting started steps"

For blog posts:
"Extract: 1) Main argument, 2) Supporting points, 3) Practical recommendations, 4) Lessons learned"

For comparisons:
"Extract: 1) What's being compared, 2) Comparison criteria, 3) Pros/cons of each, 4) Final recommendation"

For tutorials:
"Extract: 1) Prerequisites, 2) Main steps, 3) Key concepts, 4) Common pitfalls"

For case studies:
"Extract: 1) Problem, 2) Solution, 3) Results/outcomes, 4) Lessons learned"

---

## Red flags podczas researchu

### 🚩 Content red flags
- Only one source supports a claim
- Sources are all from same type (e.g., only personal blogs)
- Information contradicts official docs
- No concrete examples
- Too good to be true claims

### 🚩 Process red flags
- You've only checked 1-2 sources
- All sources are >2 years old
- You skipped official documentation
- You haven't checked for known issues
- No practical examples found

### 🚩 Analysis red flags
- You can't explain trade-offs
- You don't understand why recommendation exists
- You found only positives, no negatives
- You can't identify context for recommendations
- Patterns are unclear between sources

---

## Quality checklist dla researchu

Before finishing research phase:

✅ **Coverage:**
- [ ] Checked official documentation
- [ ] Found 2-3 expert sources
- [ ] Checked community feedback
- [ ] Looked for known issues
- [ ] Found practical examples

✅ **Depth:**
- [ ] Understand core concepts
- [ ] Know the trade-offs
- [ ] Identified edge cases
- [ ] Found best practices
- [ ] Know anti-patterns

✅ **Verification:**
- [ ] Cross-referenced key claims
- [ ] Identified consensus points
- [ ] Noted disagreements
- [ ] Checked dates of sources
- [ ] Verified with multiple source types

✅ **Practicality:**
- [ ] Found actionable recommendations
- [ ] Have concrete examples
- [ ] Understand implementation
- [ ] Know common pitfalls
- [ ] Can explain to user

---

## Przykłady strategii

### Example: "Research React state management"

**Phase 1: Overview (10 min)**
1. Official React docs → State management section
2. "React state management 2024" → Top 3 articles
3. GitHub trending → Popular libraries

**Phase 2: Deep dive (20 min)**
1. Top 3 solutions (useState/useReducer, Context, Zustand/Redux)
2. Each solution:
   - Official docs
   - One expert blog
   - GitHub repo

**Phase 3: Practical (15 min)**
1. StackOverflow: Common problems
2. "When to use [X]" articles
3. Migration stories

**Phase 4: Synthesis (10 min)**
1. Comparison matrix
2. Use cases for each
3. Recommendations by context

---

### Example: "How to implement authentication"

**Phase 1: Understand options (10 min)**
1. "Authentication best practices 2024"
2. Auth0/Clerk/Supabase docs (managed options)
3. JWT vs. Sessions articles

**Phase 2: Deep dive chosen approach (20 min)**
1. Official documentation
2. Security considerations (OWASP)
3. Implementation guides

**Phase 3: Production insights (15 min)**
1. Company blogs: How they do auth
2. Common security mistakes
3. StackOverflow: Common issues

**Phase 4: Practical guide (10 min)**
1. Step-by-step implementation
2. Security checklist
3. Common pitfalls

---

## Summary: Research workflow

```
1. Understand the question
   ↓
2. Choose research strategy
   (based on question type)
   ↓
3. Execute strategy
   - Start with best sources
   - Use effective WebFetch prompts
   - Note key findings
   ↓
4. Cross-reference
   - Verify key claims
   - Identify patterns
   ↓
5. Synthesize
   - Extract insights
   - Create recommendations
   - Document sources
```
