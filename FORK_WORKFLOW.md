# Fork Workflow for oh-my-opencode-slim-compound

This repository is a fork of [alvinunreal/oh-my-opencode-slim](https://github.com/alvinunreal/oh-my-opencode-slim) with compound engineering enhancements.

## Repository Structure

- **Origin**: `isc-tdyar/oh-my-opencode-slim-compound` (our fork)
- **Upstream**: `alvinunreal/oh-my-opencode-slim` (original)

## Syncing with Upstream

Periodically check for updates from the upstream slim repository:

### Check for Updates

```bash
# Fetch upstream changes
git fetch upstream

# See what's new
git log HEAD..upstream/master --oneline

# Or view detailed diff
git diff HEAD..upstream/master
```

### Merge Upstream Changes

```bash
# Make sure you're on master
git checkout master

# Merge upstream changes
git merge upstream/master

# Or rebase for cleaner history (use cautiously)
git rebase upstream/master

# Push to our fork
git push origin master
```

### Resolve Conflicts

If conflicts occur (likely in files we've modified):

```bash
# After merge conflict
git status  # See conflicted files

# Edit files to resolve conflicts, then:
git add <resolved-files>
git commit

# Push resolved merge
git push origin master
```

## Our Enhancements

This fork adds compound engineering features:

### New Agents

1. **workflow-planner** - Detailed planning with history analysis
2. **workflow-reviewer** - Multi-agent parallel code review
3. **workflow-compounder** - Auto-document solutions
4. **pattern-analyst** - Identify patterns/anti-patterns
5. **history-miner** - Learn from git history
6. **prevention-agent** - Generate future-proofing strategies

### New Workflows

- `/workflows:plan` - Plan before coding
- `/workflows:review` - Multi-agent review
- `/workflows:compound` - Document learnings

### Knowledge Base Integration

Integrates with `~/.config/opencode/compound-knowledge/` for persistent learning.

## Development Workflow

### Branch Strategy

```bash
# For upstream syncs
master (tracks upstream/master + our changes)

# For new features
feature/compound-workflow-planner
feature/knowledge-base-integration

# For experimental work
experiment/pattern-recognition
```

### Adding New Features

```bash
# Create feature branch
git checkout -b feature/my-enhancement

# Make changes
# Commit frequently

# Before merging, sync with master
git checkout master
git fetch upstream
git merge upstream/master
git checkout feature/my-enhancement
git rebase master

# Merge to master
git checkout master
git merge feature/my-enhancement
git push origin master
```

## Upstream Contribution Strategy

If we develop features that would benefit the original slim project:

```bash
# Create clean branch from upstream
git checkout -b upstream-contribution upstream/master

# Cherry-pick relevant commits
git cherry-pick <commit-hash>

# Push to our fork
git push origin upstream-contribution

# Create PR to upstream via GitHub
gh pr create --repo alvinunreal/oh-my-opencode-slim
```

## Periodic Sync Schedule

Recommended: Check for upstream updates **weekly**

```bash
# Add to cron or create reminder
0 9 * * MON cd ~/ws/oh-my-opencode-slim-compound && git fetch upstream && git log HEAD..upstream/master --oneline
```

## Files Likely to Conflict

When merging upstream, these files may conflict:

- `src/agents/orchestrator.ts` - We've enhanced with compound engineering
- `src/agents/index.ts` - We've added new agents
- `README.md` - Different documentation
- `package.json` - May have different dependencies

**Strategy**: Keep our versions of these files, but review upstream changes for bug fixes.

## Release Tagging

Tag releases to track our enhancement versions:

```bash
# Tag format: v{slim-version}-compound.{our-version}
# Example: if slim is at v1.2.3 and this is our 4th release
git tag v1.2.3-compound.4
git push origin v1.2.3-compound.4
```

## Quick Reference

```bash
# Fetch and view upstream changes
git fetch upstream && git log HEAD..upstream/master --oneline

# Merge upstream (safe)
git merge upstream/master

# Rebase upstream (cleaner but riskier)
git rebase upstream/master

# Abort merge
git merge --abort

# Abort rebase
git rebase --abort

# View remotes
git remote -v

# Check which branch you're on
git branch --show-current

# See divergence from upstream
git rev-list --left-right --count HEAD...upstream/master
```

## Questions?

- Original slim: https://github.com/alvinunreal/oh-my-opencode-slim
- Our fork: https://github.com/isc-tdyar/oh-my-opencode-slim-compound
- Issues: Use GitHub issues on our fork
