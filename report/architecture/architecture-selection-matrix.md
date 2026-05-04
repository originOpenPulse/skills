# Architecture Selection Matrix

> 目标：当你面对一个真实项目时，快速判断优先考虑哪些 Architecture。  
> 注意：这是决策辅助，不是绝对规则。架构选择永远要结合业务阶段、团队能力、风险和成本。

专题首页：[Big Tech Architecture Atlas](README.md)

## 1. Quick Decision Table

| Situation | Recommended Architecture | Avoid Too Early |
|---|---|---|
| 1-5 人团队，业务还在探索 | Modular Monolith, Layered Architecture | Microservices, Service Mesh, Multi-Region Active-Active |
| 业务规则复杂，但团队不大 | Modular Monolith, DDD, Hexagonal Architecture | 分布式事务、过早拆服务 |
| 多团队并行开发，发布互相阻塞 | Microservices, API Gateway, Observability | 共享数据库的假微服务 |
| 一个动作要触发多个系统 | Event-Driven Architecture | 长同步调用链 |
| 跨服务交易流程复杂 | Saga Architecture, Event-Driven Architecture | 试图用单库事务解决分布式流程 |
| 读多写少，查询模型复杂 | CQRS, Cache, Read-Write Splitting | 所有场景强行 CQRS |
| 单库容量或 QPS 到瓶颈 | Sharding, Read-Write Splitting, Cache | 没有分片键就分库分表 |
| 服务数量多，通信治理复杂 | Service Mesh, Observability, SRE | 没有基础治理就上 Mesh |
| 部署、扩容、回滚困难 | Cloud Native, Kubernetes, GitOps | 手工改线上配置 |
| 峰谷流量明显，小任务多 | Serverless Architecture | 长时间运行、强状态函数 |
| 全球用户访问延迟高 | Edge Architecture, Multi-Region | 单地域硬扛全球流量 |
| 数据分析口径混乱 | Data Warehouse, Data Mesh | 直接查业务库做所有报表 |
| 原始数据类型多，AI 训练需要沉淀 | Data Lake, Lakehouse | 无治理的数据湖 |
| 需要实时风控、推荐或告警 | Streaming Architecture, CDC | T+1 批处理 |
| 企业知识问答 | RAG, Vector Database, LLMOps | 直接 Fine-Tuning 全部知识 |
| AI 需要执行多步骤任务 | Agentic Architecture, LLMOps, Audit | 无权限边界的工具调用 |
| 大团队交付慢、重复造轮子 | Platform Engineering, IDP, GitOps | 平台只做工单入口 |
| 线上问题定位慢 | Observability, SRE | 只有日志没有 Trace 和指标 |
| 权限、审计、合规复杂 | IAM, Zero Trust, Policy as Code, Audit | 权限判断散落在业务 if 中 |

## 2. Selection By Team Size

| Team Size | Good Default | Why |
|---|---|---|
| 1-5 | Modular Monolith + Layered | 简单、可控、低运维成本 |
| 5-20 | Modular Monolith + DDD + Hexagonal | 业务边界开始重要，但仍要控制复杂度 |
| 20-100 | Microservices + Event-Driven + Observability | 多团队并行需要独立边界和可观测性 |
| 100+ | Platform Engineering + IDP + SRE + Security Architecture | 标准化交付、治理和可靠性成为核心问题 |

## 3. Selection By System Pressure

| Pressure | Architecture Response |
|---|---|
| Code Complexity | Layered, Clean, Hexagonal, DDD |
| Team Coupling | Modular Monolith, Microservices, Platform Engineering |
| Runtime Traffic | Cache, Sharding, Rate Limiting, Edge |
| Data Scale | Data Lake, Lakehouse, Streaming, CDC |
| Consistency | Transaction Script, DDD Aggregate, Saga, Event Sourcing |
| Deployment Risk | CI/CD, GitOps, Kubernetes, SRE |
| Security Risk | IAM, Zero Trust, Policy as Code, Audit |
| AI Quality Risk | RAG, LLMOps, Evaluation, Guardrails |

## 4. Selection By Consistency Requirement

| Requirement | Recommended Pattern |
|---|---|
| 单服务内强一致 | Clean Architecture + DDD Aggregate + DB Transaction |
| 跨服务最终一致 | Saga + Event-Driven Architecture |
| 完整审计历史 | Event Sourcing + CQRS |
| 高性能查询但写入严谨 | CQRS + Projection |
| 允许短暂旧数据 | Cache + Read Replica |
| 不能读到旧数据 | 主库读、强一致读模型、避免异步投影 |

## 5. Selection By AI Scenario

| AI Scenario | Recommended Architecture |
|---|---|
| 企业文档问答 | RAG + Vector Database + LLMOps |
| 代码库问答 | RAG + Hybrid Search + Rerank + Trace |
| 固定格式分类/抽取 | Prompt Engineering, Fine-Tuning if needed |
| 多步骤自动化 | Agentic Architecture + Tool Permission + Audit |
| 模型生产化 | MLOps + Model Registry + Monitoring |
| Prompt 频繁变化 | LLMOps + Evaluation + Versioning |

## 6. Red Flags

如果出现这些信号，说明架构可能选重了或选错了：

- 团队 3 个人，却维护 20 个微服务。
- 还没有服务边界，却先上 Service Mesh。
- 简单 CRUD 系统强行 CQRS + Event Sourcing。
- RAG 系统只有向量库，没有权限、评测和 Trace。
- 上 Kubernetes 后发布仍然靠手工命令。
- 数据湖里只有文件，没有 Catalog、Owner 和质量规则。

## 7. Minimal Recommendation

如果你不知道从哪里开始，大多数业务系统可以先用：

```text
Modular Monolith
+ Layered Architecture
+ DDD Tactical Patterns
+ Hexagonal Architecture for external dependencies
+ Observability basics
```

等出现真实压力后，再引入：

```text
Event-Driven
CQRS
Microservices
Kubernetes
Platform Engineering
SRE
```

