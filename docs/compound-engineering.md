# Compound Engineering in oh-my-opencode-slim-compound

This fork adds compound engineering capabilities to oh-my-opencode-slim, making each coding task progressively easier through systematic knowledge accumulation.

## Philosophy

> "Each unit of engineering work should make subsequent units easier—not harder."

**Compounding Effect:**
- First time solving a problem: 30 minutes
- With documented solution: 3 minutes
- **Time saved: 90% (9-10x faster)**

## The 4-Step Compound Loop

### 1. PLAN (with Knowledge Base Search)
**Agent: @workflow-planner**

Before starting any task, the planner automatically:
- Searches `~/.config/opencode/compound-knowledge/` for similar problems
- References past solutions for 90% time savings
- Identifies applicable patterns
- Notes anti-patterns to avoid
- Estimates time based on history

**Example:**
```
User: "Add OAuth authentication"

@workflow-planner:
✓ Found similar solution: oauth-integration-pattern.md
✓ Original time: 120 minutes
✓ With pattern: ~15 minutes (87.5% savings)
✓ References: 3 related solutions
```

### 2. WORK (Build the Solution)
**Agent: @orchestrator**

Standard development workflow:
- Create TODO list for multi-step tasks
- Delegate to specialized agents (explore, librarian, etc.)
- Use LSP tools for refactoring
- Verify with lsp_diagnostics
- Mark TODOs complete

### 3. REVIEW (Verify Quality)
**Agent: @code-simplicity-reviewer**

Code review and validation:
- Check lsp_diagnostics after changes
- Deep review with code-simplicity-reviewer
- Verify tests pass
- Validate solution meets requirements

### 4. COMPOUND (Document for Future)
**Agent: @workflow-compounder**

Automatically documents solved problems:
- Detects completion signals ("problem solved", "tests passing")
- Creates solution document with YAML frontmatter
- Extracts reusable patterns
- Saves to correct category
- Makes next occurrence 90% faster

## Knowledge Base Structure

Location: `~/.config/opencode/compound-knowledge/`

```
compound-knowledge/
├── solutions/              # Categorized problem solutions
│   ├── build-errors/      # Build system, compilation
│   ├── test-failures/     # Test suite problems
│   ├── runtime-errors/    # Production runtime issues
│   ├── performance-issues/ # Optimization, slow queries
│   ├── database-issues/   # Migrations, schema, queries
│   ├── security-issues/   # Vulnerabilities, CVEs
│   ├── ui-bugs/          # Frontend rendering, UX
│   ├── integration-issues/ # API, third-party integrations
│   └── logic-errors/      # Business logic bugs
├── patterns/
│   ├── successful/        # Patterns that worked well
│   └── anti-patterns/     # Patterns to avoid
└── learnings/
    ├── architecture/      # System design insights
    ├── frameworks/        # Framework-specific knowledge
    └── tools/            # Tool usage, CLI tricks
```

## New Agents

### @workflow-planner
- **Purpose**: Search knowledge base before starting work
- **Model**: Opus 4.5
- **Output**: Enhanced plan with past solution references

### @workflow-compounder
- **Purpose**: Auto-document solutions after completing work
- **Model**: Opus 4.5
- **Output**: Solution documents in knowledge base

### Existing Agents (Enhanced)
- **@orchestrator**: Now includes compound engineering workflow
- **@code-simplicity-reviewer**: Part of review phase

## Usage Examples

### Example 1: First Time Problem

```bash
User: "Fix authentication bug - users logged out after 5 minutes"

@workflow-planner: Searched knowledge base, no similar solution found
[Work phase: 45 minutes to solve]
@workflow-compounder: Documented to solutions/security-issues/session-timeout-fix.md

Result:
- Time: 45 minutes (solve) + 5 minutes (document) = 50 minutes total
- Knowledge base now contains the solution
```

### Example 2: Second Occurrence

```bash
User: "Another project has the same 5-minute logout issue"

@workflow-planner:
✓ Found: session-timeout-fix.md
✓ Original time: 45 minutes
✓ Estimated with KB: 5 minutes

[Work phase: Apply documented solution - 5 minutes]

Result:
- Time: 5 minutes
- Time saved: 90% (45 min → 5 min)
- Compound return: 9x faster
```

### Example 3: Pattern Extraction

```bash
# After solving 3 similar authentication issues...

@workflow-compounder:
✓ Pattern detected across 3 solutions
✓ Created: patterns/successful/oauth-integration-pattern.md
✓ Cross-referenced 3 solutions

Future authentication tasks:
- Check pattern first
- Apply proven approach
- Avoid documented pitfalls
```

## Configuration

### User Config: `~/.config/opencode/oh-my-opencode-slim.json`

```json
{
  "agents": {
    "orchestrator": {
      "model": "vertex-claude/claude-opus-4-5@20251101"
    },
    "workflow-planner": {
      "model": "vertex-claude/claude-opus-4-5@20251101"
    },
    "workflow-compounder": {
      "model": "vertex-claude/claude-opus-4-5@20251101"
    },
    "code-simplicity-reviewer": {
      "model": "vertex-claude/claude-opus-4-5@20251101"
    }
  }
}
```

### Disable Compound Engineering (if needed)

```json
{
  "disabled_agents": ["workflow-planner", "workflow-compounder"]
}
```

## Auto-Trigger Conditions

The @workflow-compounder automatically activates when detecting:
- "problem solved" / "bug fixed" / "issue resolved"
- "test passing" / "tests passing" / "all tests pass"
- "feature complete" / "feature working" / "implementation done"
- "working now" / "fixed it" / "resolved"
- TODO list fully completed
- Error messages no longer appearing

## Solution Document Template

Each solution includes:

```markdown
---
title: "Brief description of problem"
category: integration-issues
date: 2026-01-17
severity: medium
tags: [oauth, authentication, session]
time_to_solve: 45min
related_solutions:
  - ../security-issues/session-management.md
---

## Problem Symptom
[What the user observed]

## Investigation Steps Tried
[What didn't work and why]

## Root Cause
[Technical explanation]

## Solution
[Step-by-step fix with code examples]

## Prevention Strategies
1. Test cases to add
2. Code review checklist items
3. Tool configuration changes
```

## Searching the Knowledge Base

### By Problem Symptom
```bash
grep -r "401 Unauthorized" ~/.config/opencode/compound-knowledge/solutions/
```

### By Tag
```bash
grep -r "tags: \\[.*oauth.*\\]" ~/.config/opencode/compound-knowledge/
```

### By Category
```bash
ls ~/.config/opencode/compound-knowledge/solutions/integration-issues/
```

### By Date
```bash
grep -r "date: 2026-01" ~/.config/opencode/compound-knowledge/
```

## Time Allocation

**Compound Engineering Ratio: 80/20**
- 80% of time: Planning, Review, Documentation
- 20% of time: Actual coding

This inverted ratio (compared to traditional development) yields compounding returns:
- More planning = fewer mistakes
- Better review = higher quality
- Good documentation = 90% time savings next time

**Example:**
- 30-minute fix: 3 minutes documenting (10%)
- 2-hour feature: 12 minutes documenting (10%)

## Measuring Compound Returns

Track effectiveness over time:

```bash
# Count total solutions
find ~/.config/opencode/compound-knowledge/solutions/ -name "*.md" | wc -l

# Solutions by category
for cat in ~/.config/opencode/compound-knowledge/solutions/*/; do
  echo "$(basename $cat): $(find $cat -name "*.md" | wc -l)"
done

# Most common tags
grep -r "^tags:" ~/.config/opencode/compound-knowledge/solutions/ | \
  sed 's/.*\\[\\(.*\\)\\]/\\1/' | tr ',' '\\n' | sort | uniq -c | sort -rn
```

## Benefits Over Time

### Week 1
- Building knowledge base
- 10% overhead for documentation
- No time savings yet

### Month 1
- 5-10 solutions documented
- Start seeing 30-50% time savings on recurring issues
- Patterns beginning to emerge

### Quarter 1
- 20-30 solutions documented
- 5-10 patterns extracted
- 60-80% time savings on covered areas
- Team velocity increasing

### Year 1
- 100+ solutions documented
- 20+ patterns extracted
- 90% time savings on most issues
- Knowledge becomes competitive advantage

## Integration with Upstream

This fork tracks upstream oh-my-opencode-slim:
- Periodic merges from alvinunreal/oh-my-opencode-slim
- Compound engineering features are additive
- See FORK_WORKFLOW.md for sync strategy

## Related Documentation

- [Compound Engineering Research](compound-engineering-research.md) - Original concept research
- [Implementation Plan](compound-engineering-implementation-plan.md) - Detailed implementation strategy
- [Code Simplification Comparison](code-simplification-comparison.md) - Simplifier vs reviewer
- [Fork Workflow](../FORK_WORKFLOW.md) - Upstream sync strategy
- [Knowledge Base README](~/.config/opencode/compound-knowledge/README.md) - KB documentation

## Success Stories

### OneNote Graph API Permissions (Real Example)
- **First occurrence**: 90 minutes (investigation + workaround)
- **Documented**: solutions/integration-issues/onenote-graph-api-permissions.md
- **Next occurrence**: ~10 minutes (read doc + apply)
- **Time saved**: 89% reduction

### Multi-Platform Knowledge Aggregation (Real Pattern)
- **First occurrence**: 45 minutes (Confluence + JIRA + SharePoint + OneNote)
- **Pattern extracted**: patterns/successful/multi-platform-knowledge-aggregation.md
- **Next occurrence**: 5 minutes (follow pattern)
- **Time saved**: 89% reduction

## Privacy

Knowledge base is **local only**:
- Stored at `~/.config/opencode/compound-knowledge/`
- Not synced to cloud by default
- Not committed to repositories

Optional sync methods:
1. Symlink to cloud folder (Dropbox, OneDrive)
2. Git version control
3. Manual copy between machines

## Getting Started

1. Install oh-my-opencode-slim-compound (this fork)
2. Knowledge base auto-created on first use
3. After solving your first problem, @workflow-compounder documents it
4. On second similar problem, @workflow-planner finds it
5. Measure time saved
6. Continue compounding

**First problem solved?** The orchestrator will automatically trigger @workflow-compounder to document it.

## Troubleshooting

### Planner not finding solutions
- Check KB path: `~/.config/opencode/compound-knowledge/`
- Verify solution documents have YAML frontmatter
- Try different search terms (tags, categories)

### Compounder not auto-triggering
- Check for completion signals in conversation
- Manually call: `@workflow-compounder "Document the solution we just completed"`
- Verify agent not disabled in config

### Time savings lower than expected
- Ensure solution documents are detailed enough
- Check cross-references are accurate
- Verify code examples are included
- Update related_solutions links

## Contributing

This fork is maintained separately from upstream oh-my-opencode-slim:
- Report issues at: https://github.com/isc-tdyar/oh-my-opencode-slim-compound/issues
- Upstream: https://github.com/alvinunreal/oh-my-opencode-slim

---

**Remember**: Every problem solved without documentation is a missed opportunity to make the next problem easier. Compound your knowledge!
