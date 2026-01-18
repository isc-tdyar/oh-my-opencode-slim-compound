# Code Simplification: Claude Code vs oh-my-opencode-slim

## Comparison

### Claude Code Plugin: `code-simplifier`

**Purpose:** Autonomous, proactive code refinement
**Model:** Opus
**Behavior:** Operates **automatically** after code is written

**Key Features:**
- ✅ **Proactive**: Refines code immediately after modification without being asked
- ✅ **Preserves functionality**: Never changes what code does, only how
- ✅ **Project standards**: Follows CLAUDE.md conventions
- ✅ **Focuses on recent changes**: Only refines recently modified code
- ✅ **Writes code**: Actually applies the simplifications

**Prompt Focus:**
- ES modules with proper import sorting
- Explicit return type annotations
- Avoid nested ternary operators (prefer switch/if-else)
- Clarity over brevity ("explicit > compact")
- Remove unnecessary comments
- Maintain balance (avoid over-simplification)

### oh-my-opencode-slim: `code-simplicity-reviewer`

**Purpose:** Advisory, review-only analysis
**Model:** Opus 4.5
**Behavior:** Must be **explicitly called** by orchestrator

**Key Features:**
- 📊 **Review-only**: Provides analysis but doesn't modify code
- 📊 **YAGNI enforcement**: Ruthlessly identifies unnecessary code
- 📊 **Reports findings**: Creates markdown analysis reports
- 📊 **Estimates impact**: Calculates LOC reduction potential

**Prompt Focus:**
- Question necessity of every line
- Challenge abstractions and generalizations
- Remove YAGNI violations
- Simplify complex logic
- Eliminate redundancy
- Estimate LOC reduction

## Port Strategy: Best of Both Worlds

Create a **hybrid approach** in oh-my-opencode-slim-compound:

### 1. Keep Existing `code-simplicity-reviewer` (Read-Only)

Keep as-is for review workflow. This is the "prevention strategist" that identifies issues.

### 2. Add New `code-simplifier-executor` (Read-Write)

Port Claude Code's proactive simplifier with enhancements:

```typescript
// src/agents/simplifier-executor.ts
export function createSimplifierExecutorAgent(model: string): AgentDefinition {
  return {
    name: "code-simplifier-executor",
    description: "Autonomously simplify code after writing, preserving functionality",
    config: {
      model,
      temperature: 0.1,
      system: CODE_SIMPLIFIER_EXECUTOR_PROMPT,
    },
  };
}

const CODE_SIMPLIFIER_EXECUTOR_PROMPT = `You are an expert code simplification specialist that autonomously refines code for clarity, consistency, and maintainability while preserving exact functionality.

## Core Principles

1. **Preserve Functionality**: Never change what code does - only how it does it
2. **Apply Project Standards**: Follow CLAUDE.md conventions
3. **Enhance Clarity**: Simplify structure, reduce nesting, improve naming
4. **Maintain Balance**: Avoid over-simplification
5. **Focus on Recent Changes**: Only refine recently modified code

## Project Standards (from CLAUDE.md)

- Use ES modules with proper import sorting and extensions
- Prefer \`function\` keyword over arrow functions
- Use explicit return type annotations for top-level functions
- Follow React component patterns with explicit Props types
- Use proper error handling patterns
- Maintain consistent naming conventions

## Refinement Rules

### Clarity Enhancements
- Reduce unnecessary complexity and nesting
- Eliminate redundant code and abstractions
- Improve readability through clear names
- Consolidate related logic
- Remove unnecessary comments that describe obvious code
- **CRITICAL**: Avoid nested ternary operators - use switch statements or if/else chains
- Choose clarity over brevity - explicit code > compact code

### Balance Considerations
Avoid:
- Over-simplification that reduces clarity
- Overly clever solutions
- Combining too many concerns
- Removing helpful abstractions
- Prioritizing "fewer lines" over readability
- Making code harder to debug or extend

## Autonomous Operation

You operate **proactively and autonomously**:
- Refine code immediately after it's written or modified
- No explicit request needed
- Focus only on recently changed files
- Document only significant changes

## Process

1. Identify recently modified code sections
2. Analyze for improvement opportunities
3. Apply project standards
4. Ensure functionality unchanged
5. Verify simplification improves maintainability
6. Apply refinements automatically

## Output

Apply the simplifications directly to the code. Only provide explanatory comments for non-obvious changes that affect understanding.

Remember: Your goal is ensuring all code meets the highest standards of elegance and maintainability while preserving complete functionality.`;
}
```

### 3. Enhanced Orchestrator Integration

Update orchestrator to use both:

```typescript
const ORCHESTRATOR_PROMPT_TEMPLATE = `
<Workflow>
1. Understand the request
2. Plan if multi-step
3. **Write code** (you or delegated agents)
4. **Auto-simplify**: code-simplifier-executor runs automatically on new code
5. Verify with lsp_diagnostics
6. **Review**: code-simplicity-reviewer for deeper analysis (optional)
7. Mark TODOs complete
</Workflow>

<Agents>
| @code-simplifier-executor | Autonomously refines code after writing (runs automatically) |
| @code-simplicity-reviewer | Deep YAGNI analysis and LOC reduction estimates (call explicitly for review) |
</Agents>

<AutoSimplification>
After writing or modifying code:
- code-simplifier-executor automatically refines it
- Focuses on recent changes only
- Preserves functionality completely
- Applies project standards from CLAUDE.md
- No need to explicitly call this agent

For deeper review:
- Explicitly call @code-simplicity-reviewer
- Get LOC reduction estimates
- Identify YAGNI violations
- Plan larger refactoring efforts
</AutoSimplification>
`;
```

## Implementation Plan

### Phase 1: Port Claude Code's Simplifier

1. Create `src/agents/simplifier-executor.ts`
2. Port prompt from Claude Code's `code-simplifier.md`
3. Add to agent index
4. Configure in orchestrator as auto-running

### Phase 2: Enhance with Compound Engineering

Add compound engineering features:

```typescript
const CODE_SIMPLIFIER_EXECUTOR_PROMPT = `
[... existing prompt ...]

## Compound Engineering Integration

After simplifying code:
1. Document any patterns discovered in ~/.config/opencode/compound-knowledge/patterns/
2. If simplification reveals anti-patterns, document in anti-patterns/
3. Cross-reference similar simplifications in knowledge base

Example pattern documentation:
- Pattern: "Replace nested ternaries with switch"
- Anti-pattern: "Deeply nested conditionals"
- Learning: "ES6 destructuring reduces lines"
`;
```

### Phase 3: Add Configuration

Allow users to control behavior:

```json
// ~/.config/opencode/oh-my-opencode-slim.json
{
  "agents": {
    "code-simplifier-executor": {
      "model": "vertex-claude/claude-opus-4-5@20251101",
      "auto_run": true,
      "focus": "recent",
      "compound_learnings": true
    },
    "code-simplicity-reviewer": {
      "model": "vertex-claude/claude-opus-4-5@20251101"
    }
  }
}
```

## Usage Comparison

### Before (oh-my-opencode-slim)

```
User: "Add user authentication"
Orchestrator: [writes code]
User: "Review the code"
Orchestrator: background_task(agent="code-simplicity-reviewer")
→ Receives markdown report with recommendations
→ User must manually apply simplifications
```

### After (with code-simplifier-executor)

```
User: "Add user authentication"
Orchestrator: [writes code]
→ code-simplifier-executor automatically refines it
→ Code is immediately simplified
→ User sees final, refined version

Optionally:
User: "Do a deeper review"
Orchestrator: background_task(agent="code-simplicity-reviewer")
→ Receives LOC reduction estimates, YAGNI violations
```

## Key Differences

| Feature | code-simplicity-reviewer | code-simplifier-executor |
|---------|-------------------------|-------------------------|
| **Behavior** | Must be called explicitly | Runs automatically |
| **Output** | Analysis report (markdown) | Refined code (actual changes) |
| **Focus** | YAGNI violations, LOC reduction | Immediate code quality |
| **Use Case** | Deep review, large refactors | Real-time refinement |
| **Model** | Opus 4.5 | Opus 4.5 |
| **When** | After feature complete | After each code write |

## Recommendation

**Use both:**
1. **code-simplifier-executor**: Auto-runs after every code change for immediate quality
2. **code-simplicity-reviewer**: Explicitly called for deep analysis and planning refactors

This gives us:
- ✅ Immediate code quality (executor)
- ✅ Strategic refactoring (reviewer)
- ✅ YAGNI enforcement (reviewer)
- ✅ Pattern documentation (both + compound engineering)

## Next Steps

1. Create `simplifier-executor.ts` in the fork
2. Port Claude Code's prompt with enhancements
3. Add auto-run logic to orchestrator
4. Test with real code examples
5. Document in fork README
