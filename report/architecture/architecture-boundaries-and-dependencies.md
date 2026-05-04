# Architecture Boundaries and Dependencies

> 目标：把每一种 Architecture 的“本体内容”和“外部依赖”隔离开。  
> 读架构时最容易混乱的点是：图里出现了很多组件，但不是所有组件都属于这个架构。有些是核心内容，有些只是依赖、运行环境、配套工具或下游系统。

专题首页：[Big Tech Architecture Atlas](README.md)

配套阅读：

- [Architecture Anti-Patterns](architecture-anti-patterns.md)
- [Architecture Review Checklist](architecture-review-checklist.md)
- [Architecture Decision Records](adr/README.md)

## 1. 判断规则

看一个架构时，先分 4 层：

| 层级 | 含义 | 例子 |
|---|---|---|
| Core | 这个架构真正要表达的核心结构 | Clean Architecture 的 Use Case、Entity、Port |
| Boundary | 这个架构划出来的边界 | Microservices 的服务边界、DDD 的 Bounded Context |
| Dependency | 为了让架构运行而需要的外部依赖 | Database、Message Broker、Kubernetes、LLM Provider |
| Supporting Capability | 配套治理能力，不是架构本体但经常一起出现 | Observability、CI/CD、Auth、Rate Limit |

一个简单判断：

- 去掉它，架构思想就不成立：通常是 `Core`。
- 换成别的实现，架构仍然成立：通常是 `Dependency`。
- 帮它更好运行，但不是它定义的一部分：通常是 `Supporting Capability`。

## 2. Application Architecture

| Architecture | Core：主要内容 | Boundary：边界 | Dependencies：常见依赖 | 不要混淆 |
|---|---|---|---|---|
| Monolithic Architecture | 一个部署单元内包含 UI、业务逻辑、数据访问 | 应用进程边界 | Database、Web Server、Framework | 数据库不是单体架构本体，只是它常用的持久化依赖 |
| Modular Monolith Architecture | 单体内部的业务模块、模块接口、模块依赖规则 | Module Boundary | Database、Framework、Module Event Bus | 它不是 Microservices，模块独立不等于服务独立部署 |
| Layered Architecture | Presentation、Application、Domain、Infrastructure 分层 | Layer Boundary | Framework、ORM、Database、HTTP | Spring MVC、ORM 是实现工具，不是分层架构本身 |
| Clean Architecture | Entity、Use Case、Input/Output Port、Adapter | Use Case Boundary | Web、DB、External API、Framework | 数据库和 Web 框架在外圈，不应该成为核心业务依赖 |
| Hexagonal Architecture | Application Core、Input Port、Output Port、Adapter | Port Boundary | REST、CLI、Message Queue、DB、Third-party API | Adapter 是接入方式，核心是 Port 隔离 |
| Domain-Driven Design Architecture | Bounded Context、Aggregate、Entity、Value Object、Domain Event | Domain Boundary | Repository、Database、Message Broker | DDD 不是目录命名法，核心是业务语义和领域边界 |

## 3. Service Architecture

| Architecture | Core：主要内容 | Boundary：边界 | Dependencies：常见依赖 | 不要混淆 |
|---|---|---|---|---|
| Service-Oriented Architecture | 可复用企业服务、服务契约、服务编排 | Enterprise Service Boundary | ESB、SOAP/REST、Legacy System | ESB 是常见实现，不是 SOA 的全部 |
| Microservices Architecture | 独立服务、独立部署、服务自治、服务间通信 | Service Boundary | API Gateway、DB、Message Broker、Service Registry | Kubernetes 不是微服务本体，只是常见运行环境 |
| API Gateway Architecture | 统一入口、路由、鉴权、限流、协议转换 | Edge/API Boundary | Auth Service、Backend Services、Rate Limit Store | 后端业务服务不是网关本体 |
| Backend for Frontend Architecture | 面向特定前端的聚合 API、View Model 转换 | Client Experience Boundary | Backend Services、Auth、Cache | BFF 不是通用 API 网关，它更贴近页面和端体验 |
| Event-Driven Architecture | Event、Producer、Consumer、Event Contract、异步解耦 | Event Boundary | Kafka/Pulsar/RabbitMQ、Schema Registry | Message Broker 是依赖，架构核心是事件语义和订阅关系 |
| CQRS Architecture | Command Model、Query Model、Projection | Read/Write Model Boundary | Database、Event Bus、Cache、Search Engine | CQRS 不等于必须 Event Sourcing |
| Event Sourcing Architecture | Event Store、事件追加、状态重放、Projection | Event Stream Boundary | Event Store、Read Model、Message Broker | 当前状态表不是事实来源，事件历史才是核心 |
| Saga Architecture | 本地事务、流程步骤、补偿动作、最终一致 | Transaction Boundary | Message Broker、Workflow Engine、Service APIs | Saga 不是强一致分布式事务，它接受中间状态 |
| Service Mesh Architecture | Sidecar Proxy、Control Plane、Traffic Policy、mTLS | Service-to-Service Communication Boundary | Kubernetes、Envoy、Certificate Authority | 业务服务不是 Mesh 本体，Mesh 治理的是通信面 |

## 4. Cloud Native Architecture

| Architecture | Core：主要内容 | Boundary：边界 | Dependencies：常见依赖 | 不要混淆 |
|---|---|---|---|---|
| Cloud Native Architecture | 容器化、声明式配置、弹性、自动化、可恢复 | Runtime/Deployment Boundary | Container Registry、Kubernetes、CI/CD、Observability | 云原生不是单一工具集合，而是一组交付和运行原则 |
| Kubernetes Architecture | Desired State、Pod、Deployment、Service、Controller | Cluster Boundary | Container Runtime、etcd、Image Registry、CNI | 应用业务逻辑不属于 Kubernetes 架构本体 |
| Serverless Architecture | Function、Event Trigger、Managed Runtime、按需伸缩 | Function Execution Boundary | Cloud Provider、Managed DB、Event Source | 无服务器不是没有服务器，而是不管理服务器 |
| Cell-Based Architecture | Cell、Cell Router、Cell Isolation、Blast Radius Control | Cell Boundary | Control Plane、Regional Infra、Tenant Mapping DB | Cell 不是普通分片，它强调故障隔离和独立运维 |
| Multi-Region Active-Active Architecture | 多地域同时服务、健康路由、跨地域复制 | Region Boundary | DNS/Traffic Manager、Replication、Global DB | 多活核心难点是数据一致性，不只是多部署几个机房 |
| Edge Architecture | Edge Node、Edge Cache、Edge Function、Origin Shield | Edge Boundary | CDN、Origin Service、WAF、KV Store | 源站不是边缘架构本体，边缘层是靠近用户的计算和缓存 |

## 5. Data Architecture

| Architecture | Core：主要内容 | Boundary：边界 | Dependencies：常见依赖 | 不要混淆 |
|---|---|---|---|---|
| Data Warehouse Architecture | 主题建模、事实表、维度表、指标口径 | Analytical Data Boundary | ETL/ELT、BI、Source DB | 业务库不是数仓，数仓服务分析而不是交易 |
| Data Lake Architecture | 原始数据存储、元数据、Schema-on-Read | Raw Data Boundary | Object Storage、Catalog、Compute Engine | 数据湖不是“文件堆”，缺少治理会变成 Data Swamp |
| Lakehouse Architecture | Open Table Format、ACID Table、统一批流和 BI/AI | Table Format Boundary | Object Storage、Query Engine、Catalog | Lakehouse 核心是表格式和事务能力，不只是把数据放对象存储 |
| Streaming Architecture | Event Stream、Stream Processor、Window、State | Stream Processing Boundary | Kafka/Pulsar、Flink/Spark、Sink Store | Broker 只是管道，流处理逻辑才是架构核心 |
| Lambda Architecture | Batch Layer、Speed Layer、Serving Layer | Batch/Speed Boundary | Batch Engine、Stream Engine、Serving DB | 它的代价是维护两套处理逻辑 |
| Kappa Architecture | Immutable Log、Single Stream Processor、Replay | Log Replay Boundary | Kafka/Pulsar、Stream Processor、Serving Store | Kappa 不是没有历史数据，而是用日志重放处理历史 |
| Change Data Capture Architecture | Change Log、CDC Connector、Change Event | Database Change Boundary | Binlog/WAL、Kafka、Sink Connector | CDC 不应该靠业务代码手写同步逻辑 |
| Data Mesh Architecture | Data Product、Domain Ownership、Federated Governance | Domain Data Boundary | Data Platform、Catalog、Policy Engine | Data Mesh 不是买一个数据平台，而是组织责任重构 |

## 6. AI Architecture

| Architecture | Core：主要内容 | Boundary：边界 | Dependencies：常见依赖 | 不要混淆 |
|---|---|---|---|---|
| Machine Learning Platform Architecture | Training Pipeline、Model Registry、Serving、Monitoring | Model Lifecycle Boundary | Data Store、Feature Store、Compute、Serving Infra | Notebook 不是平台，生命周期治理才是核心 |
| MLOps Architecture | 数据版本、模型版本、实验追踪、部署回滚、漂移监控 | Model Operation Boundary | CI/CD、Registry、Monitoring、Data Versioning | MLOps 不是单纯训练模型，而是生产化治理 |
| RAG Architecture | Chunking、Embedding、Retrieval、Rerank、Grounded Generation | Knowledge Context Boundary | Vector DB、LLM、Document Store、Embedding Model | 向量数据库不是 RAG 全部，检索质量和上下文组装更关键 |
| Agentic Architecture | Planner、Agent Loop、Tool Calling、Observation、Guardrails | Action Boundary | Tools/APIs、LLM、Memory、Permission System | Agent 不是聊天机器人，它会执行动作，所以权限和审计更重要 |
| LLMOps Architecture | Prompt Versioning、Trace、Evaluation、Model Routing、Cost Control | LLM Application Operation Boundary | LLM Provider、Gateway、Eval Dataset、Observability | Prompt 不是临时字符串，而是需要版本和评测的资产 |
| Vector Database Architecture | Vector Index、Similarity Search、Metadata Filter、Top-K Retrieval | Semantic Search Boundary | Embedding Model、Storage、Reranker | Embedding 模型是依赖，向量库核心是索引和检索 |

## 7. Engineering Productivity Architecture

| Architecture | Core：主要内容 | Boundary：边界 | Dependencies：常见依赖 | 不要混淆 |
|---|---|---|---|---|
| DevOps Architecture | 开发、测试、发布、监控反馈闭环 | Delivery Boundary | Git、CI/CD、Artifact Registry、Monitoring | DevOps 不是某个工具，而是交付协作方式 |
| GitOps Architecture | Git Desired State、Controller、Reconciliation、Drift Detection | Environment State Boundary | Kubernetes、Git Repo、Argo CD/Flux | GitOps 不等于把 YAML 放 Git，核心是自动对账和收敛 |
| Platform Engineering Architecture | Golden Path、Self-Service、Platform APIs、Developer Experience | Platform Product Boundary | CI/CD、Cloud APIs、Templates、Observability | 平台工程不是运维平台换皮，而是把平台当产品 |
| Internal Developer Platform Architecture | Service Catalog、Scaffolder、Docs、Runtime View | Developer Entry Boundary | Backstage、Git、CI/CD、Cloud APIs | IDP 是研发入口，不等于所有底层系统都在 IDP 内部 |
| Observability Architecture | Metrics、Logs、Traces、Profiles、Correlation | Telemetry Boundary | Collector、Storage、Dashboard、Alertmanager | 监控是子集，可观测性强调从信号推断系统状态 |
| SRE Architecture | SLI、SLO、Error Budget、Incident Response、Toil Reduction | Reliability Boundary | Monitoring、Alerting、Runbook、Automation | SRE 不是值班团队，而是可靠性工程体系 |

## 8. Security Architecture

| Architecture | Core：主要内容 | Boundary：边界 | Dependencies：常见依赖 | 不要混淆 |
|---|---|---|---|---|
| Zero Trust Architecture | Verify Explicitly、Least Privilege、Assume Breach、Continuous Authorization | Access Boundary | IdP、Device Posture、Policy Engine、Proxy | VPN 不是零信任，位置可信不等于身份可信 |
| IAM Architecture | Identity、Authentication、Authorization、Token、Role/Policy | Identity Boundary | IdP、Directory、OAuth2/OIDC、MFA | 账号系统不是 IAM 全部，权限决策同样关键 |
| Policy as Code Architecture | Policy Code、Decision Engine、Input、Evaluation Result | Policy Decision Boundary | OPA/Kyverno、CI/CD、Kubernetes/API Gateway | 策略本身要可测试、可版本化，而不只是写在文档里 |
| Audit Architecture | Audit Event、Immutable Log、Search、Evidence Chain | Evidence Boundary | Log Store、SIEM、IAM、App Event | 普通业务日志不等于审计日志，审计强调不可抵赖和证据链 |

## 9. High Performance and Resilience Architecture

| Architecture | Core：主要内容 | Boundary：边界 | Dependencies：常见依赖 | 不要混淆 |
|---|---|---|---|---|
| Cache Architecture | Cache Key、TTL、Eviction、Cache Aside/Write Through | Hot Data Boundary | Redis、CDN、Local Cache、Database | Redis 不是缓存架构本体，缓存策略才是核心 |
| Sharding Architecture | Shard Key、Shard Router、Shard Mapping、Resharding | Data Partition Boundary | Database Cluster、Middleware、Metadata Store | 分表不等于分片，分片核心是路由和扩容规则 |
| Read-Write Splitting Architecture | Primary Write、Replica Read、Read Router、Replication Lag Handling | Read/Write Boundary | Primary DB、Replica DB、Proxy | 读写分离核心问题是复制延迟 |
| Circuit Breaker and Bulkhead Architecture | Failure Threshold、Open/Half-Open/Closed、Resource Isolation | Failure Isolation Boundary | Resilience Library、Thread Pool、Dependency Service | 重试不是熔断，重试过多反而可能放大故障 |
| Rate Limiting and Degradation Architecture | Limit Rule、Token/Window、Priority、Fallback、Feature Switch | Traffic Protection Boundary | Redis、Gateway、Config Center、Sentinel | 限流不是拒绝用户，而是保护核心链路 |

## 10. Frontend and Client Architecture

| Architecture | Core：主要内容 | Boundary：边界 | Dependencies：常见依赖 | 不要混淆 |
|---|---|---|---|---|
| Micro Frontends Architecture | App Shell、Micro App、Route Ownership、Independent Deployment | Frontend App Boundary | Module Federation、single-spa、CDN、BFF | 微前端不是 iframe 拼页面，核心是团队和发布边界 |
| Component-Based Architecture | Component、Props、State、Event、Composition | Component Boundary | React/Vue/Svelte、Storybook、Build Tool | 框架不是组件化本身，组件契约和复用边界才是核心 |
| Design System Architecture | Design Token、Component Library、Guidelines、Documentation | Product Experience Boundary | Figma、Storybook、Package Registry、CI | UI 组件库只是设计系统的一部分 |

## 11. 推荐阅读方式

读任意一个 GitHub Demo 时，可以按这个模板拆：

```text
Architecture:
  Core:
    - 这个架构真正定义的组件是什么？
  Boundary:
    - 它把哪些东西隔离开了？
  Dependencies:
    - 它依赖哪些外部组件才能跑起来？
  Supporting Capabilities:
    - 哪些只是监控、部署、安全、治理能力？
  I/O:
    - 它吃什么输入？
    - 它产出什么输出？
  Failure:
    - 它失败时会影响谁？
```

一个例子：

```text
Architecture: RAG Architecture
Core:
  - Chunking
  - Embedding
  - Retrieval
  - Context Assembly
  - Grounded Generation
Boundary:
  - Knowledge Context Boundary
Dependencies:
  - Embedding Model
  - Vector Database
  - LLM Provider
  - Document Store
Supporting Capabilities:
  - Permission Filter
  - Evaluation
  - Trace
  - Cost Monitoring
I/O:
  - Input: Question + Documents
  - Output: Retrieved Context + Answer
Failure:
  - 检索错了，模型大概率答错
```

这样看架构会清爽很多：先看本体，再看依赖，最后看治理能力。
