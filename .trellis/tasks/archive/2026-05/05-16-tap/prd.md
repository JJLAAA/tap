# 精简并重写 TAP 长远规划文档

## Goal

把 `docs/readonly-data-access-roadmap.md` 重写为一篇更聚焦的规划文档。
核心叙事锁定为："让 Agent 能用一个稳定的业务语义入口，跨工具执行、判断、收集证据。"
当前文档战略判断正确，但结构松散、信息量冗余，需要在保留关键判断的前提下大幅精简。

## 已知事实（来自原文 + 对话）

- 原文约 297 行，包含：定位、核心判断、组织现实、wrapper→orchestration、只读约束、目标场景、命令设计原则、source provider 扩展方向（四阶段）、只读安全模型、技术可行性、阶段路线、非目标
- 核心叙事：Agent 调用业务工具的 orchestration layer；护城河是 domain knowledge（查哪些工具、按什么顺序、如何判断、如何输出）
- 当前文档已说清楚的：定位边界、组织现实、wrapper→orchestration 转变、只读约束、非目标
- 当前文档未说清楚的（对话中补充的）：
  1. governance 的落地路径（谁维护 allowlist、audit log 到哪）
  2. adapter lifecycle 标注（临时 / 稳定 / 待迁移 / 已迁移）
  3. workflow adapter 的核心语义（check / summarize 实现形态）
  4. "先跑真实 workflow，再提炼抽象"的优先级建议

## Open Questions

（已全部解决）

## Requirements

- 章节重组、只留骨干：删掉推导过程，读者直接接受结论
- 核心叙事"让 Agent 能用一个稳定的业务语义入口，跨工具执行、判断、收集证据"在文档开头显式点明
- 保留的关键判断：定位、组织现实、wrapper→orchestration、只读约束、阶段路线、非目标
- 删掉的内容：各类"为什么"的推导段落、重复铺垫、四阶段 source 扩展的详细描述（保留结论）
- 不加入其他新内容（governance、lifecycle、workflow adapter 语义属于实践层，超出"厘清方向"范围）

## Decision

- 精简方式：章节重组 + 只保留骨干（选项 2）
- 新增内容：仅显式补充核心叙事一句话，其余不加

## Acceptance Criteria

- [ ] 文档字数显著少于原文（目标 ≤ 原文 60%）
- [ ] 核心叙事在文档开头即可感知
- [ ] 不丢失原文中任何关键战略判断

## Out of Scope

- 修改 CLAUDE.md 或其他文档
- 对代码做任何修改

## Technical Notes

- 原文路径：`docs/readonly-data-access-roadmap.md`
- 原文约 297 行，中文，结构完整但较冗长
