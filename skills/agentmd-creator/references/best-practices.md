# Universal Best Practices

## Core Principles

### 1. Minimalism > Completeness

**Why:** Context window is limited. Every token matters.

**Rule:** Keep rules < 300 lines total

**Examples:**
- ✅ "Use TypeScript strict mode"
- ❌ "We prefer using TypeScript in our codebase because it provides type safety which helps catch bugs early in development and makes refactoring easier..."

### 2. Reference, Don't Copy

**Why:** Code changes, copied snippets become outdated

**Rule:** Use file:line references instead of code copies

**Examples:**
- ✅ "Follow auth pattern in src/lib/auth.ts:45-67"
- ❌ [Paste 50 lines of authentication code]

### 3. Commands with Flags, Not Tool Names

**Why:** AI needs executable commands, not abstract mentions

**Examples:**
- ✅ `test: npm test -- --coverage --watch`
- ❌ `Use Jest for testing`

### 4. Specific > Generic

**Why:** Generic advice gets ignored, specific instructions get followed

**Examples:**
- ✅ "API responses must include rate limit headers (X-RateLimit-*)"
- ❌ "Write good API code"

### 5. Examples > Explanations

**Why:** Show don't tell. Real code demonstrates better than text

**Examples:**
- ✅ Include 3-5 line code snippet showing pattern
- ❌ Paragraph explaining what pattern should look like

---

## Essential Sections

Every good configuration file includes these sections:

### 1. Role Definition

```markdown
## Role
[Specific role description with domain expertise]

Example:
Research specialist conducting systematic information gathering with focus on business intelligence and market analysis
```

**Why:** AI needs clear role definition to understand scope and approach

### 2. Tools and Systems

```markdown
## Tools and Systems
- CRM: Salesforce (credentials in 1Password)
- Documentation: Notion workspace
- Communication: Slack channels (#sales, #support)
- Calendar: Google Calendar (work@company.com)
```

**Why:** AI needs to know which tools to use and how to access them

### 3. Knowledge Base Locations

```markdown
## Knowledge Base
- Company policies: /docs/policies/
- Process documentation: /docs/processes/
- Templates: /templates/
- Historical data: /data/archive/
- Team contacts: /team-directory.md
```

**Why:** AI knows where to find reference information

### 4. Important Notes (Domain-Specific Gotchas)

```markdown
## Important Notes
- Client data is confidential - never share externally
- Response time SLA: 24 hours for standard, 4 hours for urgent
- All financial data requires CFO approval before sharing
- Use templates in /templates/ for consistency
```

**Why:** Domain-specific warnings prevent mistakes

### 5. Communication Style

```markdown
## Communication Style
- Tone: Professional and empathetic
- Format: Clear, structured, bullet points for action items
- Timing: Response within 2 hours during business hours
- Signature: Always include [name, title, contact]
```

**Why:** AI knows how to communicate appropriately for the domain

### 6. Boundaries (Three-tier System)

```markdown
## Boundaries

### Always Do
- Run tests after changes
- Use TypeScript strict mode
- Validate all inputs
- Log errors to Sentry

### Ask First
- Database schema changes
- New npm dependencies
- Breaking API changes
- Performance optimizations

### Never Do
- Commit secrets or API keys
- Modify generated files (migrations, prisma client)
- Skip type definitions
- Use `any` type
```

**Why:** Clear boundaries prevent dangerous actions and establish workflow

---

## What to Avoid

### Anti-pattern 1: Code Style Rules

**Bad:**
```markdown
## Code Style
- Use 2 spaces for indentation
- Single quotes for strings
- Semicolons required
- Max line length 100
- Trailing commas
- Arrow functions
[... 50 more rules]
```

**Why:** Use linters/formatters instead. Don't waste context.

**Good:**
```markdown
## Code Style
Enforced by ESLint + Prettier (see .eslintrc.json)
Run: npm run lint:fix
```

### Anti-pattern 2: Generic Best Practices

**Bad:**
```markdown
- Write clean code
- Use meaningful variable names
- Add comments where needed
- Follow SOLID principles
- Write maintainable code
```

**Why:** AI already knows this. Be project-specific.

### Anti-pattern 3: Lengthy Explanations

**Bad:**
```markdown
## Authentication
We use NextAuth.js for authentication in this project. NextAuth.js is a complete open source authentication solution for Next.js applications. It supports multiple providers including OAuth, email/passwordless, and credentials. The configuration is stored in the auth.ts file which exports the NextAuth configuration...
[... 200 more lines]
```

**Good:**
```markdown
## Authentication
NextAuth.js (config in /lib/auth.ts)
- OAuth providers: Google, GitHub
- Session strategy: JWT
- Callback URLs in .env.local
```

### Anti-pattern 4: Outdated Information

**Problem:** References to old versions, deprecated patterns, removed features

**Solution:** Include version numbers, regular reviews (quarterly)

### Anti-pattern 5: Missing Boundaries

**Problem:** No "Never" section with dangerous operations

**Solution:** Always include explicit "Never" section with destructive actions to avoid

---

## Token Efficiency Techniques

### Progressive Disclosure

**Instead of monolithic:**
```
AGENTS.md (6000 tokens) - always loaded
```

**Use modular:**
```
AGENTS.md (1000 tokens) - always loaded
docs/frontend.md (2000 tokens) - when needed
docs/backend.md (2000 tokens) - when needed
docs/db.md (1000 tokens) - when needed
```

**Savings:** Load only what's needed per session

### Reference External Documentation

**Bad (wasteful):**
```markdown
## React Best Practices
[500 lines explaining React patterns]
```

**Good (efficient):**
```markdown
## React Patterns
Follow official docs: https://react.dev/learn
Project-specific: See /docs/components.md for our patterns
```

### Use Tools, Not Text

**Prefer:**
- ESLint/Prettier for code style
- Husky for pre-commit hooks
- TypeScript for type safety
- Scripts for workflows

**Over:**
- Long rules describing style
- Manual reminders to run commands
- Type annotations in rules

---

## File Size Guidelines

| File Type | Recommended Size | Maximum |
|-----------|------------------|---------|
| AGENTS.md | < 200 lines | 300 lines |
| CLAUDE.md | < 60 lines | 300 lines |
| .cursor/rules/*.mdc | < 50 lines | 100 lines |
| .windsurf/rules/*.md | < 6000 chars | 6000 chars |

**Strategy:** When exceeding limits, split into modules or use nested files

---

## Quality Checklist

Before finalizing a configuration file:

**Content:**
- [ ] Minimal (< 300 lines or modular equivalent)
- [ ] Specific (no generic advice)
- [ ] Actionable (commands with flags)
- [ ] Current (versions up-to-date)
- [ ] Unique (info not available elsewhere)

**Structure:**
- [ ] Clear sections with headings
- [ ] Easy to scan
- [ ] Uses file:line references
- [ ] Includes code examples
- [ ] Has 3-tier boundaries (always/ask/never)

**Technical:**
- [ ] Correct format for platform
- [ ] Proper file location
- [ ] Valid YAML frontmatter (if applicable)
- [ ] Committed to repo (except .local files)

---

## Common Mistakes to Avoid

1. **Too long** - > 300 lines without modularization
2. **Too generic** - Advice that applies to all projects
3. **Too outdated** - Old versions or deprecated patterns
4. **Too verbose** - Explanations instead of examples
5. **Missing boundaries** - No "Never" section
6. **Copying code** - Pasting code instead of referencing
7. **Tool descriptions** - "Use Jest" instead of "npm test"
8. **No versions** - "React" instead of "React 18.2"
9. **No structure** - Wall of text without sections
10. **Duplicate info** - Same info in multiple places

---

## Maintenance

Configuration files should be:

- **Living documents** - Updated as project evolves
- **Version controlled** - Committed to git
- **Reviewed regularly** - Quarterly audits
- **Team-owned** - Not single-person responsibility

**Review checklist:**
- Remove obsolete content
- Update versions
- Add new gotchas discovered
- Consolidate duplicate information
- Test with new team members
