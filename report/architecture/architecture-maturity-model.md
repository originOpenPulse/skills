# Architecture Maturity Model

> 目标：标记每种 Architecture 的学习难度、生产常见度、适用阶段和使用风险，避免“看到一个高级架构就想马上用”。

专题首页：[Big Tech Architecture Atlas](README.md)

## 1. Labels

| Label | Meaning |
|---|---|
| Beginner Friendly | 适合初学者和小项目建立基础能力 |
| Production Common | 大量生产系统常见 |
| Advanced | 需要较强工程经验和团队协作能力 |
| Use With Caution | 容易被过度使用或误用 |
| Big-Team Only | 通常只有较大团队或平台型组织才值得投入 |

## 2. Maturity Table

| Architecture | Learning Level | Production Common | Best Stage | Risk Label |
|---|---|---|---|---|
| Monolithic Architecture | Beginner Friendly | High | MVP, Internal Tools | Can become Big Ball of Mud |
| Modular Monolith Architecture | Beginner Friendly | High | Early to Mid Stage | Needs module discipline |
| Layered Architecture | Beginner Friendly | High | Most Business Apps | Can become anemic layers |
| Clean Architecture | Intermediate | Medium | Long-lived Systems | Can be over-abstracted |
| Hexagonal Architecture | Intermediate | Medium | Systems with external dependencies | Too many ports if overused |
| DDD Architecture | Advanced | High in complex domains | Complex Business Domains | Directory-driven fake DDD |
| SOA | Intermediate | Medium | Enterprise Integration | ESB bottleneck |
| Microservices Architecture | Advanced | High | Multi-team Systems | Distributed Monolith |
| API Gateway Architecture | Intermediate | High | External API Systems | Business logic in gateway |
| BFF Architecture | Intermediate | Medium | Multi-client Products | BFF sprawl |
| Event-Driven Architecture | Advanced | High | Async cross-domain workflows | Event soup |
| CQRS Architecture | Advanced | Medium | Read/write model divergence | Overuse in CRUD |
| Event Sourcing Architecture | Advanced | Low to Medium | Audit-heavy systems | Event versioning complexity |
| Saga Architecture | Advanced | Medium | Cross-service transactions | Missing compensation |
| Service Mesh Architecture | Advanced | Medium | Large Kubernetes microservices | Too early adoption |
| Cloud Native Architecture | Intermediate | High | Scalable online services | Tool-first thinking |
| Kubernetes Architecture | Intermediate | High | Container platforms | Operational complexity |
| Serverless Architecture | Beginner to Intermediate | High | Event-based workloads | Hidden coupling |
| Cell-Based Architecture | Advanced | Medium | SaaS and large platforms | Expensive isolation |
| Multi-Region Active-Active | Advanced | Medium | Global critical systems | Consistency complexity |
| Edge Architecture | Intermediate | High | Global low-latency products | Cache correctness |
| Data Warehouse Architecture | Intermediate | High | Business analytics | Metric chaos |
| Data Lake Architecture | Intermediate | High | Raw data and AI datasets | Data swamp |
| Lakehouse Architecture | Advanced | Medium to High | Unified BI and AI | Table governance required |
| Streaming Architecture | Advanced | High | Real-time data products | State and replay complexity |
| Lambda Architecture | Advanced | Medium | Accuracy + low latency | Double logic maintenance |
| Kappa Architecture | Advanced | Medium | Log-centric systems | Replay capacity |
| CDC Architecture | Intermediate | High | Data sync and projections | Schema change risk |
| Data Mesh Architecture | Advanced | Medium | Large organizations | Org change required |
| ML Platform Architecture | Advanced | High in AI orgs | Multi-model AI teams | Platform cost |
| MLOps Architecture | Advanced | High in AI orgs | Production ML | Data version gaps |
| RAG Architecture | Intermediate | High | Knowledge QA | Retrieval quality |
| Agentic Architecture | Advanced | Emerging | Multi-step AI automation | Permission and audit risk |
| LLMOps Architecture | Intermediate to Advanced | Emerging | Production LLM apps | Eval gaps |
| Vector Database Architecture | Intermediate | High in AI apps | Semantic search and RAG | Embedding mismatch |
| DevOps Architecture | Beginner to Intermediate | High | All engineering teams | Tool-only adoption |
| GitOps Architecture | Intermediate | Medium to High | Kubernetes environments | Manual drift |
| Platform Engineering | Advanced | High in large orgs | Multi-team engineering | Platform as ticket system |
| Internal Developer Platform | Advanced | Medium to High | Large engineering orgs | Portal without self-service |
| Observability Architecture | Intermediate | High | Distributed systems | Logs-only monitoring |
| SRE Architecture | Advanced | High in reliability-focused orgs | Critical systems | Alert fatigue |
| Zero Trust Architecture | Advanced | High in enterprises | Security-sensitive systems | VPN rebrand |
| IAM Architecture | Intermediate | High | Most products | Role explosion |
| Policy as Code | Advanced | Medium | Regulated/cloud-native orgs | Policy without tests |
| Audit Architecture | Intermediate | High | Regulated systems | Debug logs as audit |
| Cache Architecture | Beginner to Intermediate | High | Read-heavy systems | Inconsistency |
| Sharding Architecture | Advanced | Medium | Huge data scale | Bad shard key |
| Read-Write Splitting | Intermediate | High | Read-heavy systems | Replica lag |
| Circuit Breaker and Bulkhead | Intermediate | High | Distributed systems | Retry storm |
| Rate Limiting and Degradation | Intermediate | High | High-traffic systems | Wrong priority |
| Micro Frontends | Advanced | Medium | Large frontend teams | Runtime complexity |
| Component-Based Architecture | Beginner Friendly | High | Most frontend apps | Poor component contracts |
| Design System Architecture | Intermediate | High in product orgs | Multi-product teams | Component library only |

## 3. Suggested Focus By Career Stage

| Stage | Focus |
|---|---|
| Junior Engineer | Layered, Modular Monolith, Component-Based, Cache, Basic CI/CD |
| Mid-Level Engineer | Clean, Hexagonal, DDD, API Gateway, Event-Driven, Observability |
| Senior Engineer | Microservices, CQRS, Saga, Kubernetes, Data Architecture, IAM |
| Staff / Architect | Platform Engineering, SRE, Data Mesh, Multi-Region, Zero Trust, Architecture Decision Records |
| AI Engineer | RAG, Vector Database, LLMOps, MLOps, Agentic Architecture |

## 4. Default Advice

Prefer boring architecture until reality demands complexity.

```text
Start simple.
Make boundaries explicit.
Observe real pressure.
Add architecture only when it removes real pain.
```

