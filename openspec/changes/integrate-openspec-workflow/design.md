# Design: Integrate Superpowers Skills with OpenSpec Workflow

## Architecture Decision: Unified Workflow Integration

### Context

Superpowers provides three core skills for software development:
- **brainstorming**: Transform ideas into designs
- **writing-plans**: Create detailed implementation plans
- **executing-plans**: Execute plans and track progress

OpenSpec provides a spec-driven development workflow:
- **proposal.md**: Document purpose and scope of changes
- **tasks.md**: High-level task checklist
- **design.md**: Technical decisions
- **specs/**: Specification changes

### Decision

**Deeply integrate the three skills with OpenSpec change workflow, forming a unified pipeline from ideation to implementation.**

### Directory Structure

```
openspec/changes/<feature-name>/
├── proposal.md      # Why (OpenSpec standard)
├── design.md        # Technical decisions (optional)
├── tasks.md         # High-level task overview
├── plan/
│   ├── YYYY-MM-DD-v1.md    # Initial plan
│   ├── YYYY-MM-DD-v2.md    # Revised plan
│   └── ...                  # Multiple versions supported
└── specs/
    └── <spec-name>/
        └── spec.md
```

### Workflow

```
[brainstorming] → OpenSpec change structure
                      ↓
[writing-plans] → plan/YYYY-MM-DD-v<N>.md
                      ↓
[executing-plans] → Execute + sync tasks.md
                      ↓
                openspec archive
```

### Key Design Decisions

| Decision Point | Choice | Rationale |
|----------------|--------|-----------|
| Plan storage location | OpenSpec only | Centralized management, remove docs/plans/ |
| tasks.md vs plan.md relationship | Layered | tasks.md for overview, plan.md for detailed implementation |
| Plan file naming | YYYY-MM-DD-v<N>.md | Support multiple version iterations |
| Commit strategy | Manual control | No forced commits, developer decides |

### Rationale

1. **Consistency**: All project documents centralized in OpenSpec structure
2. **Traceability**: Complete lifecycle visible from proposal to implementation
3. **Flexibility**: Multiple plan versions support iterative changes
4. **Standardization**: Follow OpenSpec archive workflow

### Trade-offs

**Advantages:**
- Unified workflow, reduced confusion
- Complete context passing between skills
- Compatible with OpenSpec ecosystem

**Disadvantages:**
- Requires modifying three existing skills
- Dependency on OpenSpec
- Slightly increased learning curve
