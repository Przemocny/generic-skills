# Contributing to Generic Skills

Thank you for your interest in contributing! This repository welcomes contributions that improve systematic research, skill management, and agent configuration capabilities for Claude AI users.

## How to Contribute

### 1. Report Issues

Found a bug or have a suggestion?
- Open an [Issue](../../issues)
- Describe the problem or idea clearly
- Include examples if applicable

### 2. Improve Existing Skills

You can help by:
- Adding research strategies to `references/research-strategies.md`
- Expanding quality criteria in `references/quality-criteria.md`
- Improving refactoring patterns in `references/refactoring-patterns.md`
- Adding upgrade types to `references/upgrade-types.md`
- Adding platform support to `references/platforms.md` in agentmd-creator
- Fixing typos or clarifying instructions in `SKILL.md` files
- Adding examples of good workflows

### 3. Create New Skills

We welcome new generic skills! Consider:

**Additional Research Skills:**
- `academic-research` - Systematic academic literature review
- `market-research` - Market analysis and competitive intelligence
- `technical-research` - Deep technical documentation research
- `trend-research` - Industry trend analysis

**Additional Quality/Analysis Skills:**
- `code-reviewer` - Systematic code quality review
- `documentation-reviewer` - Documentation quality analysis
- `architecture-reviewer` - System architecture evaluation
- `security-analyzer` - Security assessment framework

**Additional Creation/Enhancement Skills:**
- `skill-template-generator` - Generate skill templates from examples
- `skill-optimizer` - Performance optimization for skills
- `reference-generator` - Automatic reference material creation
- `pattern-detector` - Identify common patterns in skills

**Additional Agent Configuration Skills:**
- `team-agent-configurator` - Multi-agent team configuration
- `domain-agent-customizer` - Domain-specific agent customization
- `platform-migrator` - Migrate configs between platforms
- `agent-optimizer` - Optimize agent performance

### New Skill Requirements

Each new skill must include:
1. `SKILL.md` with proper YAML frontmatter (name, description)
2. Clear phase-based or step-based workflow
3. Reference files (3-10 supporting documents)
4. Quality checklist tailored to skill purpose
5. Examples showing input → output patterns
6. No time estimates (strictly forbidden)

### 4. Pull Request Process

1. **Fork** the repository
2. **Create a branch** for your contribution:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes**:
   - Follow existing code style and structure
   - Test with Claude to ensure it works
   - Update documentation if needed
4. **Commit** with clear messages:
   ```bash
   git commit -m "Add research strategy for academic papers"
   ```
5. **Push** to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```
6. **Open a Pull Request** with:
   - Clear description of what changed and why
   - Examples of the improvement in action
   - Reference any related issues

## Contribution Guidelines

### Writing Style

- **Be systematic** - Follow proven methodologies and frameworks
- **Use examples** - Show input → output patterns
- **Keep it concise** - SKILL.md should be 200-400 lines (details in references/)
- **Mark uncertainties** - Note what's not yet perfected
- **No time estimates** - Strictly forbidden per Agent Skills guidelines

### Skill Design Principles

1. **Systematic over ad-hoc** - Follow proven methodologies
2. **Multi-source verification** - Cross-reference information
3. **Quality-focused** - Prioritize quality over quantity
4. **Evidence-based** - Concrete data over assumptions
5. **Structured output** - Well-organized documentation
6. **Continuous improvement** - Regular refinement cycles

### Code of Conduct

- Be respectful and constructive
- Focus on improving the skills, not criticizing contributors
- Assume good intent
- Help others learn

## Testing Your Contribution

Before submitting:
1. Install your changed/new skill in Claude
2. Test with real scenarios
3. Verify the output format is correct
4. Check that reference files are properly organized
5. Ensure documentation is clear
6. Run through general-skill-refiner to check quality

## Questions?

- Open a [Discussion](../../discussions) for general questions
- Check existing [Issues](../../issues) for similar questions
- Review [AGENTS.md](../AGENTS.md) for skill structure details

## Recognition

Contributors will be:
- Listed in release notes
- Acknowledged in commit history
- Appreciated by the community 🙏

---

**Remember:** The goal is to provide systematic frameworks that help people work more effectively with AI agents. Every contribution that advances this goal is valuable.
