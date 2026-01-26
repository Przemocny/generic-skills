# Briefing Questions Library

Complete list of questions to ask users when creating skills. Ask ONE AT A TIME, adapt based on answers.

---

## Core Questions (REQUIRED)

These questions are essential for every skill.

### 1. Skill Purpose

**Question:** "Jaki jest główny cel tego skilla? Co konkretnie ma robić?"

**Examples:**
- PDF processing
- Code review
- Git workflow
- API testing
- Database queries
- Documentation generation

**If unclear:** Provide examples and ask for specific scenarios

**If too broad:** Help narrow down to primary function

**Why important:** Determines entire skill scope and structure

---

### 2. Usage Examples

**Question:** "Czy możesz podać 2-3 przykłady, jak użytkownik będzie prosił o użycie tego skilla? Konkretne frazy lub zdania."

**Examples:**
- "Extract text from this PDF"
- "Review this pull request for security issues"
- "Generate API documentation from these endpoints"

**Why critical:** These phrases directly inform the `description` field - the most important field for skill triggering. Agent uses semantic matching on description to decide when to load skill.

**Follow-up if needed:**
- "Jakie inne słowa użytkownik może użyć?"
- "Czy są warianty tej frazy?"
- "W jakim kontekście to będzie używane?"

---

### 3. Task Scope

**Question:** "Czy ten skill będzie obsługiwał pojedynczy task, czy szerszy zestaw powiązanych zadań?"

**Options:**
- **Single task** - Focused, specific operation
  - Example: "rotate PDF"
  - When: Operation is atomic and well-defined

- **Related tasks** - Family of operations in same domain
  - Example: "PDF processing: extract, rotate, merge"
  - When: Tasks share domain knowledge and workflows

**If many unrelated tasks:** Suggest splitting into multiple focused skills

**Why important:** Determines skill complexity and whether to use references/ for sub-domains

---

### 4. Environment/Platform

**Question:** "Gdzie będzie używany ten skill?"

**Options:**
- **Claude Code** → Universal SKILL.md format
- **Cursor** → Can use .mdc modular format or SKILL.md
- **Windsurf** → SKILL.md format
- **Antigravity** → SKILL.md format
- **Universal/Multiple** → SKILL.md (works everywhere)

**If "don't know":** Recommend universal SKILL.md format

**If multiple platforms:** Use universal SKILL.md format (most compatible)

**Why important:** Determines file format and any platform-specific features

---

### 5. Freedom Level

**Question:** "Jak bardzo deterministyczny ma być proces? Czy agent potrzebuje dużej swobody decyzyjnej, czy raczej ma wykonywać ściśle określone kroki?"

**Options:**

**High freedom** (text-based instructions):
- Multiple valid approaches
- Decisions depend on context
- Heuristics guide the approach
- Example: "Analyze code quality" - many ways to do it

**Medium freedom** (preferred patterns):
- Preferred pattern exists
- Some variation acceptable
- Configuration affects behavior
- Example: "Create API endpoint" - follow conventions but adapt

**Low freedom** (specific scripts/steps):
- Fragile operations
- Consistency critical
- Narrow path with guardrails
- Example: "Deploy to production" - exact steps, no deviation

**Guidance:** Match fragility and variability of task

**Analogy:** Think of agent exploring path:
- High freedom = open field (many routes)
- Medium freedom = marked trail (recommended path, can deviate)
- Low freedom = narrow bridge with cliffs (must follow exactly)

**Why important:** Determines whether to use scripts vs instructions, how prescriptive to be

---

### 6. Expected Output

**Question:** "Jaki jest oczekiwany output tego skilla?"

**Examples:**
- Modified files
- Generated reports (markdown, HTML, JSON)
- Created code/tests
- Analysis results
- Data transformations
- New files/directories

**Follow-up:** "W jakim formacie? Gdzie ma być zapisane?"

**Why important:** Determines what to include in "success criteria" and quality checks

---

## Optional Questions (Context-Dependent)

Ask these based on responses to core questions.

### 7. User Base

**Question:** "Czy skill będzie używany tylko przez Ciebie, czy przez zespół/wielu użytkowników?"

**Solo use:**
- Can include personal preferences
- Can hardcode personal paths (with caution)
- Can use personal conventions
- Less documentation needed

**Team use:**
- Should be generic and portable
- No hardcoded personal paths
- Document conventions clearly
- More comprehensive documentation

**Why important:** Affects portability and documentation requirements

---

### 8. Scripts Needed

**Question:** "Czy ten skill będzie wymagał gotowych skryptów do wykonywania powtarzalnych operacji? Jeśli tak, jakich?"

**When scripts make sense:**
- Same code repeatedly rewritten by agent
- Deterministic reliability needed
- Complex operations prone to errors
- Output-heavy operations (script output cheaper than code in context)
- Operations requiring specific libraries/setup

**When to skip scripts:**
- Agent can easily write code inline
- Logic varies significantly per use case
- Operation is simple and non-repetitive

**If yes, ask for details:**
- What language? (Python, Bash, JavaScript, etc.)
- What does script do?
- What are inputs/outputs?
- Any dependencies?

**Why important:** Determines whether to create scripts/ folder and what scripts to include

---

### 9. Reference Documentation

**Question:** "Czy potrzebna będzie dokumentacja referencyjna do wczytywania on-demand (np. API docs, schematy baz danych, company policies, detailed examples)?"

**When references make sense:**
- Detailed API documentation
- Database schemas and relationships
- Domain knowledge (finance formulas, legal templates)
- Extensive examples (would clutter SKILL.md)
- Company policies and procedures
- Complex workflows with variations

**Types of references:**
- **API docs** - Complete endpoint documentation
- **Schemas** - Database tables, relationships, field definitions
- **Examples** - Comprehensive example library
- **Guidelines** - Detailed best practices
- **Patterns** - Design patterns and templates

**If yes, ask:**
- What kind of documentation?
- How much detail?
- Will it change frequently?

**Why important:** Determines references/ folder structure and progressive disclosure strategy

---

### 10. Assets/Templates

**Question:** "Czy skill będzie potrzebował assetów/templatek do używania w outputcie (np. pliki startowe, szablony, obrazki, boilerplate code)?"

**When assets make sense:**
- Document templates (DOCX, PDF, PowerPoint)
- Boilerplate code (HTML/React templates, config files)
- Images (logos, diagrams, icons)
- Config templates (JSON, YAML)
- Lookup tables (CSV, JSON data)

**Assets vs References:**
- **Assets**: Files used IN output (not loaded to context)
- **References**: Files loaded to context for agent to read

**If yes, ask:**
- What kind of assets?
- Will user provide them or should we create examples?
- What formats?

**Why important:** Determines assets/ folder and what to include

---

### 11. External Integrations

**Question:** "Czy skill będzie integrował się z zewnętrznymi narzędziami lub API? Jakimi?"

**Examples:**
- GitHub API (gh cli)
- Jira, Salesforce, Slack
- Database connections (PostgreSQL, MySQL, MongoDB)
- Cloud providers (AWS, GCP, Azure)
- CLI tools (git, docker, kubectl)

**If yes, important considerations:**
- **Authentication:** How to handle credentials?
- **API rate limits:** How to handle throttling?
- **Error handling:** Network issues, timeouts, API errors
- **Dependencies:** Tools must be installed?

**Follow-up questions:**
- "Jak będą przechowywane credentials?"
- "Czy są rate limits?"
- "Co się dzieje gdy API nie odpowiada?"

**Why important:** Determines integration patterns to use, error handling needs, security considerations

---

### 12. Workflow Type

**Question:** "Czy workflow będzie multi-step z zależnościami między etapami, czy raczej proste, jednokrokowe zadania?"

**Multi-step workflow:**
- Sequential processes
- Steps depend on previous results
- Conditional branching
- State management
- See: references/workflow-patterns.md

**Single-step:**
- Simple, atomic operations
- No dependencies
- Straightforward input → output

**Follow-up if multi-step:**
- "Czy kroki muszą być wykonane w kolejności?"
- "Czy są warunki które zmieniają flow?"
- "Co się dzieje gdy któryś krok fail?"

**Why important:** Determines workflow pattern to use and complexity of SKILL.md

---

### 13. User Interaction

**Question:** "Czy skill będzie wymagał interakcji z użytkownikiem w trakcie wykonywania (pytania, potwierdzenia), czy ma działać autonomicznie?"

**Interactive:**
- Agent asks questions during execution
- Confirms dangerous actions
- Gathers additional context mid-workflow
- Good for: complex decisions, safety-critical operations

**Autonomous:**
- Agent executes without interruption
- Uses information from initial prompt
- Good for: routine tasks, well-defined operations

**Hybrid:**
- Autonomous by default
- Asks only for critical decisions

**Why important:** Affects workflow design and user experience

---

### 14. Quality Standards

**Question:** "Czy są jakieś quality standards lub wymagania walidacyjne, które output musi spełniać?"

**Examples:**
- Code must pass linter (eslint, pylint, etc.)
- Tests must run successfully
- Output must match JSON schema
- Security requirements (no hardcoded secrets)
- Performance benchmarks (response time, file size)
- Accessibility standards (WCAG)

**If yes, consider:**
- PostToolUse hooks for validation (Claude Code)
- Validation scripts in scripts/
- Quality checklist in SKILL.md

**Why important:** Determines validation strategy and quality gates

---

## Adaptive Questioning Strategy

**Don't ask all questions blindly.** Adapt based on answers:

### If simple single-task skill:
- Skip: Scripts (unless obvious need)
- Skip: References (keep everything in SKILL.md)
- Skip: Assets (usually not needed)
- Focus: Clear instructions, examples

### If complex multi-domain skill:
- Ask: References (likely needed)
- Ask: Scripts (if repetitive operations)
- Ask: Workflow type (likely multi-step)
- Ask: Quality standards (likely important)

### If integration-heavy skill:
- **Must ask:** External integrations
- Ask: Error handling needs
- Ask: Authentication strategy
- Consider: Rate limiting, retries

### If workflow skill:
- **Must ask:** Workflow type
- Ask: User interaction (confirmations?)
- Ask: Quality standards (validation?)
- Consider: State management, rollback

---

## Question Flow Examples

### Example 1: Simple Script Wrapper Skill

```
1. Purpose: "Rotate PDF files"
2. Usage: "Rotate this PDF 90 degrees"
3. Scope: Single task
4. Platform: Universal
5. Freedom: Low (deterministic operation)
6. Output: Rotated PDF file

→ Ask: Scripts? YES (PDF rotation script)
→ Skip: References (simple operation)
→ Skip: Assets (no templates needed)
→ Skip: Integrations (standalone operation)
```

### Example 2: Complex Multi-Domain Skill

```
1. Purpose: "Query company database for analytics"
2. Usage: "Show me revenue by region", "List top customers"
3. Scope: Related tasks (all analytics queries)
4. Platform: Universal
5. Freedom: Medium (preferred patterns, some variation)
6. Output: Query results, charts, reports

→ Ask: Scripts? MAYBE (common query templates)
→ Ask: References? YES (schema documentation)
→ Ask: Integrations? YES (database connection)
→ Ask: Workflow? Multi-step (query → process → visualize)
→ Ask: Quality? YES (data validation)
```

### Example 3: Briefing/Interview Skill

```
1. Purpose: "Brief users on business concepts"
2. Usage: "Help me brief a client", "Ask questions about product"
3. Scope: Related tasks (different briefing types)
4. Platform: Claude Code
5. Freedom: High (conversational, adaptive)
6. Output: Completed brief document

→ Skip: Scripts (conversational, no deterministic operations)
→ Ask: References? YES (question libraries, templates)
→ Ask: Assets? MAYBE (brief templates)
→ Ask: Workflow? Multi-step (phases of briefing)
→ Skip: Integrations (self-contained)
```

---

## Tips for Effective Briefing

**DO:**
- Ask ONE question at a time
- Wait for answer before next question
- Adapt based on answers (skip irrelevant questions)
- Provide examples when user is unclear
- Clarify vague answers
- Confirm understanding before proceeding

**DON'T:**
- Ask all 14 questions in one message (overwhelming)
- Ask irrelevant questions (e.g., scripts for conversational skill)
- Accept vague answers without clarification
- Make assumptions without confirming
- Rush through briefing (quality briefing = quality skill)

**Remember:** Better briefing = better skill. Take time to understand user's needs deeply.
