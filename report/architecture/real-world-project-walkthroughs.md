# Real-World Project Walkthroughs

> 目标：用真实 GitHub 项目练习“从代码看架构”。  
> 方法：先判断架构类型，再找 Core、Boundary、Dependency、I/O、Failure Path。

专题首页：[Big Tech Architecture Atlas](README.md)

## 1. Spring PetClinic

GitHub: https://github.com/spring-projects/spring-petclinic

### What To Learn

Spring PetClinic 是理解 **Monolithic Architecture** 和 **Layered Architecture** 的经典示例。

### Architecture Identified

| Layer | What To Look For |
|---|---|
| Presentation | Controller、Web routes |
| Application/Business | Service-like operations and validation |
| Data Access | Repository |
| Dependency | Spring Boot, database, template engine |

### Reading Path

```text
HTTP Request
-> Controller
-> Repository / Domain Object
-> Database
-> View / Response
```

### Review Questions

- 业务逻辑主要在 Controller、Domain Object 还是 Repository 附近？
- 如果要改成 Modular Monolith，应该按哪些模块拆？
- Database 是架构本体，还是依赖？
- 哪些地方适合抽成 Use Case？

### What Not To Overread

不要把 Spring Boot 当作架构本体。这里真正值得看的，是一个小型单体应用如何组织 Web、Domain 和 Data Access。

## 2. GoogleCloudPlatform Microservices Demo

GitHub: https://github.com/GoogleCloudPlatform/microservices-demo

### What To Learn

Online Boutique 是理解 **Microservices Architecture**、**Cloud Native Architecture** 和 **Observability** 的好入口。

### Architecture Identified

| Boundary | What To Look For |
|---|---|
| Service Boundary | cartservice、checkoutservice、paymentservice、shippingservice |
| Communication | gRPC / HTTP service calls |
| Runtime | Kubernetes manifests |
| Dependency | Redis, container registry, Kubernetes |

### Reading Path

```text
Frontend
-> Checkout Service
-> Cart Service
-> Payment Service
-> Shipping Service
-> Email Service
```

### Review Questions

- 每个服务是否有清晰职责？
- 哪些调用是同步的？
- 如果 Payment Service 失败，Checkout 如何处理？
- Kubernetes 是微服务本体，还是运行依赖？
- 哪些地方需要 Trace 才能排查问题？

### What Not To Overread

不要以为“服务数量多”就是微服务成熟。重点看服务边界、通信契约、故障路径和可观测性。

## 3. Istio Bookinfo

GitHub: https://github.com/istio/istio/tree/master/samples/bookinfo

### What To Learn

Bookinfo 是理解 **Service Mesh Architecture** 的标准样例。

### Architecture Identified

| Part | What To Look For |
|---|---|
| Business Services | productpage、details、reviews、ratings |
| Mesh Core | Sidecar proxy, traffic policy, routing |
| Control Plane | Istio config |
| Dependency | Kubernetes, Envoy |

### Reading Path

```text
Request
-> Product Page
-> Sidecar Proxy
-> Reviews / Details / Ratings
-> Traffic Policy applies
```

### Review Questions

- 哪些逻辑属于业务服务？
- 哪些逻辑属于 Mesh？
- 灰度发布和流量切分在哪里配置？
- mTLS、Retry、Timeout 是业务代码做的，还是 Sidecar 做的？

### What Not To Overread

Service Mesh 不是业务架构，它治理的是服务间通信。业务边界仍然要由服务设计自己负责。

## 4. OpenTelemetry Demo

GitHub: https://github.com/open-telemetry/opentelemetry-demo

### What To Learn

OpenTelemetry Demo 适合理解 **Observability Architecture**。

### Architecture Identified

| Signal | What To Look For |
|---|---|
| Metrics | System and business measurements |
| Logs | Event details |
| Traces | Cross-service request path |
| Collector | Telemetry pipeline |

### Reading Path

```text
Service emits telemetry
-> OpenTelemetry Collector
-> Backend storage / dashboard
-> Alert or analysis
```

### Review Questions

- 一次用户请求跨了哪些服务？
- Trace 中的 Span 是否能定位慢点？
- Metrics、Logs、Traces 能否关联？
- 如果线上出问题，哪个信号最先告诉你？

### What Not To Overread

Observability 不是“装一个监控工具”。它的核心是让系统状态可以被推断和解释。

## 5. OpenAI Cookbook RAG Examples

GitHub: https://github.com/openai/openai-cookbook/tree/main/examples

### What To Learn

OpenAI Cookbook 中的示例适合理解 **RAG Architecture** 和 **LLMOps** 的基础链路。

### Architecture Identified

| Part | What To Look For |
|---|---|
| Document Processing | Chunking and cleanup |
| Retrieval | Embedding and search |
| Generation | Prompt with context |
| Evaluation | Answer quality checks |

### Reading Path

```text
Document
-> Chunk
-> Embed
-> Store/Search
-> Retrieve Context
-> LLM Answer
```

### Review Questions

- Chunk 大小如何影响检索？
- 检索结果是否足够回答问题？
- Prompt 是否明确要求基于上下文回答？
- 是否有评测集检查回答质量？
- 是否有权限过滤？

### What Not To Overread

向量库不是 RAG 的全部。RAG 的质量来自文档治理、检索、上下文组装、Prompt、评测和权限控制。

