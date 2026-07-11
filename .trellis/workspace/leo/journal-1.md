# Journal - leo (Part 1)

> AI development session journal
> Started: 2026-04-25

---

## Session 1: Migrate to Bun single executable with external adapters

**Date**: 2026-04-25

把 CLI 从 Node.js 迁移为 Bun 编译的单一可执行文件。shebang 改为 bun，适配器路径改为 `~/.tap/adapters/`（支持 `TAP_ADAPTERS_DIR` 覆盖），添加 build 脚本，新增 linuxdo/news.js 适配器。

- `src/cli.js`, `src/adapters.js`, `src/help.js` 等核心模块拆分
- README + npm packaging scripts


## Session 2: Add intercept and transform pipeline steps

**Date**: 2026-04-25

新增 `intercept`（fetch/XHR 拦截）、`select`（dot-path 深层取值）、`sort`（字段排序）pipeline steps；`map`/`filter` 改进；CDPSession 扩展；env var 重命名 `OPENCLI_CDP_ENDPOINT` → `TAP_CDP_ENDPOINT`。

- `src/cdp.js`, `src/executor.js`


## Session 3: CLI help & command discovery

**Date**: 2026-04-28

CLI 逻辑拆分为 src 模块（cli.js, adapters.js, help.js），实现 global/site/command 三级 help，添加 README 和 npm packaging。


## Session 4: Close CLI help discovery task

**Date**: 2026-04-28

归档 `04-27-cli-help-discovery` task，验证 global/site/command help、error exits、build 均正常。


## Session 5: Default TAP output to JSON

**Date**: 2026-04-29

**关键决策**：默认输出改为 JSON（agent-friendly），table 改为 opt-in。更新 README 中英文、help text。

- `src/cli.js`, `src/help.js`


## Session 6: Extend select path syntax

**Date**: 2026-04-29

扩展 select 路径语法：bracket indexes、quoted keys、wildcard projection、nested wildcard flattening、deterministic null for missing paths。


## Session 7: Prioritize user adapters

**Date**: 2026-04-29

**关键决策**：适配器优先级改为 `TAP_ADAPTERS_DIR` > `~/.tap/adapters/` > 内置。更新 README 和 spec。

- `src/adapters.js`


## Session 8: Explicit skill installation

**Date**: 2026-04-30

新增 `tap skill install <claude-code|codex>`，移除 npm postinstall 自动写入 `~/.claude/skills/`。

- `src/skills.js`, `src/cli.js`


## Session 9: Add setup browser doctor commands

**Date**: 2026-04-30

新增 `tap setup`（创建 `~/.tap`）、`tap browser start/status/stop`（agent Chrome 生命周期）、`tap doctor`（诊断）。移除 npm postinstall 隐式 `~/.tap` 写入。

- `src/setup.js`, `src/browser.js`, `src/doctor.js`


## Session 10: Unify bundled skill asset layout

**Date**: 2026-04-30

将 bundled tap-adapter-author skill 迁移到 TAP-owned `skills/` 布局，更新 skill resolution 使用 `TAP_PACKAGE_ROOT`/package roots，验证 source/standalone/npm wrapper 三种安装路径。


## Session 11: Schema-aware JSON output

**Date**: 2026-05-01

实现 schema-aware JSON envelopes，要求 adapter 声明 `output.fields`，移除旧内置 adapters，更新 tap-adapter-author schema confirmation workflow。

- `src/output.js`, `src/schema.js`


## Session 12: JSON-only output and argument validation

**Date**: 2026-05-02

**关键决策**：移除 public table 输出，JSON 成为唯一输出格式。新增 adapter 参数验证。

- `src/cli.js`, `src/output.js`


## Session 13: Use IPv4 loopback for default CDP endpoint

**Date**: 2026-05-02

`DEFAULT_CDP_ENDPOINT` 从 `localhost:9222` 改为 `127.0.0.1:9222`。同步 README/CLAUDE.md/spec。

- `src/config.js`


## Session 14: Add multi-request adapter pipeline primitives

**Date**: 2026-05-02

新增 named state (`as`/`from`)、`foreach` fan-out（bounded concurrency）、`mapOne`、`browserFetch`（cookie-backed）。

- `src/executor.js`


## Session 15: Bundle skill assets and quiet Chrome sessions

**Date**: 2026-05-02

将 skill 文件嵌入 standalone binary；agent Chrome 默认最小化启动，新增 `--foreground`，创建 Chrome profile 目录后 spawn，使用 background CDP targets。


## Session 16: Agent-friendly CLI phase 1

**Date**: 2026-05-02

**关键决策**：结构化 JSON 错误模型 + exit codes 2-6。`--format` 全局解析在 dispatch 前完成。Pipeline 错误用 regex 分类（browser → exit 4, else → exit 5）。

- `src/cli.js`, `src/doctor.js`, `bin/cli.js`


## Session 17: Agent CLI Phase 2: schema introspection & arg validation

**Date**: 2026-05-03

新增 `tap schema`（global/per-command/management）、adapter 参数验证（unknown flags、type/enum/min-max、boolean coercion）、help text 类型标注。

- `src/schema.js`（新模块）


## Session 18: Add adapter install/list/remove command namespace

**Date**: 2026-05-04

新增 `tap adapter install/list/remove`，支持 github:/url:/git: 来源。Conflict detection、GitHub API redirect handling、`execFileSync` 防注入、orphaned file cleanup。

- `src/adapter-manager.js`（新模块）


## Session 19: Complete split npm binary packages

**Date**: 2026-05-06

完成 split npm binary package 发布流：platform optional packages、小主包、dry-run 验证、npm token 处理。


## Session 20: Agent-Friendly CLI: --fields mask & help examples

**Date**: 2026-05-06

新增 `--fields` 字段掩码（逗号分隔，未知字段进 `meta.warnings`）和 `adapter.examples`（help 渲染）。契约诊断始终对完整 schema 运行，不受掩码影响。

- `src/cli.js`, `src/output.js`, `src/help.js`


## Session 21: Create consolidate spec skill

**Date**: 2026-05-09

创建 consolidate-spec skill，整合 Trellis spec 文档，加入 checklist routing 和聚焦的 core/adapter contracts。


## Session 22: Update tap-adapter-author skill: Pattern F + recon rules

**Date**: 2026-05-10

新增 Pattern F（login-state HTML partial/infinite-scroll）、403-response decision gate、sensitive-param gate、扩展 recon checklist、三层 limit verification、data-access priority ladder。Version 0.1.2。


## Session 23: Add tap browser restart command

**Date**: 2026-05-10

新增 `tap browser restart`（stop + relaunch agent Chrome），更新 schema/docs/spec。


## Session 24: Bump npm version for browser restart release

**Date**: 2026-05-10

npm 版本 bump 到 0.1.3，新增 spec rule：code logic 或 bundled resource 变更需版本 bump。


## Session 25: Update tap adapter author skill dependencies

**Date**: 2026-05-10

tap-adapter-author skill 新增 runtime dependencies 声明（tap CLI、chrome-devtools、browser session、network）。npm 0.1.4。


## Session 26: Tap adapter author schema gate

**Date**: 2026-05-11

新增 mandatory schema confirmation gate，npm publish handoff expectations，0.1.5 bump，README 围绕 deterministic Agent data access 定位。


## Session 27: Restore English default README

**Date**: 2026-05-12

README.md 恢复为英文默认，README.zh.md 为中文版本。


## Session 28-30: TAP roadmap 定位精简

**Date**: 2026-05-16

TAP 长远规划文档三步迭代：(1) 更新定位反映 ownership 现实和 orchestration-layer 方向；(2) 精简重复定位语言，明确 production read-only / non-production acceptance 边界；(3) 最终从 297 行压缩至 95 行，核心叙事前置，战略判断以结论形式保留。
