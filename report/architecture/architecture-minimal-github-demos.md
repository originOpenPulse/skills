# Architecture Minimal GitHub Demos

> 目标：给上一份架构报告中的每一种 Architecture 找一个 GitHub 上可参考的最小 Demo 或轻量示例。  
> 说明：有些架构本身不是一个“可运行小项目”，而是一类组织方法或平台能力，所以这里选择“最小可学习仓库”或“官方示例仓库”。学习时不要只跑起来，更重要的是看目录结构、依赖方向、边界划分和运行链路。

专题首页：[Big Tech Architecture Atlas](README.md)

配套阅读：

- [Architecture Diagrams, Sequence Diagrams and I/O](architecture-diagrams-and-io.md)
- [Architecture Boundaries and Dependencies](architecture-boundaries-and-dependencies.md)
- [Architecture Review Checklist](architecture-review-checklist.md)
- [Architecture Anti-Patterns](architecture-anti-patterns.md)

## 1. 使用方式

建议按这个顺序学习：

1. 先看 `Application Architecture`，理解代码怎么组织。
2. 再看 `Service Architecture`，理解服务怎么拆、怎么通信。
3. 再看 `Cloud Native Architecture`，理解服务怎么部署和治理。
4. 再看 `Data Architecture`，理解数据如何从业务库流向分析和 AI。
5. 最后看 `AI / Platform / Security / Resilience Architecture`，理解大厂工程体系。

每个 Demo 建议至少做三件事：

- 看 README：先理解它解决什么问题。
- 看目录结构：观察代码边界和模块划分。
- 跑最小流程：只跑核心路径，不急着研究所有细节。

## 2. Application Architecture Demos

| Architecture | 图解入口 | GitHub Demo | 最小学习入口 | 重点看什么 |
|---|---|---|---|---|
| Monolithic Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#21-monolithic-architecture) | https://github.com/spring-projects/spring-petclinic | Spring PetClinic | 一个传统 Web 单体如何组织 Controller、Service、Repository、View |
| Modular Monolith Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#22-modular-monolith-architecture) | https://github.com/kgrzybek/modular-monolith-with-ddd | Modular Monolith with DDD | 一个进程内如何按业务模块隔离边界 |
| Layered Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#23-layered-architecture) | https://github.com/gothinkster/spring-boot-realworld-example-app | RealWorld Spring Boot | Controller、Service、Repository、DTO 的典型分层 |
| Clean Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#24-clean-architecture) | https://github.com/ardalis/CleanArchitecture | Ardalis Clean Architecture | Domain、UseCases、Infrastructure、Web 的依赖方向 |
| Hexagonal Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#25-hexagonal-architecture) | https://github.com/thombergs/buckpal | BuckPal | Port 和 Adapter 如何隔离业务核心与外部系统 |
| Domain-Driven Design Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#26-domain-driven-design-architecture) | https://github.com/VaughnVernon/IDDD_Samples | Implementing DDD Samples | Aggregate、Repository、Domain Event、Bounded Context |

### 学习提醒

Application Architecture 的关键不是技术栈，而是依赖方向。判断一个 Demo 是否值得学，可以看业务核心是否被框架、数据库、Web API 细节污染。

## 3. Service Architecture Demos

| Architecture | 图解入口 | GitHub Demo | 最小学习入口 | 重点看什么 |
|---|---|---|---|---|
| Service-Oriented Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#31-service-oriented-architecture) | https://github.com/ewolff/microservice | Microservice 示例集 | 服务化拆分、独立部署、接口协作 |
| Microservices Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#32-microservices-architecture) | https://github.com/GoogleCloudPlatform/microservices-demo | Online Boutique | 多服务电商系统如何协作 |
| API Gateway Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#33-api-gateway-architecture) | https://github.com/spring-cloud-samples/spring-cloud-gateway-sample | Spring Cloud Gateway Sample | 路由、过滤器、统一入口 |
| Backend for Frontend Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#34-backend-for-frontend-architecture) | https://github.com/adityaeka26/go-bff | Go BFF Demo | 面向不同前端聚合后端接口 |
| Event-Driven Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#35-event-driven-architecture) | https://github.com/confluentinc/kafka-streams-examples | Kafka Streams Examples | Producer、Consumer、Topic、Stream Processing |
| CQRS Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#36-cqrs-architecture) | https://github.com/AxonIQ/giftcard-demo | Axon Giftcard Demo | Command Model 和 Query Model 如何分离 |
| Event Sourcing Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#37-event-sourcing-architecture) | https://github.com/cer/event-sourcing-examples | Event Sourcing Examples | 用事件重建状态、事件存储、投影 |
| Saga Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#38-saga-architecture) | https://github.com/eventuate-tram/eventuate-tram-sagas-examples-customers-and-orders | Customers and Orders Saga | 跨服务业务流程和补偿事务 |
| Service Mesh Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#39-service-mesh-architecture) | https://github.com/istio/istio/tree/master/samples/bookinfo | Istio Bookinfo | Sidecar、流量治理、灰度、mTLS 的入口示例 |

### 学习提醒

Service Architecture 的关键是“边界 + 通信 + 故障处理”。只看服务数量没有意义，真正要看每个服务为什么独立、失败时怎么保护调用链。

## 4. Cloud Native Architecture Demos

| Architecture | 图解入口 | GitHub Demo | 最小学习入口 | 重点看什么 |
|---|---|---|---|---|
| Cloud Native Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#41-cloud-native-architecture) | https://github.com/GoogleCloudPlatform/microservices-demo | Online Boutique | 容器化、多服务、Kubernetes 部署、可观测入口 |
| Kubernetes Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#42-kubernetes-architecture) | https://github.com/kubernetes/examples/tree/master/web/guestbook | Kubernetes Guestbook | Deployment、Service、Pod、Redis、前后端服务发现 |
| Serverless Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#43-serverless-architecture) | https://github.com/serverless/examples | Serverless Examples | Function、Event Trigger、按事件执行 |
| Cell-Based Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#44-cell-based-architecture) | https://github.com/aws-samples/aws-saas-cell-based-architecture | AWS SaaS Cell-Based Architecture | Cell 隔离、故障爆炸半径、按租户/用户分区 |
| Multi-Region Active-Active Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#45-multi-region-active-active-architecture) | https://github.com/aws-samples/multi-region-data-residency | AWS Multi-Region Data Residency | 多地域部署、流量切换、灾备思路 |
| Edge Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#46-edge-architecture) | https://github.com/cloudflare/templates | Cloudflare Workers Templates | 边缘函数、请求拦截、低延迟处理 |

### 学习提醒

Cloud Native Architecture 不等于“用了 Kubernetes”。它强调声明式配置、自动化部署、弹性伸缩、故障恢复和可观测性。

## 5. Data Architecture Demos

| Architecture | 图解入口 | GitHub Demo | 最小学习入口 | 重点看什么 |
|---|---|---|---|---|
| Data Warehouse Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#51-data-warehouse-architecture) | https://github.com/dbt-labs/jaffle-shop | dbt Jaffle Shop | 维度建模、事实表、指标口径、SQL 转换 |
| Data Lake Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#52-data-lake-architecture) | https://github.com/aws-samples/aws-ml-data-lake-workshop | AWS ML Data Lake Workshop | 原始数据、权限、元数据、数据治理 |
| Lakehouse Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#53-lakehouse-architecture) | https://github.com/delta-io/delta | Delta Lake Examples | ACID Table、Time Travel、批流一体 |
| Streaming Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#54-streaming-architecture) | https://github.com/apache/flink-training | Apache Flink Training | 流处理、窗口、状态、事件时间 |
| Lambda Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#55-lambda-architecture) | https://github.com/apssouza22/big-data-pipeline-lambda-arch | Big Data Pipeline Lambda Architecture | Batch Layer、Speed Layer、Serving Layer |
| Kappa Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#56-kappa-architecture) | https://github.com/confluentinc/kafka-streams-examples | Kafka Streams Examples | 只用流处理和日志重放处理数据 |
| Change Data Capture Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#57-change-data-capture-architecture) | https://github.com/debezium/debezium-examples | Debezium Examples | 数据库 Binlog -> Kafka -> 下游系统 |
| Data Mesh Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#58-data-mesh-architecture) | https://github.com/datahub-project/datahub | DataHub | Data Product、领域数据所有权、元数据治理、自助发现 |

### 学习提醒

Data Architecture 的关键不是把数据搬过去，而是数据质量、血缘、权限、口径和时效性。Demo 跑通只是第一步。

## 6. AI Architecture Demos

| Architecture | 图解入口 | GitHub Demo | 最小学习入口 | 重点看什么 |
|---|---|---|---|---|
| Machine Learning Platform Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#61-machine-learning-platform-architecture) | https://github.com/kubeflow/examples | Kubeflow Examples | 数据、训练、模型发布流水线 |
| MLOps Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#62-mlops-architecture) | https://github.com/iterative/example-get-started | DVC Get Started | 数据版本、模型版本、实验追踪 |
| RAG Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#63-rag-architecture) | https://github.com/openai/openai-cookbook/tree/main/examples | OpenAI Cookbook Examples | Embedding、Retrieval、Generation、Evaluation |
| Agentic Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#64-agentic-architecture) | https://github.com/openai/openai-agents-python | OpenAI Agents SDK | Agent、Tool、Handoff、Guardrail |
| LLMOps Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#65-llmops-architecture) | https://github.com/Arize-ai/phoenix | Phoenix | Prompt/Trace/Evaluation/LLM Observability |
| Vector Database Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#66-vector-database-architecture) | https://github.com/milvus-io/bootcamp | Milvus Bootcamp | 向量写入、相似度检索、RAG 检索链路 |

### 学习提醒

AI Architecture 不只是“调模型 API”。企业级 AI 系统一定会遇到知识更新、权限控制、评测、成本、延迟、可观测性和安全边界。

## 7. Engineering Productivity Architecture Demos

| Architecture | 图解入口 | GitHub Demo | 最小学习入口 | 重点看什么 |
|---|---|---|---|---|
| DevOps Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#71-devops-architecture) | https://github.com/actions/starter-workflows | GitHub Actions Starter Workflows | CI、测试、构建、发布流水线 |
| GitOps Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#72-gitops-architecture) | https://github.com/argoproj/argocd-example-apps | Argo CD Example Apps | Git 作为部署事实来源、应用同步 |
| Platform Engineering Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#73-platform-engineering-architecture) | https://github.com/backstage/backstage | Backstage | Developer Portal、Catalog、插件平台 |
| Internal Developer Platform Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#74-internal-developer-platform-architecture) | https://github.com/backstage/backstage/tree/master/packages/create-app | Backstage Create App | 服务目录、模板、文档、研发入口 |
| Observability Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#75-observability-architecture) | https://github.com/open-telemetry/opentelemetry-demo | OpenTelemetry Demo | Trace、Metric、Log 如何贯穿多服务 |
| SRE Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#76-sre-architecture) | https://github.com/google/slo-generator | Google SLO Generator | SLI、SLO、Error Budget 的配置化实践 |

### 学习提醒

Engineering Productivity Architecture 的目标是让研发团队“更快、更稳、更少重复劳动”。不要只看工具，要看工具如何嵌入开发、测试、发布和运维流程。

## 8. Security Architecture Demos

| Architecture | 图解入口 | GitHub Demo | 最小学习入口 | 重点看什么 |
|---|---|---|---|---|
| Zero Trust Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#81-zero-trust-architecture) | https://github.com/aws-samples/moving-to-a-zero-trust-architecture-in-aws | Moving to Zero Trust on AWS | 身份感知访问、零信任访问入口 |
| IAM Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#82-iam-architecture) | https://github.com/keycloak/keycloak-quickstarts | Keycloak Quickstarts | OAuth2、OIDC、SSO、角色权限 |
| Policy as Code Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#83-policy-as-code-architecture) | https://github.com/nmnellis/opa-examples | OPA Examples | Policy、Input、Decision、CI/CD 或 Kubernetes 门禁 |
| Audit Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#84-audit-architecture) | https://github.com/arkime/arkime | Arkime | 网络流量审计、检索、追踪和取证 |

### 学习提醒

Security Architecture 的价值在于默认可控、默认可审计。尤其在 AI Agent 和微服务系统里，权限边界比功能本身更重要。

## 9. High Performance and Resilience Architecture Demos

| Architecture | 图解入口 | GitHub Demo | 最小学习入口 | 重点看什么 |
|---|---|---|---|---|
| Cache Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#91-cache-architecture) | https://github.com/redis/redis-om-spring | Redis OM Spring | Redis 缓存、查询、对象映射 |
| Sharding Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#92-sharding-architecture) | https://github.com/apache/shardingsphere-example | ShardingSphere Example | 分库分表、分片键、读写路由 |
| Read-Write Splitting Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#93-read-write-splitting-architecture) | https://github.com/apache/shardingsphere-example | ShardingSphere Example | 主从读写分离配置 |
| Circuit Breaker and Bulkhead Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#94-circuit-breaker-and-bulkhead-architecture) | https://github.com/resilience4j/resilience4j-spring-boot2-demo | Resilience4j Demo | 熔断、限流、隔离、重试、超时 |
| Rate Limiting and Degradation Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#95-rate-limiting-and-degradation-architecture) | https://github.com/alibaba/Sentinel | Alibaba Sentinel | 限流、降级、热点参数、流控规则 |

### 学习提醒

高性能架构和韧性架构要一起看。单纯追求 QPS 容易把系统做脆，真正的大厂系统会同时考虑限流、降级、隔离、恢复和观测。

## 10. Frontend and Client Architecture Demos

| Architecture | 图解入口 | GitHub Demo | 最小学习入口 | 重点看什么 |
|---|---|---|---|---|
| Micro Frontends Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#101-micro-frontends-architecture) | https://github.com/single-spa/single-spa | single-spa | 多前端应用注册、挂载、卸载 |
| Component-Based Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#102-component-based-architecture) | https://github.com/storybookjs/storybook | Storybook | 组件隔离开发、组件文档、视觉测试 |
| Design System Architecture | [架构图 + 时序图](architecture-diagrams-and-io.md#103-design-system-architecture) | https://github.com/primer/react | GitHub Primer React | 设计系统组件、Token、工程化复用 |

### 学习提醒

Frontend Architecture 的核心是长期维护体验。组件化解决复用，设计系统解决一致性，微前端解决大团队和大应用边界。

## 11. 推荐练习路线

如果你想真正从 Demo 里学会架构，可以按下面路线做：

1. 跑通 `spring-petclinic`，画出 Monolithic Architecture 调用链。
2. 阅读 `modular-monolith-with-ddd`，标出每个业务模块的边界。
3. 跑 `buckpal`，理解 Hexagonal Architecture 的 Port 和 Adapter。
4. 跑 `microservices-demo`，画出服务依赖图。
5. 加入 `spring-cloud-gateway-sample`，理解统一入口。
6. 跑 `kafka-streams-examples` 或 `debezium-examples`，理解事件和数据流。
7. 跑 `kubernetes/examples/web/guestbook`，理解 Kubernetes 最小部署单元。
8. 看 `istio/bookinfo`，理解服务治理为什么会独立出来。
9. 跑 `opentelemetry-demo`，理解可观测性如何贯穿系统。
10. 跑一个 `RAG` 示例，把自己的 Markdown 文档接入向量检索。

## 12. 最值得优先看的 10 个 Demo

如果时间有限，优先看这 10 个：

| 优先级 | Demo | 原因 |
|---|---|---|
| 1 | https://github.com/spring-projects/spring-petclinic | 最经典的单体 Web 应用 |
| 2 | https://github.com/kgrzybek/modular-monolith-with-ddd | 学模块化单体和 DDD 很好 |
| 3 | https://github.com/thombergs/buckpal | Hexagonal Architecture 极简清晰 |
| 4 | https://github.com/GoogleCloudPlatform/microservices-demo | 微服务、云原生、电商场景完整 |
| 5 | https://github.com/istio/istio/tree/master/samples/bookinfo | Service Mesh 入门标准样例 |
| 6 | https://github.com/kubernetes/examples/tree/master/web/guestbook | Kubernetes 最小经典例子 |
| 7 | https://github.com/debezium/debezium-examples | CDC 学习很实用 |
| 8 | https://github.com/open-telemetry/opentelemetry-demo | 可观测性非常贴近大厂实践 |
| 9 | https://github.com/openai/openai-cookbook/tree/main/examples | RAG 和 LLM 应用最容易上手 |
| 10 | https://github.com/backstage/backstage | 理解平台工程和内部开发者平台 |

## 13. 后续可以继续补充的内容

这份清单解决“看什么 Demo”的问题。下一步可以继续拆成更实操的版本：

- 每个 Demo 的本地运行步骤。
- 每个 Demo 的目录结构解读。
- 每个 Demo 对应的架构图。
- 每个 Demo 的最小改造任务。
- 按 Java / Go / Python / Rust / TypeScript 技术栈重新分类。
- 把这些 Demo 做成一份 30 天架构学习路线。
