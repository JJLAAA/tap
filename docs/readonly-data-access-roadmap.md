# TAP 长期规划：面向 Agent 的业务工具编排层

## 核心叙事

TAP 的存在是为了让 Agent 能用一个稳定的业务语义入口，跨工具执行、判断、收集证据。

Agent 调用现实业务工具时，不应该临时理解网页路径、内部接口、登录态、字段结构、筛选逻辑，以及多个工具之间的调用顺序和结果拼接方式。TAP 把这些复杂性封装为：

```bash
tap <domain> <command> [--key value] [--format json]
```

## 定位

TAP 是面向 Agent 的业务工具 **orchestration layer**，不是：

- 正式业务 API 的替代品
- 企业核心数据的统一入口或治理平台
- 通用自动化脚本框架

TAP 站在已有工具之上做跨工具编排、结果归一化、断言、证据收集和 Agent-friendly 输出。当官方工具缺位时，TAP 临时桥接长尾、非正式、尚未 Agent-friendly 的数据入口。

**护城河不是"能访问某个数据源"，而是"知道一个跨系统业务问题应该查哪些工具、按什么顺序查、如何判断、如何输出给 Agent"。**

## 组织现实

真实组织中，核心数据入口通常由数据持有方自行建设（ownership 清晰、KPI 驱动、权限治理）。TAP 不与这些官方工具竞争，而在它们不存在、尚未 Agent-friendly、或数据源属于长尾/第三方场景时补位。

一个数据入口的自然演进路径：

```
一次性人工查询
  → TAP 临时 adapter
  → TAP 验证稳定工作流
  → 数据持有方专有 CLI/API/MCP
  → TAP 保留桥接或边缘补位角色
```

当某个 adapter 证明高频价值后，应推动数据持有方把能力正式产品化；TAP 的成功标准不是"所有业务数据都接入 TAP"。

## 核心方向：从 wrapper 到 orchestration

TAP 的长期主线是 orchestration，而不是包装单个核心系统：

```
系统 owner 的官方 CLI/API/MCP
日志 / 指标 / BI / 浏览器只读入口 / 测试 runner / 长尾 adapter
        ↓
TAP orchestration：组合、归一化、断言、报告、artifacts
        ↓
Agent / CI / 人类开发者
```

workflow 命令设计原则：优先设计业务语义命令，而不是围绕单个数据源堆积查询命令：

```bash
tap diagnose order --id O123
tap acceptance order-smoke --env staging
tap release verify --service payment --version 1.2.3
tap incident collect --trace-id abc123
```

## 只读约束

TAP 在生产环境只服务查询、诊断、汇报、巡检和证据收集，不承载写入动作。禁止：

- 创建、修改、删除业务对象
- 提交表单或触发状态变更
- 执行写接口（POST / PUT / PATCH / DELETE）或写 SQL
- 通过浏览器点击触发不可逆动作

只读边界由三层兜底：adapter 静态校验（白名单 step、HTTP 默认 GET、SQL 限 SELECT）、provider 运行时约束（超时、行数限制、脱敏）、基础设施权限（只读账号、只查询 token、审计日志）。

测试/预发环境可开启受控 acceptance mode，但必须显式启用，并具备幂等键、清理策略和环境隔离。

## 目标场景

- **非生产端到端业务验收**：准备数据、触发流程、等待异步状态、跨系统检查结果、收集 artifacts、输出 pass/fail
- **业务诊断**：围绕订单、广告计划、traceId 等对象，串联业务系统、日志、指标、配置和队列状态
- **发布/变更验证**：版本、流量、错误率、核心链路和回滚信号
- **事故证据收集**：按 traceId、业务 ID、时间窗口聚合日志、指标和接口返回
- **长尾数据补位**：workflow 环节缺少官方工具时，临时接入 HTTP/Web/第三方 SaaS
- **Agent 上下文补全**：把分散工具输出整理成稳定 JSON，供 Agent 继续分析和生成下一步动作

## 阶段路线

**近期**：保持 HTTP/Web 能力稳定；建立 workflow 命令粒度规范；为诊断、非生产验收、发布验证沉淀代表性 workflow 命令。

**中期**：引入 datasource 配置；实现 `sql`、`log`、`metric` 三类 provider（含超时、限制、脱敏、审计）；建立常见 workflow。

**长期**：支持受控 RPC / 内部 CLI allowlist provider；将已验证 workflow 抽象为正式 workflow adapter；与 Agent 工具生态集成；推动高频单点 adapter 迁移到数据持有方维护的专有工具。

## 非目标

TAP 不应成为：正式业务 API 替代品、通用低代码平台、任意脚本执行器、写操作自动化工具、外部强契约集成平台、企业核心数据统一治理平台、替代 Playwright/Cypress 的 UI E2E 测试框架、长期包装单个系统核心能力的平行入口。
