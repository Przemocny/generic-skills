# Generic Skills - Agent Skills Project

You are an AI agent working with the Generic Skills project by CampusAI.

## Project Overview

Generic Skills are reusable Agent Skills for Claude AI that enhance research capabilities, skill management, and agent configuration creation. These skills provide systematic frameworks for gathering knowledge from the internet, creating and improving agent skills, and generating AI agent configuration files.

**Philosophy:** Quality and methodology matter. Good research follows systematic approaches with clear evaluation criteria. Good skills require ongoing refinement and critical analysis. Effective agents need proper configuration.

**Approach:**
- **Systematic over ad-hoc** - Follow proven research and analysis methodologies
- **Multi-source verification** - Cross-reference information across multiple credible sources
- **Quality-focused** - Prioritize source credibility and content accuracy
- **Continuous improvement** - Skills should be analyzed and refined regularly
- **Evidence-based** - Concrete data and examples over assumptions
- **Structured output** - Well-organized reports and improvement plans
- **Guided creation** - Structured briefing process for skill and agent creation

## Project Structure

```
skills/
├── general-research/              # Systematic internet research
│   ├── SKILL.md                   # Skill instructions
│   └── references/                # Research strategies, questions, source evaluation
│       ├── research-strategies.md
│       ├── question-templates.md
│       ├── report-template.md
│       └── source-evaluation.md
├── general-skill-refiner/         # Skill quality analysis and improvement
│   ├── SKILL.md                   # Skill instructions
│   └── references/                # Quality criteria, common issues, refactoring patterns
│       ├── quality-criteria.md
│       ├── common-issues.md
│       └── refactoring-patterns.md
├── general-skill-maker/           # Create new agent skills
│   ├── SKILL.md                   # Skill instructions
│   └── references/                # Best practices, patterns, examples
│       ├── best-practices.md
│       ├── briefing-questions.md
│       ├── common-mistakes.md
│       ├── complete-example.md
│       ├── quality-checklist.md
│       ├── workflow-patterns.md
│       ├── tool-based-patterns.md
│       ├── domain-knowledge-patterns.md
│       ├── integration-patterns.md
│       └── advanced-features.md
├── general-skill-upgrader/        # Enhance existing skills functionally
│   ├── SKILL.md                   # Skill instructions
│   └── references/                # Upgrade types, patterns, examples
│       ├── upgrade-types.md
│       ├── implementation-patterns.md
│       └── examples.md
├── agentmd-creator/               # Create AI agent configuration files
│   ├── SKILL.md                   # Skill instructions
│   └── references/                # Best practices, examples, platforms
│       ├── best-practices.md
│       ├── examples.md
│       ├── platforms.md
│       └── use-cases.md
└── context-collecter/             # Personal and company context management
    ├── SKILL.md                   # Skill instructions
    └── references/                # Briefing question libraries
        ├── private-categories.md  # Questions for 10 private context layers
        └── company-categories.md  # Questions for 10 company context layers
```

**Output locations:**
- **general-research:** `.research/[topic-slug]-[date].md` containing research reports
- **general-skill-refiner:** `.tasks/skill-refactoring-[skill-name]-[date]/` containing change reports
- **general-skill-maker:** `skills/[skill-name]/` containing new skill directory with SKILL.md and references
- **general-skill-upgrader:** `.tasks/skill-upgrade-[skill-name]-[date]/` containing upgrade reports and changes
- **agentmd-creator:** `AGENTS.md`, `CLAUDE.md`, `.cursor/rules/*.mdc`, or platform-specific config files
- **context-collecter:** `~/.claude/context/private/` (10 private layers) and `~/.claude/context/company/` (10 company layers)

---

## Available Skills

### 1. general-research

**Purpose:** Systematic internet research on any topic through methodical questioning and multi-source analysis.

**When to use:**
- User needs to gather knowledge from the internet (not codebase exploration)
- User wants to find best practices, compare solutions, or evaluate technologies
- User asks about industry trends or needs to understand a topic thoroughly
- User mentions "research online", "gather information about", "find best practices"
- User wants to compare solutions or evaluate options

**What it does:**
- Conducts systematic research through 5-phase process:
  - **Phase 1:** Understanding - clarify what's truly needed before starting
  - **Phase 2:** Strategic questions - narrow scope and direction (4 core questions)
  - **Phase 3:** Research - multi-source gathering using WebSearch + WebFetch
  - **Phase 4:** Creating report - structured documentation in `.research/`
  - **Phase 5:** Review and feedback - summarize findings and next steps
- Uses minimum 5-8 different sources from multiple tiers
- Cross-references key claims across sources for verification
- Evaluates source credibility using tier system (official docs → expert blogs → community)
- Identifies patterns, consensus, and controversies across sources
- Produces structured reports with analysis, not just data collection

**Key behaviors:**
- Starts with WebSearch to identify best sources
- Uses tier system to evaluate source quality (Tier 1: official docs, Tier 2: expert blogs, Tier 3: community)
- Cross-references all key claims across multiple sources
- Focuses on patterns and consensus, not just individual opinions
- Documents both agreements and disagreements between sources
- Provides practical, actionable recommendations
- Marks uncertainties explicitly
- Iterative process - can return to earlier phases as needed

**Output:** Research report in `.research/[topic-slug]-[date].md` containing:
- Executive summary with key insights
- Key findings organized by topic
- Patterns and consensus across sources
- Best practices and recommendations
- Common pitfalls and anti-patterns
- All sources documented with tier ratings
- Practical next steps

**Triggers:**
- "Research [topic] online"
- "What are best practices for [X]?"
- "Compare [A] vs [B] vs [C]"
- "Find information about [topic]"
- "What do sources say about [X]?"
- "Look up [technology/approach]"
- "Investigate [topic] thoroughly"

---

### 2. general-skill-refiner

**Purpose:** Critical analysis and improvement of agent skills through systematic quality review.

**When to use:**
- User wants to review, improve, or refactor an existing skill
- User asks about skill quality or wants an audit
- User mentions "review skill", "improve skill", "optimize skill"
- After creating a new skill, to ensure quality
- When skill seems to have issues or isn't performing well

**What it does:**
- Conducts comprehensive skill analysis through 5-phase process:
  - **Phase 1:** Read & Understand - deeply analyze skill structure and references
  - **Phase 2:** Critical Analysis - identify issues using quality criteria
  - **Phase 3:** Present & Gather Feedback - show findings, get user approval
  - **Phase 4:** Refactor - systematically implement improvements
  - **Phase 5:** Verify & Report - ensure quality and document changes
- Checks against comprehensive quality criteria from Agent Skills guidelines
- Identifies issues in 3 priority levels:
  - **🔴 Priority 1 (MUST FIX):** Time estimates, excessive length, broken structure
  - **🟡 Priority 2 (SHOULD FIX):** Redundancy, unclear phases, missing examples
  - **🟢 Priority 3 (NICE TO HAVE):** Style improvements, enhancements
- Provides concrete solutions, not just complaints
- Tracks changes and reports before/after metrics

**Key behaviors:**
- Ruthlessly critical in analysis phase (finds all issues)
- Prioritizes problems clearly (MUST/SHOULD/NICE)
- Gives specific solutions with line numbers
- Gets user feedback before making changes
- Makes atomic, verifiable changes
- Documents what changed and why
- Verifies quality after refactoring
- Respects user preferences even if suboptimal

**Output:** Refactored skill + change report in `.tasks/skill-refactoring-[skill-name]-[date]/` containing:
- Complete prioritized issue list
- Before/after metrics (line counts, structure improvements)
- Detailed change log
- Quality checklist verification
- Specific improvements made

**Common issues detected:**
- Time estimates (forbidden per guidelines)
- SKILL.md too long (>400 lines)
- Template bloat (inline templates should be in references/)
- Question explosion (too many questions without prioritization)
- Missing examples or unclear workflow
- Redundant content between SKILL.md and references
- Weak quality checklists
- Missing special cases handling

**Triggers:**
- "Review this skill"
- "Improve [skill name]"
- "Audit skill quality"
- "Refactor this skill"
- "Is this skill good?"
- "Fix issues in [skill]"
- "Optimize skill structure"

---

### 3. general-skill-maker

**Purpose:** Create effective agent skills through guided briefing process that explores requirements, patterns, and best practices.

**When to use:**
- User wants to create a new skill from scratch
- User needs to build a skill for specific task or workflow
- User mentions "create skill", "make skill", "build skill", "new skill"
- User wants to design skill architecture
- User needs guidance on skill structure and patterns

**What it does:**
- Guides through 6-step creation process:
  - **Step 1:** Briefing questions - understand requirements through strategic questions
  - **Step 2:** Load references - gather relevant patterns and best practices
  - **Step 3:** Plan structure - design skill architecture and workflow
  - **Step 4:** Generate - create SKILL.md and references files
  - **Step 5:** Apply best practices - ensure quality and completeness
  - **Step 6:** Present - show final skill and usage instructions
- Asks targeted questions one at a time (not overwhelming)
- Identifies appropriate patterns: workflow-based, tool-based, domain-knowledge, or hybrid
- Generates comprehensive references/ subdirectory with supporting materials
- Ensures skill follows Agent Skills specification and best practices
- Creates quality checklist tailored to skill's purpose

**Key behaviors:**
- Adapts questioning based on previous answers
- Skips irrelevant questions intelligently
- Selects appropriate skill patterns based on requirements
- Generates complete reference materials (not just SKILL.md)
- Includes concrete examples and templates
- Ensures proper triggers for skill activation
- Creates skills that are 200-400 lines (details in references/)
- Validates against quality criteria automatically

**Output:** New skill directory in `skills/[skill-name]/` containing:
- SKILL.md with frontmatter, overview, workflow, examples, quality checklist
- references/ subdirectory with 3-10 supporting files
- Best practices, patterns, templates, examples as needed
- Quality checklist specific to skill's purpose

**Triggers:**
- "Create skill for [X]"
- "Make new skill"
- "Build skill"
- "Design skill"
- "Help me create skill"
- "New skill for [purpose]"
- "Initialize skill"

---

### 4. general-skill-upgrader

**Purpose:** Strategic enhancement and functional evolution of agent skills through business-driven improvements.

**When to use:**
- User wants to add new features or capabilities to existing skill
- User asks to enhance, upgrade, or expand skill functionality
- Skill works well but could do more
- User mentions "upgrade skill", "enhance skill", "add features", "make skill better"
- Looking for opportunities to improve user experience or automation

**What it does:**
- Analyzes skill through 5-phase process:
  - **Phase 1:** Understand business goal - what problem does skill solve?
  - **Phase 2:** Identify opportunities - scan 22 types of potential upgrades
  - **Phase 3:** Present options - show 1-2 most valuable upgrades
  - **Phase 4:** Implement - user picks, agent executes enhancement
  - **Phase 5:** Verify & report - confirm improvement and document changes
- Focuses on functional improvements (not quality fixes)
- Presents 1-2 upgrade options per run (iterative enhancement)
- Identifies opportunities across 22 categories: automation, error handling, output formats, integrations, UX, etc.
- Ensures upgrades align with skill's core business purpose

**Key behaviors:**
- Understands business context before proposing upgrades
- Presents clear value proposition for each option
- Focuses on practical, implementable enhancements
- Doesn't break existing functionality
- Iterative approach - can run multiple times for step-by-step evolution
- Measures improvement impact
- Documents upgrade rationale and implementation

**Output:** Enhanced skill + upgrade report in `.tasks/skill-upgrade-[skill-name]-[date]/` containing:
- Business goal analysis
- Identified upgrade opportunities with priority/impact scores
- Selected upgrade details and implementation plan
- Before/after comparison
- Upgrade verification results

**Key difference from refiner:**
- **Refiner** = fix problems (quality issues, bugs, violations)
- **Upgrader** = add value (new capabilities, better UX, smarter workflow)

**Triggers:**
- "Upgrade skill"
- "Enhance [skill name]"
- "Add features to skill"
- "Improve skill functionality"
- "Evolve skill"
- "Make skill better"
- "Expand skill capabilities"

---

### 5. agentmd-creator

**Purpose:** Create AI agent configuration files (AGENTS.md, CLAUDE.md, .cursorrules, etc.) for general-purpose and business-domain agents.

**When to use:**
- User wants to create agent configuration file for specific purpose or domain
- User needs to set up AI assistant for business workflows
- User mentions "create agent config", "make AGENTS.md", "setup AI assistant"
- User wants to configure agent for specific role (sales, HR, research, etc.)
- User needs agent guidelines for organization or team

**What it does:**
- Guides through 6-step creation process:
  - **Step 1:** Briefing questions - understand agent purpose, domain, and platform
  - **Step 2:** Load references - gather platform-specific best practices and examples
  - **Step 3:** Generate structure - create appropriate config file format
  - **Step 4:** Customize content - add domain knowledge, boundaries, guidelines
  - **Step 5:** Apply best practices - ensure quality and completeness
  - **Step 6:** Present - show final config and installation instructions
- Asks targeted questions one at a time
- Supports multiple platforms: Claude Code, Cursor, Windsurf, Antigravity, JetBrains Junie, GitHub Copilot
- Generates platform-specific file formats and locations
- Includes domain-specific knowledge and business rules
- Defines clear boundaries and quality standards

**Key behaviors:**
- Adapts to agent purpose and domain
- Selects appropriate platform format automatically
- Includes relevant best practices and examples
- Defines clear agent boundaries and limitations
- Adds quality checklists and verification steps
- Ensures proper tone and communication style
- Includes file-specific rules if needed

**Output:** Platform-specific configuration file(s):
- `AGENTS.md` - universal format for any platform
- `CLAUDE.md` - for Claude Code
- `.cursor/rules/*.mdc` - for Cursor IDE
- `.windsurf/rules/*.md` - for Windsurf IDE
- `.antigravity/rules.md` - for Antigravity
- `.junie/guidelines.md` - for JetBrains Junie
- `.github/copilot-instructions.md` - for GitHub Copilot

**Triggers:**
- "Create agent config"
- "Setup AI assistant"
- "Make AGENTS.md"
- "Configure agent for [role]"
- "AI agent for [business domain]"
- "Generate CLAUDE.md"
- "Help me configure Cursor/Windsurf agent"

---

### 6. context-collecter

**Purpose:** Collect, save and load personal and company context across 20 information layers. Always communicates in Polish.

**When to use:**
- User wants to save information for future sessions (personal or company)
- User says "zapamiętaj że...", "zapisz że...", "zanotuj...", "dodaj do kontekstu..."
- User wants guided interview to fill context layers: "zbierz kontekst o...", "przeprowadź wywiad o..."
- User wants to load saved context: "zamontuj kontekst...", "załaduj kontekst...", "co wiesz o mnie..."
- User wants to edit or delete context entries: "popraw...", "zmień to że...", "usuń wpis..."

**What it does:**
- Manages context through 4 modes:
  - **Mode 1: Quick Save** - Analyzes message, picks most likely layer, checks for conflicts/duplicates, asks for confirmation before saving
  - **Mode 2: Briefing** - Guided interview (deep or quick scan) using question libraries from references/
  - **Mode 3: Load/Mount** - Silently loads requested layers, confirms what was loaded, checks for expired entries
  - **Mode 4: Edit/Delete** - Finds specific entry, shows it to user, applies change after confirmation
- Stores data in structured Markdown files with `<!-- static/dynamic | date | deadline -->` annotations
- Follows cross-layer links `[[layer/name]]` automatically
- Alerts on expired `dynamic` entries at every load

**Key behaviors:**
- ALWAYS asks for confirmation before saving (proposes layer name, waits for OK)
- ALWAYS forces confirmation before deleting entries
- Checks for conflicts and duplicates before writing
- Asks one question at a time during briefing
- Loads context silently (no raw file dump) then confirms briefly what was loaded
- Communicates exclusively in Polish

**Context layers:**
- **Private (10):** `tozsamosc`, `wartosci`, `cele`, `rodzina`, `zdrowie`, `finanse-osobiste`, `hobby`, `styl-zycia`, `rozwoj`, `relacje`
- **Company (10):** `firma`, `moja-rola`, `zespol`, `produkty`, `klienci`, `procesy`, `strategia`, `technologia`, `finanse-firmy`, `wyzwania`

**Output:** Context files in `~/.claude/context/private/` and `~/.claude/context/company/`

**Triggers:**
- "Zapamiętaj że..."
- "Zapisz że..."
- "Zbierz kontekst o..."
- "Zamontuj kontekst..."
- "Co wiesz o mnie?"
- "Załaduj kontekst firmowy"
- "Popraw wpis o..."

---

## Skill Workflows

All skills follow systematic, phase-based workflows:

### general-research Workflow (5 phases)

**Phase 1: Understanding the Topic**
- Clarify what user seeks and why
- Understand level of detail needed
- Identify usage context and constraints
- Note scope limitations

**Phase 2: Strategic Questions**
- Ask 4 core questions (scope, direction, context, constraints)
- Narrow research focus
- Confirm understanding before proceeding
- Use additional questions from `references/question-templates.md` if needed

**Phase 3: Research**
- Find sources using WebSearch
- Prioritize using tier system (see `references/source-evaluation.md`)
- Extract information using WebFetch with specific prompts
- Cross-reference claims across sources
- Identify patterns and consensus
- Iterative - can return to earlier phases if needed

**Phase 4: Creating the Report**
- Choose template (minimal/standard/extended) based on scope
- Organize findings systematically
- Add analysis and synthesis (not just data collection)
- Document all sources with tier ratings
- Create file in `.research/[topic-slug]-[date].md`

**Phase 5: Review and Feedback**
- Summarize key findings
- Present practical recommendations
- Ask for feedback on scope
- Offer next steps if appropriate

### general-skill-refiner Workflow (5 phases)

**Phase 1: Read & Understand**
- Read SKILL.md and all references
- Measure file lengths and structure
- Understand skill workflow and purpose
- Compare with other skills

**Phase 2: Critical Analysis**
- Check against quality criteria (see `references/quality-criteria.md`)
- Identify issues using `references/common-issues.md`
- Classify problems: 🔴 MUST / 🟡 SHOULD / 🟢 NICE priority
- Propose concrete solutions for each issue

**Phase 3: Present & Gather Feedback**
- Present prioritized issue list to user
- Explain each problem and proposed fix
- Get user approval on what to fix
- Listen for user preferences and constraints

**Phase 4: Refactor**
- Fix Priority 1 issues first (critical)
- Apply refactoring patterns from `references/refactoring-patterns.md`
- Make atomic changes, verify each one
- Track changes in log file
- Continue with Priority 2, then 3 if approved

**Phase 5: Verify & Report**
- Verify files are valid
- Check metrics improved
- Run quality checklist
- Report changes to user with before/after metrics
- Create report in `.tasks/skill-refactoring-[skill-name]-[date]/`

### general-skill-maker Workflow (6 steps)

**Step 1: Briefing Questions**
- Ask core questions one at a time (purpose, examples, platform, freedom level, output)
- Ask optional questions based on context (scope, integrations, quality standards, etc.)
- Adapt questioning dynamically based on answers
- See `references/briefing-questions.md` for complete question library

**Step 2: Load Appropriate References**
- Always load `references/best-practices.md`
- Load pattern-specific references based on skill type:
  - Workflow patterns for multi-phase processes
  - Tool-based patterns for tool-centric skills
  - Domain knowledge patterns for specialized domains
  - Integration patterns for external services

**Step 3: Plan Structure**
- Determine skill pattern (workflow/tool/domain/hybrid)
- Design phase structure or step sequence
- Plan references/ subdirectory content (3-10 files)
- Identify needed templates, examples, checklists

**Step 4: Generate**
- Create SKILL.md with frontmatter, overview, workflow, examples, quality checklist
- Generate references/ files with detailed guidance
- Include complete examples showing input → output
- Add quality checklist tailored to skill purpose

**Step 5: Apply Best Practices**
- Verify SKILL.md is 200-400 lines (details in references/)
- Check no time estimates anywhere
- Ensure proper triggers in description
- Validate against quality criteria
- Add examples and special cases

**Step 6: Present**
- Show skill structure and key features
- Explain how to use and when to trigger
- Provide installation instructions
- Offer to refine or enhance based on feedback

### general-skill-upgrader Workflow (5 phases)

**Phase 1: Understand Business Goal**
- Read skill thoroughly (SKILL.md + references)
- Identify core business purpose
- Understand what problem it solves
- Note current capabilities and limitations

**Phase 2: Identify Opportunities**
- Scan 22 upgrade types (see `references/upgrade-types.md`)
- Find 3-7 potential enhancements
- Evaluate each opportunity (impact, effort, value)
- Prioritize by business value

**Phase 3: Present Options**
- Show 1-2 most valuable upgrades this run
- Explain clear value proposition for each
- Provide examples of how upgrade improves workflow
- Ask user to pick which upgrade(s) to implement

**Phase 4: Implement**
- Execute selected upgrade(s)
- Apply patterns from `references/implementation-patterns.md`
- Ensure backward compatibility
- Update documentation and examples
- Track changes systematically

**Phase 5: Verify & Report**
- Test upgraded functionality works
- Verify existing features still work
- Document improvements and rationale
- Create report in `.tasks/skill-upgrade-[skill-name]-[date]/`
- Offer to continue with next round of upgrades

### agentmd-creator Workflow (6 steps)

**Step 1: Briefing Questions**
- Ask purpose question (REQUIRED): "Jaki jest główny cel tego agenta?"
- Ask domain question (can infer): "W jakiej dziedzinie będzie pracował?"
- Ask platform question (REQUIRED): "Jakiego narzędzia używasz?"
- Ask optional questions based on context (boundaries, quality standards, team/solo)
- Adapt questioning based on previous answers

**Step 2: Load References**
- Load platform-specific guide from `references/platforms.md`
- Load best practices from `references/best-practices.md`
- Load domain examples from `references/examples.md` if applicable
- Load use cases from `references/use-cases.md` for similar agents

**Step 3: Generate Structure**
- Determine file format based on platform
- Create appropriate config file structure
- Add frontmatter or metadata as needed
- Plan sections: overview, guidelines, boundaries, quality

**Step 4: Customize Content**
- Add agent purpose and domain context
- Include business rules and workflows
- Define clear boundaries and limitations
- Add quality standards and verification
- Include domain-specific knowledge if applicable

**Step 5: Apply Best Practices**
- Ensure proper tone and communication style
- Add file-specific rules if needed
- Include examples showing desired behavior
- Verify completeness using platform checklist
- Add troubleshooting and edge cases

**Step 6: Present**
- Show generated config file(s)
- Explain installation and setup
- Provide usage examples
- Offer customization suggestions
- Document testing and verification steps

### context-collecter Workflow (4 modes)

**Mode 1: Quick Save**
- Analyze message content, determine which layer(s) it belongs to
- Load 1-2 most likely layers
- Check for expired dynamic entries (prompt user if any found)
- Check for conflicts or duplicates in existing data
- Propose layer with confirmation: "Chcę to zapisać na warstwie [name]. OK?"
- If conflict found: show existing entry, ask to overwrite/append/skip
- After confirmation: write entry with `<!-- static/dynamic | date -->` annotation

**Mode 2: Briefing**
- Identify target layer from user message
- Load existing data from that layer + follow `[[links]]`
- Assess data volume → propose deep interview (few/no data) or quick scan (lots of data)
- Ask questions ONE AT A TIME, wait for answer before next
- After each answer: propose layer, confirm, save
- Summarize what was collected at the end

**Mode 3: Load/Mount**
- If scope unclear: ask "Co chcesz załadować? Wszystkie warstwy, kategorię czy temat?"
- Load requested layers, follow `[[links]]`
- Load silently (no raw content dump), confirm briefly: "Kontekst załadowany (X warstw)."
- Check all loaded layers for expired dynamic entries

**Mode 4: Edit/Delete**
- Identify target layer
- Load current content, find relevant entry
- Show entry to user: "Znalazłem: '[content]' — to ten wpis?"
- Wait for confirmation
- For edit: show proposed change, wait for approval
- For delete: force confirmation ("Usunąć trwale? Tego nie można cofnąć.")
- Apply change and save

---

## Skill Usage Patterns

### Independent Usage

All skills can be used independently:

**general-research:**
- Use anytime you need information from the internet
- No prerequisites or dependencies
- Can be used repeatedly for different topics
- Research reports accumulate in `.research/` folder

**general-skill-refiner:**
- Use to review any existing skill
- Can be used on skills from this repo or others
- Typically used after creating/modifying a skill
- Can be re-run on same skill after changes

**general-skill-maker:**
- Use to create new skills from scratch
- No prerequisites needed
- Generates complete skill directory with references
- Can create skills for any purpose or domain

**general-skill-upgrader:**
- Use to enhance existing skills functionally
- Works on any skill (from this repo or others)
- Iterative - run multiple times for step-by-step evolution
- Focuses on adding value, not fixing problems

**agentmd-creator:**
- Use to generate agent configuration files
- Works for any platform (Cursor, Claude, Windsurf, etc.)
- Can create general-purpose or domain-specific agents
- Generates platform-appropriate file format automatically

**context-collecter:**
- Use to build persistent memory about user, team, and company
- Four modes: Quick Save, Briefing, Load/Mount, Edit/Delete
- Manages 20 context layers (10 private + 10 company)
- Always communicates in Polish

### Complementary Usage

Skills work together in powerful workflows:

**Complete Skill Development Cycle:**
1. **Research** best practices for skill topic (general-research)
2. **Create** skill based on research findings (general-skill-maker)
3. **Refine** to fix quality issues (general-skill-refiner)
4. **Upgrade** to add new capabilities (general-skill-upgrader)
5. Repeat steps 3-4 for continuous improvement

**Agent Configuration Workflow:**
1. **Research** best practices for agent domain (general-research)
2. **Create** agent config file (agentmd-creator)
3. Test and gather feedback
4. **Research** improvements if needed
5. Update config based on findings

**Skill Enhancement Loop:**
1. **Upgrade** skill with new features (general-skill-upgrader)
2. **Refine** to ensure quality (general-skill-refiner)
3. **Research** better approaches if needed (general-research)
4. Apply improvements
5. Verify with refiner again

**Meta-Improvement:**
- Use general-research to study Agent Skills best practices
- Use general-skill-maker to create new meta-skills
- Use general-skill-refiner to improve skills themselves
- Use general-skill-upgrader to add capabilities to skills
- Use agentmd-creator to configure specialized agents
- Continuous improvement cycle for all skills

**Domain-Specific Workflows:**
- Research domain best practices → Create domain skill → Refine quality → Upgrade features
- Research agent requirements → Create agent config → Test → Research improvements → Update config
- Create skill → Upgrade functionality → Refine quality → Deploy

---

## Key Principles

### Systematic Over Ad-hoc
- Follow proven methodologies and frameworks
- Use structured approaches (phases, checklists)
- Don't skip steps even when tempting
- Trust the process - systematic work produces better results

### Multi-Source Verification
- Never rely on single source for important claims
- Cross-reference information across multiple sources
- Use source tier system to evaluate credibility
- Document where information comes from

### Quality-Focused
- Prioritize source quality over quantity
- Official documentation > blog posts > forums
- Recent information preferred (check dates)
- Look for consensus across credible sources

### Analysis Over Collection
- Don't just gather data - synthesize insights
- Identify patterns and trends
- Draw conclusions and make recommendations
- Practical, actionable outputs

### Continuous Improvement
- Skills should be analyzed and refined regularly
- Use quality criteria to identify issues
- Prioritize fixes (MUST/SHOULD/NICE)
- Track improvements with metrics

### Evidence-Based
- Concrete data and examples over assumptions
- Verify claims with multiple sources
- Mark uncertainties explicitly
- Provide references for all claims

### Structured Output
- Well-organized reports and documentation
- Clear sections and hierarchy
- Easy to scan and navigate
- Consistent formatting and style

---

## Installation & Usage

### Manual Installation
```bash
# Copy to local skills directory
cp -r skills/* ~/.cursor/skills/

# Or for Claude Desktop
cp -r skills/* ~/.claude/skills/
```

### Verify Installation
```bash
# Check skills are present
ls ~/.cursor/skills/
# Should show: general-research/ general-skill-refiner/ general-skill-maker/ general-skill-upgrader/ agentmd-creator/

# Check skill structure
ls ~/.cursor/skills/general-research/
# Should show: SKILL.md references/

ls ~/.cursor/skills/general-skill-maker/
# Should show: SKILL.md references/ (with 10 reference files)
```

### Development & Testing
```bash
# Edit skills
vim skills/general-research/SKILL.md

# Test locally by copying to skills directory
cp -r skills/* ~/.cursor/skills/

# Use in conversations - agent will auto-discover based on triggers
```

### Usage
Skills are automatically discovered when user mentions trigger phrases:
- "Research [topic]" → activates general-research
- "Review this skill" → activates general-skill-refiner
- "Create skill for [X]" → activates general-skill-maker
- "Upgrade skill" → activates general-skill-upgrader
- "Create agent config" → activates agentmd-creator

No special commands needed - just describe what you want to do.

---

## File Structure & References

### general-research Contains:

**SKILL.md** (~389 lines)
- Overview and principles
- 5-phase workflow
- Examples of good research
- Quality checklist
- Key reminders

**references/research-strategies.md** (~537 lines)
- Research strategies by type (tech, comparison, best practices, etc.)
- Research tactics (pyramid approach, multi-angle, consensus building)
- WebFetch optimization tips
- Quality checklist for research phase

**references/question-templates.md** (~191 lines)
- 4 core questions (always ask)
- Optional questions by category
- Example question sequences
- When to use which questions

**references/source-evaluation.md** (~434 lines)
- Source tier system (Tier 1-5)
- Evaluation criteria (authority, accuracy, currency)
- Source type strengths/weaknesses
- Red flags to watch for

**references/report-template.md**
- Minimal report template for simple topics
- Standard report template for typical research
- Extended report template for complex topics
- Section guidelines and examples

### general-skill-refiner Contains:

**SKILL.md** (~435 lines)
- Overview and principles
- 5-phase workflow
- Examples of refactoring
- Quality checklist
- Key reminders

**references/quality-criteria.md** (~326 lines)
- Priority 1 issues (MUST FIX) - time estimates, excessive length, broken structure
- Priority 2 issues (SHOULD FIX) - redundancy, unclear phases, missing examples
- Priority 3 issues (NICE TO HAVE) - enhancements
- Comparison metrics and scoring system

**references/common-issues.md** (~548 lines)
- Structural issues (template bloat, question explosion, etc.)
- Content issues (time estimates, vague goals, missing examples)
- Organization issues (monolithic references, reference sprawl)
- Workflow issues (missing exit conditions, no error handling)
- Issue detection workflow

**references/refactoring-patterns.md** (~701 lines)
- Concrete patterns for fixing issues
- Before/after examples
- Step-by-step refactoring instructions
- Quick wins vs time sinks

### general-skill-maker Contains:

**SKILL.md** (~324 lines)
- Overview and principles
- 6-step creation workflow
- Pattern types (workflow, tool-based, domain knowledge, hybrid)
- Quality checklist
- Key reminders

**references/best-practices.md**
- Universal principles for all skills
- SKILL.md structure guidelines
- Reference file organization
- Quality standards and validation

**references/briefing-questions.md**
- Complete question library (14 questions)
- Core questions (always ask)
- Optional questions (context-dependent)
- Follow-up strategies and examples

**references/common-mistakes.md**
- Frequent skill creation errors
- Anti-patterns to avoid
- How to prevent and fix mistakes
- Before/after examples

**references/complete-example.md**
- Full skill example from start to finish
- Shows briefing → planning → generation
- Includes all files (SKILL.md + references)
- Demonstrates best practices in action

**references/quality-checklist.md**
- Comprehensive validation criteria
- Structure, content, and format checks
- Verification steps for quality
- Common issues detection

**references/workflow-patterns.md**
- Multi-phase process patterns
- Sequential and parallel workflows
- Phase transition handling
- Examples for different workflow types

**references/tool-based-patterns.md**
- Tool-centric skill patterns
- Tool call orchestration
- Error handling for tools
- Examples using specific tools

**references/domain-knowledge-patterns.md**
- Specialized domain skill patterns
- Domain expertise integration
- Terminology and conventions
- Examples for various domains

**references/integration-patterns.md**
- External service integration patterns
- API interaction strategies
- Authentication and error handling
- Examples for common integrations

**references/advanced-features.md**
- Advanced skill capabilities
- Complex workflow patterns
- Dynamic adaptation strategies
- Performance optimization

### general-skill-upgrader Contains:

**SKILL.md** (~506 lines)
- Overview and principles
- 5-phase upgrade workflow
- Upgrade philosophy and approach
- Quality checklist
- Key reminders

**references/upgrade-types.md**
- 22 types of potential upgrades
- Impact and effort assessment
- Value scoring methodology
- Examples for each type

**references/implementation-patterns.md**
- Concrete upgrade implementation patterns
- Before/after examples for each upgrade type
- Backward compatibility strategies
- Testing and verification approaches

**references/examples.md**
- Real upgrade scenarios from start to finish
- Shows analysis → options → implementation
- Multiple skill types covered
- Demonstrates decision-making process

### agentmd-creator Contains:

**SKILL.md** (~326 lines)
- Overview and principles
- 6-step creation workflow
- Platform support and formats
- Quality checklist
- Key reminders

**references/best-practices.md**
- Universal agent config principles
- Structure and organization guidelines
- Tone and communication style
- Quality standards

**references/examples.md**
- Complete agent config examples
- General-purpose agents
- Domain-specific agents (sales, HR, research, etc.)
- Platform-specific examples

**references/platforms.md**
- Platform-specific formats and locations
- Claude Code (.md format)
- Cursor (.cursor/rules/*.mdc)
- Windsurf (.windsurf/rules/*.md)
- Antigravity, JetBrains Junie, GitHub Copilot
- Universal format (AGENTS.md)

**references/use-cases.md**
- Common agent use cases by domain
- Business workflow examples
- Role-specific configurations
- Team vs solo agent considerations

### context-collecter Contains:

**SKILL.md** (~207 lines)
- Overview and data storage structure
- 20 context layer definitions (private + company)
- File format specification (static/dynamic annotations)
- 4 mode workflows (Quick Save, Briefing, Load/Mount, Edit/Delete)
- Cross-layer linking rules
- Expired entry handling

**references/private-categories.md**
- Briefing questions for all 10 private layers
- Two question modes per layer: quick scan and deep interview
- Follow-up probing questions for each category

**references/company-categories.md**
- Briefing questions for all 10 company layers
- Two question modes per layer: quick scan and deep interview
- Follow-up probing questions for each category

---

## Output Format

Skills create structured outputs in designated folders:

### general-research Output

```
.research/
├── react-hooks-best-practices-2025-01-26.md
├── graphql-vs-rest-comparison-2025-01-26.md
└── authentication-methods-2025-01-26.md
```

**Research Report Structure:**
- Executive Summary - key insights and takeaways
- Research Goal - what was being investigated
- Key Findings - organized by topic/category
- Patterns & Consensus - what sources agree/disagree on
- Best Practices - actionable recommendations
- Common Pitfalls - anti-patterns to avoid
- Recommendations - concrete next steps
- Sources - all sources with tier ratings and URLs

**Template options:**
- Minimal: For simple, focused topics (3-5 sources)
- Standard: Most common use case (5-8 sources)
- Extended: Complex topics requiring deep analysis (8+ sources)

### general-skill-refiner Output

```
.tasks/
└── skill-refactoring-general-research-2025-01-26/
    ├── changes.md              # Detailed change log
    ├── analysis.md             # Initial analysis report
    └── before-after-metrics.md # Metrics comparison
```

**Refactoring Report Structure:**
- Analysis Summary - issue counts by priority
- Priority 1 Issues (MUST FIX) - critical violations
- Priority 2 Issues (SHOULD FIX) - quality improvements
- Priority 3 Issues (NICE TO HAVE) - enhancements
- What's Good - positive aspects to preserve
- Changes Made - detailed change log
- Before/After Metrics - line counts, structure improvements
- Quality Verification - checklist results

---

## Contributing

This is a public repository for generic agent skills.

### Contributions Welcome:
- Additional research-focused skills (academic research, market research, competitive analysis)
- Additional quality/analysis skills (code review, documentation review, architecture analysis)
- Additional creation/enhancement skills (skill templates, upgrade patterns, optimization tools)
- Additional agent configuration skills (platform-specific enhancements, domain templates)
- Improvements to existing skills
- Additional reference materials (strategies, templates, criteria, patterns, examples)
- Bug fixes and clarifications
- Examples and use cases
- New upgrade types for general-skill-upgrader
- New patterns for general-skill-maker
- New platform support for agentmd-creator

### Contribution Guidelines:
1. Test skill with real usage scenarios before submitting
2. Follow existing skill structure (5-phase workflow preferred)
3. Include comprehensive references/ subdirectory
4. Use quality criteria from general-skill-refiner to self-review
5. No time estimates (strictly forbidden per Agent Skills guidelines)
6. SKILL.md should be <400 lines (move details to references/)
7. Include examples showing input → output patterns

---

## Coding Conventions

### Markdown Style
- **Headers:** ATX style (`#`, `##`, etc.)
- **Lists:** Consistent indentation (2 spaces for nested)
- **Code blocks:** Fenced with triple backticks and language identifier
- **Emphasis:** `**bold**` for important, `*italic*` for subtle

### Skill Development
- Follow Agent Skills specification from [agentskills.io](https://agentskills.io)
- Include YAML frontmatter with name and description (description is king for triggering)
- Structure: Overview → Guidelines → Examples → Workflow → Special Cases → Quality Checklist → Reminders
- Always include references/ subdirectory (3-5 files recommended):
  - Question templates or strategy guides
  - Examples and patterns
  - Quality criteria or evaluation guides
  - Templates (if needed)
- SKILL.md target: 200-400 lines (details in references/)
- NO time estimates anywhere (strictly forbidden)

### File Naming
- Skills: `general-[topic]` or descriptive name (kebab-case)
- Research outputs: `.research/[topic-slug]-[YYYY-MM-DD].md`
- Task outputs: `.tasks/[task-type]-[name]-[YYYY-MM-DD]/`
- References: descriptive names (e.g., `quality-criteria.md`, `research-strategies.md`)

### Testing Requirements
- Test with real use cases before committing
- Verify output quality (reports, analysis)
- Check that reference files are comprehensive and useful
- Run through general-skill-refiner quality criteria
- Verify no time estimates anywhere (grep for "minut", "hour", etc.)
- Ensure SKILL.md is reasonable length (<400 lines)
- Test that triggers work correctly

---

## Design Philosophy

### Systematic Over Improvised
Good work follows proven methodologies. These skills embody systematic approaches:
- Research isn't just "Googling" - it's structured multi-source analysis with quality evaluation
- Skill improvement isn't just "making it better" - it's systematic quality analysis with prioritized fixes
- Skill creation isn't just "writing code" - it's guided briefing with pattern selection and validation
- Skill enhancement isn't just "adding features" - it's business-driven value identification and prioritization
- Agent configuration isn't just "writing rules" - it's platform-aware, domain-specific structured generation
- Frameworks and checklists ensure nothing important is missed

### Quality Compounds
High-quality inputs → high-quality outputs:
- Research based on credible sources → reliable insights
- Well-structured skills → better agent performance
- Systematic approaches → consistent results

Quality isn't achieved once - it requires continuous attention and improvement.

### Five Core Capabilities

1. **Knowledge Gathering** (general-research)
  - How do we find reliable information?
  - How do we evaluate source credibility?
  - How do we synthesize insights from multiple sources?
  - How do we document findings systematically?

2. **Quality Assurance** (general-skill-refiner)
  - How do we evaluate skill quality?
  - What makes a skill effective vs ineffective?
  - How do we prioritize improvements?
  - How do we verify changes improved quality?

3. **Skill Creation** (general-skill-maker)
  - How do we design effective skills?
  - What patterns and structures work best?
  - How do we ensure completeness and quality?
  - What references and examples are needed?

4. **Functional Enhancement** (general-skill-upgrader)
  - How do we identify valuable upgrades?
  - What new capabilities would add most value?
  - How do we evolve skills without breaking them?
  - How do we prioritize enhancements?

5. **Agent Configuration** (agentmd-creator)
  - How do we configure agents for specific purposes?
  - What platform-specific formats are needed?
  - How do we define agent boundaries and guidelines?
  - What domain knowledge should be included?

### Evidence Over Assumptions
Both skills emphasize evidence:
- Research: Cross-reference claims, use tier system for sources, mark uncertainties
- Skill refining: Use metrics (line counts, structure), concrete examples, before/after comparisons
- Don't assume - measure, verify, document

### Progressive Disclosure
Information should be provided at the right level:
- SKILL.md: High-level workflow and key principles (200-400 lines)
- References: Detailed strategies, examples, templates (organized by topic)
- User gets what they need, when they need it, without overwhelm

---

## Related Resources

- [Agent Skills Specification](https://agentskills.io) - Format documentation and best practices
- [Agent Skills Complete Guide](https://github.com/agentskills/agent-skills) - Comprehensive guide
- [CampusAI](https://campus.ai) - Project creator

---

## Examples & Use Cases

### general-research Examples
- "Research React state management best practices" → Compares useState, Context, Zustand, Redux
- "Compare GraphQL vs REST APIs" → Multi-source analysis with pros/cons by context
- "Find authentication methods for web apps" → OAuth, JWT, sessions comparison
- "Research Kubernetes deployment strategies" → Official docs + company blogs + community
- "What are best practices for Agent Skills?" → Study agentskills.io + examples + community

### general-skill-refiner Examples
- Analyze skill with 500+ line SKILL.md → Identify template bloat, move to references
- Find 15 time estimates → Remove all (Priority 1 violation)
- Detect question explosion → Consolidate to 4 core + references
- Review skill structure → Add missing phase goals and quality checklist
- Check newly created skill → Verify against quality criteria, fix Priority 1 issues

### general-skill-maker Examples
- "Create skill for API documentation generation" → Tool-based skill with file analysis and doc generation
- "Build skill for code review" → Workflow-based skill with analysis phases and quality checks
- "Make skill for database migration" → Hybrid skill combining workflow + tool patterns
- "Create skill for customer onboarding" → Domain knowledge skill with business rules
- "Build skill for CI/CD pipeline setup" → Integration pattern skill with external service calls

### general-skill-upgrader Examples
- Upgrade research skill → Add source caching, citation export, summary generation options
- Enhance refiner skill → Add before/after diff viewer, automated fix suggestions, batch processing
- Evolve code review skill → Add security scanning, performance checks, accessibility audits
- Upgrade documentation skill → Add diagram generation, multi-format export, version comparison
- Enhance onboarding skill → Add progress tracking, reminder scheduling, stakeholder notifications

### agentmd-creator Examples
- "Create sales agent for Cursor" → Sales CRM agent with deal tracking, objection handling, email drafting
- "Setup research assistant for Claude" → Research agent with source evaluation, synthesis capabilities
- "Make HR coordinator agent for Windsurf" → HR workflows, candidate screening, onboarding guidance
- "Configure customer service agent" → Support agent with ticket handling, knowledge base search
- "Create general-purpose agent for team" → Universal AGENTS.md for any platform with team guidelines

---

## License

Apache 2.0

---

**Made by CampusAI** | **Version 2.0.0** - Generic Skills Edition
