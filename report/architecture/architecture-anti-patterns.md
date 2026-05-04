# Architecture Anti-Patterns

> 目标：识别常见架构误区。很多系统不是因为“不懂架构”失败，而是因为用了架构名词，却没有守住边界、依赖、I/O 和失败路径。

专题首页：[Big Tech Architecture Atlas](README.md)

## 1. Application Architecture Anti-Patterns

| Architecture | Anti-Pattern | 表现 | 修正方向 |
|---|---|---|---|
| Monolithic Architecture | Big Ball of Mud | 所有业务逻辑混在 Controller、Service、Util 中 | 先做模块化边界和分层 |
| Modular Monolith Architecture | Fake Modules | 目录看似分模块，实际互相直接调用内部类和表 | 定义模块公开 API，限制跨模块依赖 |
| Layered Architecture | Anemic Service Layer | Service 只转发 Repository，业务规则散落各处 | 把业务规则收敛到应用服务或领域层 |
| Clean Architecture | Framework Leakage | Use Case 依赖 Spring、ORM、HTTP Request | 让框架停留在 Adapter 层 |
| Hexagonal Architecture | Adapter Pollution | Core 直接调用 DB Client、HTTP Client | 用 Port 表达外部能力 |
| DDD Architecture | Directory-Driven Design | 只创建 domain、application、infrastructure 目录 | 先建立 Ubiquitous Language 和 Bounded Context |

## 2. Service Architecture Anti-Patterns

| Architecture | Anti-Pattern | 表现 | 修正方向 |
|---|---|---|---|
| SOA | ESB God Layer | 所有业务编排都塞进 ESB | 服务边界内聚，编排逻辑有明确归属 |
| Microservices | Distributed Monolith | 服务拆了，但数据库共享、发布绑定、调用强耦合 | 独立数据所有权、独立发布、契约治理 |
| API Gateway | Gateway as Business Layer | 网关里写大量业务逻辑 | 网关只做入口治理，业务留在服务或 BFF |
| BFF | One BFF for Everything | 所有端共用一个巨型 BFF | 按端或体验边界拆 BFF |
| Event-Driven | Event Soup | 事件名混乱、语义不清、Schema 随意变 | 建事件契约、版本、Owner 和兼容策略 |
| CQRS | CQRS Everywhere | 简单 CRUD 也强行读写分离 | 只在读写模型确实不同的地方使用 |
| Event Sourcing | No Event Versioning | 事件结构变化后无法重放历史 | 设计事件版本和迁移策略 |
| Saga | No Compensation Design | 只设计正向流程，不设计失败补偿 | 每一步都定义补偿、重试和人工干预 |
| Service Mesh | Mesh Before Basics | 没有服务治理基础就上 Mesh | 先有清晰服务边界、可观测性和发布治理 |

## 3. Cloud Native Anti-Patterns

| Architecture | Anti-Pattern | 表现 | 修正方向 |
|---|---|---|---|
| Cloud Native | YAML-Driven Complexity | 大量 YAML 但没有自动化和治理 | 建模板、GitOps、可观测性和标准路径 |
| Kubernetes | Kubernetes as a Silver Bullet | 把所有问题都归结为上 K8s | 先明确部署、扩缩容、隔离和恢复需求 |
| Serverless | Hidden Coupling | 函数到处触发，链路不可见 | 建事件地图、Trace 和失败重试策略 |
| Cell-Based | Cell Without Isolation | 只是逻辑分组，底层仍共享关键依赖 | Cell 内资源独立，故障爆炸半径可控 |
| Multi-Region | Backup Without Drill | 有灾备架构但从不演练 | 定期故障演练和切流演练 |
| Edge | Cache Everything | 不区分动态、权限、个性化内容 | 设计缓存策略、失效策略和权限边界 |

## 4. Data Architecture Anti-Patterns

| Architecture | Anti-Pattern | 表现 | 修正方向 |
|---|---|---|---|
| Data Warehouse | Metric Chaos | 同一个指标多个口径 | 建指标 Owner、语义层和数据字典 |
| Data Lake | Data Swamp | 文件都进湖，但没人知道能不能用 | 建 Catalog、质量规则、生命周期和权限 |
| Lakehouse | Storage Only | 只把数据放对象存储，没有表格式治理 | 引入表格式、事务、元数据和权限治理 |
| Streaming | Real-Time Theater | 名义实时，实际没人消费实时结果 | 明确实时业务动作和延迟目标 |
| Lambda | Double Logic Drift | 批处理和流处理逻辑逐渐不一致 | 抽象共享逻辑，或评估 Kappa |
| Kappa | Replay Without Capacity | 设计重放，但资源无法承受重放 | 规划保留期、重放窗口和资源隔离 |
| CDC | Sync Without Ownership | 变更同步没人负责 Schema 和下游影响 | 定义表 Owner、变更通知和兼容规则 |
| Data Mesh | Platform-Only Mesh | 买了平台，但领域团队不负责数据产品 | 明确 Data Product Owner 和治理责任 |

## 5. AI Architecture Anti-Patterns

| Architecture | Anti-Pattern | 表现 | 修正方向 |
|---|---|---|---|
| ML Platform | Notebook to Production | Notebook 直接变生产任务 | 建训练、注册、发布和监控流水线 |
| MLOps | No Data Version | 只有模型版本，没有数据版本 | 数据、代码、参数、模型一起版本化 |
| RAG | Vector DB Equals RAG | 只建向量库，不做切分、权限、评测 | 建完整 RAG 链路和评测集 |
| Agentic | Tool Chaos | Agent 可以调用任何工具，权限不清 | 工具白名单、权限边界、审计和回滚 |
| LLMOps | Prompt as Text | Prompt 随手改，没有版本和指标 | Prompt 版本化、Trace、Eval 和灰度 |
| Vector Database | Embedding Blindness | 不评估 Embedding 和召回质量 | 建 Top-K、Recall、MRR 等检索评测 |

## 6. Engineering, Security and Resilience Anti-Patterns

| Architecture | Anti-Pattern | 表现 | 修正方向 |
|---|---|---|---|
| DevOps | CI Without Feedback | 有流水线，但失败没人看 | 把质量门禁和反馈接入研发流程 |
| GitOps | Manual Hotfix Drift | 线上手改配置，Git 不知道 | 禁止手工漂移，所有变更回 Git |
| Platform Engineering | Platform as Ticket System | 平台只是工单入口 | 提供自助能力和 Golden Path |
| Observability | Logs Only | 只有日志，没有指标和 Trace | 建 Metrics、Logs、Traces 关联 |
| SRE | Alert Fatigue | 告警太多，没人响应 | 用 SLO 和 Error Budget 设计告警 |
| Zero Trust | VPN Rebrand | 只是换了 VPN 名字 | 按身份、设备、上下文持续授权 |
| IAM | Role Explosion | 角色越来越多，没人懂 | RBAC + ABAC，定期权限审计 |
| Audit | Debug Log as Audit | 普通日志当审计 | 审计日志不可篡改、可检索、有证据链 |
| Cache | Cache Inconsistency | 缓存和数据库长期不一致 | 明确失效、更新和降级策略 |
| Sharding | Bad Shard Key | 分片键导致热点和跨分片查询 | 根据访问模式选择分片键 |
| Circuit Breaker | Retry Storm | 下游慢时疯狂重试 | 设置超时、熔断、退避和隔离 |
| Rate Limiting | Limit All Equally | 核心请求和非核心请求同等限流 | 按优先级保护核心链路 |

