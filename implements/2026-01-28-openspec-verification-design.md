# OpenSpec Verification - PRD Consistency Check

## Overview

在 `verification-before-completion` 和 `finishing-a-development-branch` 之间新增 PRD 一致性校验步骤，确保实现与原始需求对齐。

## Directory Structure

```
skills/openspec-verification/
├── SKILL.md              # 主 skill 文件
└── verifier-prompt.md    # Subagent prompt 模板

commands/
└── verify-openspec.md    # Slash command 入口

diagram/
└── v3.md                 # 更新后的完整流程图
```

## Workflow

```
verification-before-completion (通过)
    ↓
[openspec-verification]
    ├── 定位 OpenSpec change 目录
    ├── 收集文档 (sources/proposal/tasks/design)
    ├── 获取 git diff (BASE..HEAD)
    └── 派发 verifier subagent
            ↓
    subagent 执行三项检查
        ├── 文档一致性检查
        ├── PRD 功能完整性检查
        └── PRD 功能一致性检查
            ↓
    返回 PASS/FAIL + 报告
        ↓
[PASS] → finishing-a-development-branch
[FAIL] → 返回实现阶段修复问题
```

## Trigger Methods

| 方式 | 命令 | 说明 |
|------|------|------|
| 流程自动触发 | - | verification-before-completion 通过后 |
| 手动触发 | `/verify-openspec` | 任何时候检查 PRD 一致性 |

## Verification Checks

### 1. Document Consistency Check

对比 OpenSpec 文档对，检查是否存在冲突：
- sources.md ↔ proposal.md
- proposal.md ↔ tasks.md
- tasks.md ↔ design.md (如存在)

**检查内容：**
- 需求描述矛盾
- 范围不匹配
- 下游文档遗漏项
- 术语不一致

### 2. PRD Completeness Check

从 sources.md/proposal.md 提取所有需求，逐一验证代码实现是否存在。

**输出格式：**
```markdown
| Requirement | Status | Code Location |
|-------------|--------|---------------|
| Feature A | ✅ Implemented | src/a.ts:10-25 |
| Feature B | ❌ Missing | - |
```

### 3. PRD Consistency Check

对每个已实现的功能，验证行为是否与 PRD 描述一致。

**输出格式：**
```markdown
| Requirement | PRD Description | Actual Implementation | Status |
|-------------|-----------------|----------------------|--------|
| Feature A | "User can..." | "Code does..." | ✅/⚠️/❌ |
```

## Pass/Fail Criteria

**PASS 条件（全部满足）：**
- 文档一致性：无矛盾
- PRD 完整性：100% 需求已实现
- PRD 一致性：所有实现符合描述

**FAIL 条件（任一）：**
- 文档存在矛盾
- 任何需求未实现或部分实现
- 任何实现与 PRD 描述显著不符

## Output Format

```markdown
## Document Consistency
| Document Pair | Status | Issues |
|---------------|--------|--------|
| sources.md ↔ proposal.md | ✅/❌ | ... |

## PRD Completeness
| Requirement | Status | Code Location |
|-------------|--------|---------------|

## PRD Consistency
| Requirement | PRD Description | Actual Implementation | Status |
|-------------|-----------------|----------------------|--------|

## Verdict
**Status:** PASS / FAIL
**Summary:** [1-2 sentences]
**Blocking Issues:** (if FAIL)
```

## Key Design Decisions

| 决策点 | 选择 | 原因 |
|--------|------|------|
| 执行方式 | Subagent 审查 | 灵活处理复杂场景，能理解语义 |
| 位置 | 独立步骤 | verification 和 finishing 之间，职责清晰 |
| 输出语言 | 英文 | 统一标准，便于国际化 |
| 失败处理 | 返回实现阶段 | 阻塞完成流程，强制修复 |
| 无 OpenSpec 时 | 跳过 | 向后兼容，不强制 OpenSpec |

## Files Created

| 文件 | 用途 |
|------|------|
| `skills/openspec-verification/SKILL.md` | 定义 skill 流程和使用方式 |
| `skills/openspec-verification/verifier-prompt.md` | Subagent 的完整指令 |
| `commands/verify-openspec.md` | Slash command 入口 |
| `diagram/v3.md` | V3 流程图，包含新步骤 |
| `docs/plans/2025-01-28-openspec-verification-design.md` | 设计文档 |

## V3 Workflow Changes

| 步骤 | 变化 |
|------|------|
| openspec-verification | 新增：PRD 一致性校验步骤 |
| 校验内容 | 文档一致性、功能完整性、实现一致性 |
| 触发方式 | 流程自动触发 或 `/verify-openspec` 手动触发 |
| 失败处理 | 返回实现阶段修复问题 |

## Integration Points

**调用者：**
- 工作流：verification-before-completion 通过后
- 用户：`/verify-openspec` 命令

**后续步骤：**
- PASS → `finishing-a-development-branch`
- FAIL → 返回实现阶段
