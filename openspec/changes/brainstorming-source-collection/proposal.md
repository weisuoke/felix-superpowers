## Why

The current brainstorming skill starts directly with design exploration when invoked. In team workflows, requirements typically originate from multiple sources: Jira tickets for requirements, Apifox for API contracts, and Figma for UI designs. Without a structured way to collect and document these sources, context gets lost and there's no single source of truth for requirement traceability.

Adding a source collection phase when brainstorming is invoked without arguments enables:
- Automatic gathering of requirement context before design exploration
- Integration with MCP services (apifox-mcp, figmap-mcp) to pull relevant information
- Documentation of sources for later consistency verification

## What Changes

- **brainstorming skill**: Add a new "source collection" phase when invoked without arguments:
  1. Ask for Jira URL (optional, but requires manual description if skipped)
  2. Ask for API endpoint URL - Apifox (can skip)
  3. Ask for Figma URL (can skip)
  4. Call apifox-mcp and figmap-mcp to fetch summaries
  5. Create `sources.md` in the OpenSpec change directory
  6. Proceed to normal brainstorming flow with collected context

- **New file**: `openspec/changes/<feature-name>/sources.md` containing:
  - Source links (Jira, API, Figma)
  - PRD description (from Jira or manual input)
  - API summary (from apifox-mcp)
  - Design summary (from figmap-mcp)

## Impact

- Affected skills:
  - `skills/brainstorming/SKILL.md`

- New conventions:
  - `sources.md` added to OpenSpec change structure
  - MCP integration for automatic context gathering
  - Graceful degradation when MCP services unavailable
