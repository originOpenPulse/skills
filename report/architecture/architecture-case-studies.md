# Architecture Case Studies

> 目标：用业务场景串起架构。架构只有放到真实问题里，才知道为什么存在。

专题首页：[Big Tech Architecture Atlas](README.md)

## 1. E-Commerce Order System

### Problem

电商下单涉及用户、商品、库存、优惠券、支付、物流、通知和数据分析。核心难点是高并发、交易一致性、异步扩展和失败补偿。

### Recommended Architecture

- Domain-Driven Design Architecture
- Modular Monolith or Microservices Architecture
- Event-Driven Architecture
- Saga Architecture
- CQRS Architecture
- Cache Architecture
- Observability Architecture

### Core Boundaries

| Boundary | Responsibility |
|---|---|
| Order | 创建订单、订单状态机 |
| Inventory | 库存预留和释放 |
| Payment | 支付请求、支付回调 |
| Promotion | 优惠券和折扣 |
| Fulfillment | 发货和物流 |

### Key Flow

```text
Create Order
-> Reserve Inventory
-> Request Payment
-> Payment Callback
-> Confirm Order
-> Publish OrderPaid
-> Start Fulfillment
```

### Standard I/O

| Input | Output |
|---|---|
| CreateOrderCommand | OrderCreated Event |
| PaymentCallback | PaymentSucceeded / PaymentFailed Event |
| InventoryReserveCommand | InventoryReserved / InventoryReserveFailed |

### Risks

- 支付成功但订单状态未更新。
- 库存预留成功但支付失败后未释放。
- 大促流量击穿库存和订单服务。
- 事件重复消费导致状态错乱。

### Review Focus

- Order Aggregate 是否控制订单状态机？
- Saga 补偿是否完整？
- 事件是否幂等？
- 核心链路是否有降级和限流？

## 2. Enterprise Knowledge Base RAG

### Problem

企业希望 AI 回答内部文档、项目报告、规范制度和产品资料。难点是知识更新、权限控制、答案准确性和可追溯。

### Recommended Architecture

- RAG Architecture
- Vector Database Architecture
- LLMOps Architecture
- IAM Architecture
- Audit Architecture

### Core Boundaries

| Boundary | Responsibility |
|---|---|
| Document Ingestion | 文档采集、清洗、切分 |
| Retrieval | 语义检索、关键词检索、重排 |
| Generation | 基于上下文生成回答 |
| Permission | 文档级权限过滤 |
| Evaluation | 回答质量、引用质量和幻觉检测 |

### Key Flow

```text
User Question
-> Permission Context
-> Retrieve Relevant Chunks
-> Rerank
-> Assemble Context
-> LLM Generate
-> Return Answer with Sources
```

### Standard I/O

| Input | Output |
|---|---|
| Document | Chunk + Embedding + Metadata |
| User Question | Retrieved Context |
| Question + Context | Grounded Answer + Sources |

### Risks

- 检索召回错误导致答案错误。
- 文档权限过滤缺失导致数据泄露。
- Prompt 修改后质量下降但无人发现。
- 没有 Trace，无法定位回答为什么错。

### Review Focus

- RAG 是否只靠向量检索，还是有混合检索和重排？
- 权限过滤是在检索前、检索中还是检索后？
- 是否有评测集？
- 是否记录 Prompt、上下文和模型输出？

## 3. SaaS Multi-Tenant Platform

### Problem

SaaS 系统要服务多个客户，每个客户有不同用户、角色、配置、数据和合规要求。难点是租户隔离、权限、配置扩展和稳定性。

### Recommended Architecture

- Modular Monolith or Microservices Architecture
- Multi-Tenant Architecture
- IAM Architecture
- Cell-Based Architecture
- Audit Architecture
- Platform Engineering Architecture

### Core Boundaries

| Boundary | Responsibility |
|---|---|
| Tenant | 租户信息、套餐、配置 |
| Identity | 用户、组织、角色、权限 |
| Business Modules | CRM、Order、Billing 等业务能力 |
| Audit | 敏感操作记录 |
| Cell | 按租户隔离运行单元 |

### Key Flow

```text
Request
-> Resolve Tenant
-> Authenticate User
-> Authorize Permission
-> Route to Tenant Data
-> Execute Business Use Case
-> Write Audit Event
```

### Standard I/O

| Input | Output |
|---|---|
| Tenant Request | Tenant Context |
| User Token | Identity Claims |
| Business Command | Tenant-Scoped Result |

### Risks

- 跨租户数据泄露。
- 权限逻辑散落在业务代码里。
- 大客户流量影响小客户。
- 定制化破坏产品主线。

### Review Focus

- 每个查询是否带 Tenant Scope？
- 权限是集中策略还是分散 if？
- 是否有租户级审计？
- 是否需要 Cell 隔离大客户？

## 4. Payment and Ledger System

### Problem

支付和账务系统要求准确性、可审计、可追溯和高可靠。难点是资金状态一致性、重复回调、补偿、对账和审计。

### Recommended Architecture

- Clean Architecture
- Domain-Driven Design Architecture
- Event Sourcing Architecture
- CQRS Architecture
- Saga Architecture
- Audit Architecture
- Zero Trust Architecture

### Core Boundaries

| Boundary | Responsibility |
|---|---|
| Payment | 支付请求和支付状态 |
| Ledger | 账户记账和账务流水 |
| Reconciliation | 对账 |
| Risk | 风控 |
| Audit | 审计证据链 |

### Key Flow

```text
Payment Request
-> Create Payment Intent
-> Call Payment Provider
-> Receive Callback
-> Verify Signature
-> Append Ledger Event
-> Update Read Model
-> Reconciliation
```

### Standard I/O

| Input | Output |
|---|---|
| PaymentCommand | PaymentIntentCreated |
| ProviderCallback | PaymentSucceeded / PaymentFailed |
| LedgerEvent | Account Balance Projection |

### Risks

- 重复回调导致重复记账。
- 当前余额表被当作唯一事实来源。
- 缺少审计证据，问题无法追责。
- 跨服务事务没有补偿。

### Review Focus

- 是否所有资金变更都有不可变事件？
- 是否有幂等键？
- 是否有对账任务？
- 是否有完整审计链？

## 5. Content Recommendation Platform

### Problem

内容平台需要根据用户行为实时推荐内容，同时处理内容审核、画像、召回、排序、实验和反馈。难点是实时数据、模型迭代、低延迟和质量评估。

### Recommended Architecture

- Streaming Architecture
- Data Lake / Lakehouse Architecture
- Machine Learning Platform Architecture
- MLOps Architecture
- Feature Store Architecture
- Observability Architecture
- A/B Testing Platform

### Core Boundaries

| Boundary | Responsibility |
|---|---|
| Event Tracking | 用户行为采集 |
| Feature Engineering | 实时和离线特征 |
| Recall | 候选内容召回 |
| Ranking | 排序模型 |
| Experiment | A/B 测试 |
| Feedback | 点击、停留、转化反馈 |

### Key Flow

```text
User Event
-> Stream Processing
-> Update Real-Time Features
-> Recall Candidates
-> Rank Candidates
-> Return Feed
-> Collect Feedback
-> Train Next Model
```

### Standard I/O

| Input | Output |
|---|---|
| UserBehaviorEvent | Real-Time Feature |
| User Context | Candidate List |
| Candidate List + Features | Ranked Feed |
| Feedback Event | Training Data |

### Risks

- 实时和离线特征不一致。
- 模型上线没有灰度和回滚。
- 推荐指标只看点击，不看长期体验。
- 数据延迟导致画像过旧。

### Review Focus

- 是否区分在线特征和离线特征？
- 是否有模型版本和实验记录？
- 是否能追踪一次推荐的特征和模型？
- 是否有实时数据质量监控？

