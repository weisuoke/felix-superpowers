# Project Context

## Purpose
Superpowers is a complete software development workflow for AI coding agents, built on composable "skills" that enforce best practices. The system ensures agents don't just jump into writing code, but instead follow structured processes: brainstorming designs, creating implementation plans, using TDD, performing code reviews, and finishing work properly.

**Key goals:**
- Enforce disciplined development practices (TDD, systematic debugging, verification)
- Enable autonomous multi-hour agent work sessions without deviation from plans
- Provide reusable skill patterns that trigger automatically based on context
- Support multiple AI coding platforms (Claude Code, Codex, OpenCode)

## Tech Stack
- **JavaScript (ES Modules)** - Core library code (`lib/skills-core.js`)
- **Markdown with YAML frontmatter** - Skill definitions (`skills/*/SKILL.md`)
- **Bash** - Hooks, test runners, and automation scripts
- **Python** - Analysis tools (token usage analysis)
- **JSON** - Configuration files (plugin.json, hooks.json)

## Project Conventions

### Code Style
- **JavaScript**: ES modules (`import`/`export`), JSDoc comments for documentation
- **Shell scripts**: Strict mode (`set -euo pipefail`), descriptive variable names
- **Skill files**: YAML frontmatter with `name` and `description` fields, followed by Markdown content
- **Naming**: kebab-case for skill directories and file names

### Architecture Patterns
- **Skills as Markdown**: Each skill is a directory containing `SKILL.md` with YAML frontmatter for metadata
- **Skill resolution**: Personal skills shadow/override superpowers skills (personal > superpowers namespace)
- **Hooks**: Session lifecycle hooks defined in `hooks/hooks.json`, executed via shell scripts
- **Plugin system**: Claude Code plugin manifest in `.claude-plugin/plugin.json`
- **Cross-platform support**: Platform-specific configs in `.codex/`, `.opencode/`, `.claude-plugin/`

### Testing Strategy
- **Shell-based testing**: Tests run Claude Code CLI in headless mode (`claude -p`)
- **Helper functions**: Common assertions in `test-helpers.sh` (`assert_contains`, `assert_order`, etc.)
- **Fast tests**: Verify skill content and requirements (~2 minutes)
- **Integration tests**: Full workflow execution with real projects (~10-30 minutes, use `--integration` flag)
- **Test location**: `tests/claude-code/` for Claude Code-specific tests

### Git Workflow
- **Main branch**: `main` is the default and target for PRs
- **Feature isolation**: Use git worktrees for parallel development (via `using-git-worktrees` skill)
- **Commit conventions**: Descriptive commit messages with Co-Authored-By for AI contributions
- **PR workflow**: Complete implementation, verify tests pass, then create PR or merge

## Domain Context
- **Skills**: Reusable workflow patterns that AI agents follow automatically when relevant
- **Skill triggering**: Skills activate based on context (e.g., `brainstorming` before creative work, `systematic-debugging` for bugs)
- **Two-stage review**: Spec compliance review first, then code quality review
- **Subagent-driven development**: Spawning fresh agents per task with review checkpoints
- **YAGNI principle**: "You Aren't Gonna Need It" - ruthlessly remove unnecessary features

## Important Constraints
- Skills must be platform-agnostic where possible (support Claude Code, Codex, OpenCode)
- Skills should be composable and trigger automatically without user intervention
- Test-driven development is mandatory, not optional
- Evidence-based verification required before claiming success

## External Dependencies
- **Claude Code CLI**: Primary platform for testing and development
- **Git**: Version control and worktree management
- **GitHub**: Repository hosting, issues, and plugin marketplace (`obra/superpowers-marketplace`)
