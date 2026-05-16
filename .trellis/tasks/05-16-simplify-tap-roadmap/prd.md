# 精简 TAP 长期规划文档

## Goal

按照文档审阅结论精简 `docs/readonly-data-access-roadmap.md`，让 TAP 的长期定位更清晰地收敛到 Agent 业务工具编排层，同时消除只读、验收、官方工具复用、workflow adapter 阶段安排等容易读成矛盾的表述。

## What I already know

* 当前长期规划文档是 `docs/readonly-data-access-roadmap.md`。
* README.zh 里的长期规划链接标题仍写作“只读业务数据接入层”，与 roadmap 当前标题“业务工具编排层”不一致。
* 用户已经确认按上一轮分析进行精简。

## Requirements

* 澄清“生产只读”和“非生产受控 acceptance mode”的边界。
* 澄清 TAP 的 E2E 验收不是 Playwright/Cypress 式 UI 测试框架替代品。
* 区分官方 Agent-friendly CLI/API/MCP 与任意 shell/内部 CLI 执行。
* 澄清近期/中期 workflow 命令沉淀与长期 workflow adapter DSL 的关系。
* 删除或压缩重复表达，尤其是“不做终局数据入口”“不长期包装核心系统”“优先复用官方工具”“保留 orchestration 与长尾补位”。
* 同步 README.zh 中长期规划链接标题。

## Acceptance Criteria

* [x] Roadmap 主线仍是“面向 Agent 的业务工具编排层”。
* [x] 文档不再暗示生产环境支持写动作。
* [x] 文档不再暗示 TAP 要替代 UI E2E 测试框架。
* [x] README.zh 链接标题与 roadmap 标题一致。
* [x] 不修改 CLI/runtime 行为。

## Out of Scope

* 不修改 JavaScript 代码。
* 不新增命令、flag、schema 或 runtime contract。
* 不更新 `.trellis/spec/`，本任务只整理产品定位文档。

## Technical Notes

* 受影响文件预计为 `docs/readonly-data-access-roadmap.md` 和 `README.zh.md`。
* 仓库文档语言当前中英文混合；本任务修改中文长期规划与中文 README 链接文案。
