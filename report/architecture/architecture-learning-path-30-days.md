# 30-Day Architecture Learning Path

> 目标：用 30 天把“架构名词印象”变成“能读代码、能画图、能判断边界”的能力。

专题首页：[Big Tech Architecture Atlas](README.md)

## Learning Principle

不要平均学习所有架构。主线建议是：

```text
Modular Monolith
-> DDD
-> Hexagonal Architecture
-> Event-Driven Architecture
-> Microservices
-> Observability and Resilience
-> RAG / Agentic Architecture
```

## Week 1: Application Architecture

目标：看懂代码怎么分层、模块怎么隔离、业务核心在哪里。

| Day | Topic | Read | Practice |
|---|---|---|---|
| 1 | Architecture Map | [Big Tech Architecture Report](big-tech-architecture-report.md) | 选 5 个你最常见的架构，写出它们解决什么问题 |
| 2 | Boundaries | [Architecture Boundaries and Dependencies](architecture-boundaries-and-dependencies.md) | 任意项目中标出 Core、Boundary、Dependency |
| 3 | Layered Architecture | [Diagrams and I/O](architecture-diagrams-and-io.md#23-layered-architecture) | 画 Controller -> Service -> Repository 时序图 |
| 4 | Clean Architecture | [Diagrams and I/O](architecture-diagrams-and-io.md#24-clean-architecture) | 找出业务核心是否依赖框架 |
| 5 | Hexagonal Architecture | [Diagrams and I/O](architecture-diagrams-and-io.md#25-hexagonal-architecture) | 写出 Input Port 和 Output Port |
| 6 | Modular Monolith | [Diagrams and I/O](architecture-diagrams-and-io.md#22-modular-monolith-architecture) | 把一个单体项目按模块重新画图 |
| 7 | Review | [Architecture Review Checklist](architecture-review-checklist.md) | 对一个 GitHub Demo 做 30 分钟 Review |

## Week 2: Service and Reliability Architecture

目标：理解系统为什么拆服务，以及拆了之后如何通信、事务和防故障。

| Day | Topic | Read | Practice |
|---|---|---|---|
| 8 | Microservices | [Diagrams and I/O](architecture-diagrams-and-io.md#32-microservices-architecture) | 画 Online Boutique 服务依赖图 |
| 9 | API Gateway and BFF | [Demo List](architecture-minimal-github-demos.md#3-service-architecture-demos) | 区分网关职责和 BFF 职责 |
| 10 | Event-Driven | [Diagrams and I/O](architecture-diagrams-and-io.md#35-event-driven-architecture) | 为 OrderCreated 设计事件消费者 |
| 11 | CQRS | [Diagrams and I/O](architecture-diagrams-and-io.md#36-cqrs-architecture) | 设计一个订单读模型 |
| 12 | Saga | [Diagrams and I/O](architecture-diagrams-and-io.md#38-saga-architecture) | 写出支付失败后的补偿步骤 |
| 13 | Resilience | [Anti-Patterns](architecture-anti-patterns.md) | 找出 Retry Storm、No Timeout、No Fallback 风险 |
| 14 | Review | [Architecture Review Checklist](architecture-review-checklist.md) | 输出一份 Service Architecture Review |

## Week 3: Cloud Native, Data and Platform

目标：理解服务如何部署、数据如何流动、工程平台如何提升团队效率。

| Day | Topic | Read | Practice |
|---|---|---|---|
| 15 | Kubernetes | [Diagrams and I/O](architecture-diagrams-and-io.md#42-kubernetes-architecture) | 解释 Desired State 和 Reconciliation |
| 16 | GitOps | [Diagrams and I/O](architecture-diagrams-and-io.md#72-gitops-architecture) | 画 Git -> Controller -> Cluster 流程 |
| 17 | Observability | [Diagrams and I/O](architecture-diagrams-and-io.md#75-observability-architecture) | 设计 Metrics、Logs、Traces 三类信号 |
| 18 | Data Warehouse and Lakehouse | [Diagrams and I/O](architecture-diagrams-and-io.md#51-data-warehouse-architecture) | 区分业务库、数仓、数据湖 |
| 19 | Streaming and CDC | [Diagrams and I/O](architecture-diagrams-and-io.md#57-change-data-capture-architecture) | 画 MySQL -> Kafka -> Search 流程 |
| 20 | Platform Engineering | [Diagrams and I/O](architecture-diagrams-and-io.md#73-platform-engineering-architecture) | 设计一个新服务 Golden Path |
| 21 | Review | [Case Studies](architecture-case-studies.md) | 选择一个案例，画组件图和时序图 |

## Week 4: AI, Security and Architecture Decisions

目标：把现代 AI 架构、安全治理和架构决策结合起来。

| Day | Topic | Read | Practice |
|---|---|---|---|
| 22 | RAG | [Diagrams and I/O](architecture-diagrams-and-io.md#63-rag-architecture) | 给自己的 Markdown 文档设计 RAG 流程 |
| 23 | LLMOps | [Diagrams and I/O](architecture-diagrams-and-io.md#65-llmops-architecture) | 设计 Prompt 版本和评测集 |
| 24 | Agentic Architecture | [Diagrams and I/O](architecture-diagrams-and-io.md#64-agentic-architecture) | 列出 Agent 工具权限和审计点 |
| 25 | Zero Trust and IAM | [Diagrams and I/O](architecture-diagrams-and-io.md#81-zero-trust-architecture) | 设计一次访问决策流程 |
| 26 | ADR | [ADR README](adr/README.md) | 写一个架构决策记录 |
| 27 | Anti-Patterns | [Anti-Patterns](architecture-anti-patterns.md) | 给一个项目找 5 个架构异味 |
| 28 | Full Review | [Review Checklist](architecture-review-checklist.md) | 输出一份完整架构 Review |
| 29 | Refactor Plan | [Boundaries](architecture-boundaries-and-dependencies.md) | 提出 3 个边界改进建议 |
| 30 | Portfolio | All Docs | 整理一篇“我如何分析一个系统架构”的文章 |

## Final Deliverable

30 天结束时，建议产出：

- 一份架构 Review。
- 一张组件图。
- 一张时序图。
- 一份 I/O Contract。
- 一份 ADR。
- 一份 Anti-Pattern 修复建议。

