# Generic Skills

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Claude Skills](https://img.shields.io/badge/Claude-Agent_Skills-purple)](https://agentskills.io)
[![CampusAI](https://img.shields.io/badge/Made_by-CampusAI-orange)](https://campus.ai)

Reusable Agent Skills for Claude AI that enhance research capabilities, skill management, and agent configuration creation. These skills provide systematic frameworks for gathering knowledge from the internet, creating and improving agent skills, and generating AI agent configuration files.

## What are Generic Skills?

Generic Skills use **systematic methodologies** to create structured outputs that help users:
- Conduct thorough internet research with multi-source verification and quality evaluation
- Create effective agent skills through guided briefing and pattern selection
- Improve existing skills by identifying issues and implementing prioritized fixes
- Enhance skills with new features and capabilities through business-driven upgrades
- Generate platform-specific agent configuration files for various AI tools

These skills are **quality-focused by design** - they follow proven methodologies, use structured approaches with phases and checklists, and prioritize evidence over assumptions.

## Available Skills

### 🔍 general-research
Conducts systematic internet research through strategic questioning and multi-source analysis. Uses tier system to evaluate source credibility, cross-references claims, identifies patterns and consensus, and produces structured reports in `.research/`.

**Use when:**
- User needs to gather knowledge from the internet
- Mentions finding best practices or comparing solutions
- Asks to research, investigate, or evaluate topics
- Wants to understand industry trends thoroughly

**Output:** Research report in `.research/[topic-slug]-[date].md`

### 🎨 general-skill-maker
Creates effective agent skills through guided briefing process. Asks strategic questions, identifies appropriate patterns (workflow/tool-based/domain-knowledge/hybrid), generates SKILL.md with comprehensive references, and ensures quality through validation.

**Use when:**
- User wants to create a new skill from scratch
- Mentions "create skill", "make skill", "build skill"
- Needs guidance on skill structure and patterns
- Wants to design skill architecture

**Output:** New skill directory in `skills/[skill-name]/` with SKILL.md and references

### 🔧 general-skill-refiner
Analyzes and refines agent skills through systematic quality review. Identifies issues using quality criteria, prioritizes fixes (MUST/SHOULD/NICE), gathers user feedback, implements improvements, and tracks changes with metrics.

**Use when:**
- User wants to review, improve, or refactor existing skills
- Asks about skill quality or audit
- Mentions "review skill", "improve skill", "optimize skill"
- After creating new skill to ensure quality

**Output:** Refactored skill + change report in `.tasks/skill-refactoring-[skill-name]-[date]/`

### ⚡ general-skill-upgrader
Strategically enhances agent skills through business-driven improvements. Analyzes business purpose, identifies upgrade opportunities across 22 categories, presents 1-2 most valuable options, and implements selected enhancements iteratively.

**Use when:**
- User wants to add features or capabilities to existing skill
- Asks to enhance, upgrade, or expand skill functionality
- Mentions "upgrade skill", "make skill better", "add features"
- Skill works well but could do more

**Output:** Enhanced skill + upgrade report in `.tasks/skill-upgrade-[skill-name]-[date]/`

### 🤖 agentmd-creator
Creates AI agent configuration files for general-purpose and business-domain agents. Supports multiple platforms (Claude Code, Cursor, Windsurf, etc.), generates platform-specific formats, includes domain knowledge and business rules, and defines clear boundaries.

**Use when:**
- User wants to create agent configuration file
- Needs to set up AI assistant for specific purpose or domain
- Mentions "create agent config", "make AGENTS.md", "setup AI assistant"
- Wants to configure agent for specific role (sales, HR, research, etc.)

**Output:** Platform-specific configuration file(s) like AGENTS.md, CLAUDE.md, .cursor/rules/*.mdc

### 🧠 context-collecter
Collects, saves and loads personal and company context across 20 information layers. Manages private context (identity, values, goals, family, health, finances, hobbies, lifestyle, development, relationships) and company context (company, role, team, products, clients, processes, strategy, technology, finances, challenges). Always communicates in Polish.

**Use when:**
- User wants to save personal or company information for future sessions
- Mentions "remember that...", "save that...", "note...", "add to context..."
- Wants to collect context through guided interview ("gather context about...", "conduct interview about...")
- Needs to load saved context ("mount context...", "load context about...", "what do you know about me...")
- Wants to edit existing context entries ("correct...", "change that...", "remove entry...")

**Output:** Context files in `~/.claude/context/private/` and `~/.claude/context/company/`

## Installation

### In Claude Code

1. **Register the marketplace:**
   ```bash
   /plugin marketplace add Przemocny/generic-skills
   ```

2. **Install the plugin:**
   ```bash
   /plugin install generic-skills@generic-skills
   ```

3. **Use the skills** by mentioning them in conversation:
   - "Research best practices for..." (triggers general-research)
   - "Create skill for..." (triggers general-skill-maker)
   - "Review this skill" (triggers general-skill-refiner)
   - "Upgrade skill" (triggers general-skill-upgrader)
   - "Create agent config" (triggers agentmd-creator)
   - "Remember that..." / "Save that..." (triggers context-collecter)

### Manual Installation

Clone the repository and copy skills to your local `~/.claude/skills/` directory:

```bash
git clone https://github.com/Przemocny/generic-skills.git
cd generic-skills
cp -r skills/* ~/.claude/skills/
```

## How It Works

All skills follow systematic, phase-based workflows:

### general-research (5 phases)
1. **Understanding** - Clarify what's needed and why
2. **Strategic Questions** - Narrow scope and direction (4 core questions)
3. **Research** - Multi-source gathering with tier evaluation
4. **Creating Report** - Structured documentation with analysis
5. **Review & Feedback** - Present findings and next steps

### general-skill-maker (6 steps)
1. **Briefing Questions** - Understand requirements strategically
2. **Load References** - Gather relevant patterns and practices
3. **Plan Structure** - Design skill architecture and workflow
4. **Generate** - Create SKILL.md and references files
5. **Apply Best Practices** - Ensure quality and completeness
6. **Present** - Show final skill and usage instructions

### general-skill-refiner (5 phases)
1. **Read & Understand** - Analyze skill structure thoroughly
2. **Critical Analysis** - Identify issues using quality criteria
3. **Present & Gather Feedback** - Show findings, get approval
4. **Refactor** - Systematically implement improvements
5. **Verify & Report** - Ensure quality and document changes

### general-skill-upgrader (5 phases)
1. **Understand Business Goal** - Identify core purpose and value
2. **Identify Opportunities** - Scan 22 upgrade types for potential
3. **Present Options** - Show 1-2 most valuable upgrades
4. **Implement** - Execute selected enhancement(s)
5. **Verify & Report** - Test and document improvements

### agentmd-creator (6 steps)
1. **Briefing Questions** - Understand agent purpose and domain
2. **Load References** - Platform-specific guides and examples
3. **Generate Structure** - Create appropriate config format
4. **Customize Content** - Add domain knowledge and guidelines
5. **Apply Best Practices** - Ensure quality and completeness
6. **Present** - Show config and installation instructions

### context-collecter (4 modes)
1. **Quick Save** - Save information to the right context layer with confirmation
2. **Briefing** - Guided interview to collect context (deep or quick scan)
3. **Load/Mount** - Load and present saved context layers silently
4. **Edit/Delete** - Find, modify, or remove specific context entries

### Key Principles

- **Systematic Over Ad-hoc** - Follow proven methodologies and frameworks
- **Multi-Source Verification** - Cross-reference information, evaluate credibility
- **Quality-Focused** - Prioritize quality over quantity in all outputs
- **Analysis Over Collection** - Synthesize insights, not just gather data
- **Continuous Improvement** - Regular analysis and refinement
- **Evidence-Based** - Concrete data and examples over assumptions
- **Structured Output** - Well-organized, easy to navigate documentation

## Output Format

Skills create structured outputs in designated folders:

```
.research/
└── topic-name-2026-01-26.md          # Research reports

.tasks/
├── skill-refactoring-name-2026-01-26/ # Refactoring reports
└── skill-upgrade-name-2026-01-26/     # Upgrade reports

skills/
└── new-skill-name/                    # New skills
    ├── SKILL.md
    └── references/

AGENTS.md                              # Agent configurations
CLAUDE.md
.cursor/rules/*.mdc

~/.claude/context/
├── private/                           # 10 private context layers
│   ├── tozsamosc.md
│   ├── wartosci.md
│   └── ...
└── company/                           # 10 company context layers
    ├── firma.md
    ├── moja-rola.md
    └── ...
```

Each output includes:
- Structured documentation with clear sections
- Analysis and synthesis (not just raw data)
- Practical, actionable recommendations
- Evidence and sources for all claims
- Quality verification results

## Skill Workflows

### Independent Usage

All skills can be used independently:
- **general-research:** Anytime you need information from internet
- **general-skill-maker:** Create new skills from scratch
- **general-skill-refiner:** Review any existing skill
- **general-skill-upgrader:** Enhance existing skills functionally
- **agentmd-creator:** Generate agent configuration files

### Complementary Usage

Skills work together in powerful workflows:

**Complete Skill Development Cycle:**
1. Research best practices (general-research)
2. Create skill (general-skill-maker)
3. Refine quality (general-skill-refiner)
4. Upgrade features (general-skill-upgrader)
5. Repeat 3-4 for continuous improvement

**Agent Configuration Workflow:**
1. Research domain best practices (general-research)
2. Create agent config (agentmd-creator)
3. Test and gather feedback
4. Research improvements if needed
5. Update config based on findings

## Philosophy

These skills operate on the principle that **quality compounds**. High-quality inputs lead to high-quality outputs. Quality isn't achieved once - it requires continuous attention and improvement through:

- **Systematic methodologies** over improvised approaches
- **Evidence-based decisions** with multi-source verification
- **Progressive disclosure** of information at the right level
- **Structured frameworks** ensuring nothing important is missed
- **Continuous refinement** through regular analysis and improvement

## Contributing

This is a public repository for generic agent skills. Contributions are welcome for:
- Additional research-focused skills (academic research, market research, competitive analysis)
- Additional quality/analysis skills (code review, documentation review, architecture analysis)
- Additional creation/enhancement skills (skill templates, upgrade patterns, optimization tools)
- Additional agent configuration skills (platform-specific enhancements, domain templates)
- Improvements to existing skills
- Additional reference materials (strategies, templates, criteria, patterns, examples)
- Bug fixes and clarifications
- Examples and use cases

See [CONTRIBUTING.md](.github/CONTRIBUTING.md) for details.

## License

Apache 2.0

## Related

- [Agent Skills Specification](https://agentskills.io)
- [Agent Skills Complete Guide](https://github.com/agentskills/agent-skills)
- [CampusAI](https://campus.ai)
