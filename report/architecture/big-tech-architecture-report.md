# Big Tech Architecture Report

> 目标：系统梳理当前大厂常用的软件、云原生、数据、AI、工程效能与安全架构，说明它们为什么存在，解决了什么痛点，以及适合什么业务需求。

专题首页：[Big Tech Architecture Atlas](README.md)

## 1. 一句话结论

大厂并不是只用一种“高级架构”，而是把多种架构按业务阶段、团队规模、流量规模、数据规模、合规要求组合起来使用。

常见组合是：

- 早期业务：Modular Monolith + Layered Architecture + Clean Architecture。
- 中大型业务：Microservices + DDD + API Gateway + Event-Driven Architecture + CQRS。
- 高并发业务：Cloud Native + Kubernetes + Service Mesh + Cache + Sharding + Observability。
- 数据业务：Data Warehouse + Data Lake/Lakehouse + Streaming Architecture + CDC + Data Mesh。
- AI 业务：RAG Architecture + Agentic Architecture + MLOps/LLMOps + Feature Store + Vector Database。
- 平台型公司：Platform Engineering + Internal Developer Platform + GitOps + SRE。
- 安全合规业务：Zero Trust Architecture + IAM + Policy as Code + Audit Architecture。

## 2. 大厂为什么越来越重视 Architecture

架构的本质不是“画图好看”，而是把复杂系统拆成可理解、可演进、可治理、可扩展、可恢复的结构。

它解决的核心问题：

- 业务增长：用户量、订单量、数据量、模型调用量持续增长。
- 团队增长：几十人变成几百人、几千人后，代码冲突和协作成本急剧上升。
- 复杂度增长：一个功能会跨前端、后端、数据、算法、安全、运维、支付、风控等多个系统。
- 稳定性要求：大厂不能靠“重启试试”，要可观测、可降级、可恢复。
- 成本压力：云资源、数据库、存储、模型推理成本都需要架构层面控制。
- 合规与安全：权限、审计、隔离、数据保护不能后补。

## 3. 架构全景分类

| 分类 | 英文名 | 主要解决什么 |
|---|---|---|
| 应用架构 | Application Architecture | 代码如何分层、模块如何依赖、业务逻辑放哪里 |
| 服务架构 | Service Architecture | 多个服务如何拆分、通信、治理 |
| 云原生架构 | Cloud Native Architecture | 如何弹性扩缩容、容器化、自动化交付 |
| 数据架构 | Data Architecture | 数据如何采集、存储、治理、分析 |
| AI 架构 | AI Architecture | 模型、知识库、推理、评测、Agent 如何协作 |
| 工程效能架构 | Engineering Productivity Architecture | 如何让大团队高质量、高频率交付 |
| 安全架构 | Security Architecture | 身份、权限、审计、隔离、合规如何落地 |
| 韧性架构 | Resilience Architecture | 故障隔离、容灾、限流、降级、恢复 |

## 4. Application Architecture

### 4.1 Monolithic Architecture

中文：单体架构。

存在意义：用一个应用承载主要业务逻辑，部署简单，开发起步快。

解决痛点：

- 早期业务不确定，不适合过早拆分服务。
- 小团队沟通成本低，单体更容易快速迭代。
- 本地开发、调试、测试、部署链路短。

典型需求：

- MVP、内部系统、管理后台、初创产品。
- 业务边界尚未稳定的系统。

大厂使用方式：

- 大厂也会在内部工具、后台系统、低复杂度业务中保留单体。
- 现代实践通常不是传统“大泥球”，而是 Modular Monolith。

风险：

- 代码边界不清会演变成 Big Ball of Mud。
- 团队变大后，发布频率互相牵制。

### 4.2 Modular Monolith Architecture

中文：模块化单体架构。

存在意义：在一个应用内保持清晰模块边界，兼顾单体部署简单和微服务边界清晰。

解决痛点：

- 避免一开始就上 Microservices 带来的运维复杂度。
- 保持领域边界，为未来服务拆分做准备。
- 降低跨服务调用、分布式事务、链路追踪的复杂度。

典型需求：

- 中小团队但业务较复杂。
- 想做长期架构治理，但还没到必须拆服务的阶段。

常见设计：

- 按业务能力拆模块，例如 user、order、payment、inventory。
- 模块之间通过接口或应用服务调用，禁止随意访问彼此内部表和类。

### 4.3 Layered Architecture

中文：分层架构。

存在意义：把系统拆成表现层、应用层、领域层、基础设施层等职责不同的层。

解决痛点：

- Controller 写满业务逻辑，代码难测、难改。
- SQL、缓存、消息、HTTP 调用和业务规则混在一起。
- 新人不知道代码应该放在哪里。

典型需求：

- Web 应用、企业后台、Java/Spring、.NET、Node 服务。

常见层次：

- Presentation Layer：接口、页面、RPC 入口。
- Application Layer：用例编排、事务边界。
- Domain Layer：核心业务规则。
- Infrastructure Layer：数据库、消息队列、缓存、第三方 API。

### 4.4 Clean Architecture

中文：整洁架构。

存在意义：让核心业务逻辑不依赖框架、数据库、UI、外部服务。

解决痛点：

- 框架升级或数据库替换会牵动业务逻辑。
- 业务规则无法独立测试。
- 代码依赖方向混乱。

典型需求：

- 长生命周期系统。
- 金融、交易、风控、保险、医疗等业务规则复杂领域。

关键原则：

- Dependency Rule：依赖指向业务核心。
- 外部细节通过接口适配。

### 4.5 Hexagonal Architecture

中文：六边形架构，也叫 Ports and Adapters Architecture。

存在意义：用端口和适配器隔离业务核心和外部世界。

解决痛点：

- 同一业务能力要同时支持 REST、RPC、消息消费、CLI、定时任务。
- 数据库、消息队列、第三方服务变化频繁。
- 测试时不想依赖真实外部系统。

典型需求：

- 需要多入口、多出口的后端服务。
- 对可测试性和可替换性要求高的系统。

### 4.6 Domain-Driven Design Architecture

中文：领域驱动设计架构，简称 DDD。

存在意义：用业务语言和领域模型组织复杂业务。

解决痛点：

- 技术分层清楚，但业务概念混乱。
- 一个“订单”在销售、履约、财务、售后里含义不同。
- 服务拆分只按数据库表拆，导致边界错误。

典型需求：

- 电商、支付、物流、网约车、内容平台、广告、风控、CRM、ERP。

核心概念：

- Ubiquitous Language：统一语言。
- Bounded Context：限界上下文。
- Aggregate：聚合。
- Entity / Value Object：实体和值对象。
- Domain Service / Repository：领域服务和仓储。

大厂价值：

- DDD 常被用作 Microservices 拆分依据。
- Uber 公开讨论过 Domain-Oriented Microservice Architecture，用领域边界治理微服务复杂度。

## 5. Service Architecture

### 5.1 Service-Oriented Architecture

中文：面向服务架构，简称 SOA。

存在意义：把企业能力封装成服务，支持跨系统复用。

解决痛点：

- 企业内部系统烟囱林立，重复建设。
- ERP、CRM、供应链、财务、风控之间需要集成。

典型需求：

- 传统企业信息化。
- 银行、运营商、保险、制造业大型系统。

和 Microservices 的区别：

- SOA 常见中心化 ESB。
- Microservices 更强调小服务、去中心化治理、独立部署。

### 5.2 Microservices Architecture

中文：微服务架构。

存在意义：把大型系统拆成围绕业务能力的、可独立开发和部署的小服务。

解决痛点：

- 单体发布互相阻塞。
- 不同团队需要独立迭代。
- 某些业务模块需要独立扩容。
- 故障需要局部隔离。

典型需求：

- 大型互联网业务。
- 多团队并行开发。
- 高流量系统、平台型系统。

大厂场景：

- Netflix 是微服务实践的代表之一。
- Amazon、Uber、字节、阿里、腾讯、美团等都有大量微服务化系统。

主要代价：

- 分布式事务复杂。
- 服务治理、链路追踪、配置管理、灰度发布要求高。
- 本地调试和集成测试成本上升。

### 5.3 API Gateway Architecture

中文：API 网关架构。

存在意义：在客户端和后端服务之间提供统一入口。

解决痛点：

- 前端直接调用几十个服务，鉴权、限流、日志重复实现。
- 服务地址、协议、版本暴露给客户端。
- 多端请求聚合困难。

典型能力：

- 路由、鉴权、限流、熔断、协议转换、灰度、审计、聚合。

典型需求：

- App、小程序、Web、多租户 SaaS。
- 微服务系统对外暴露 API。

### 5.4 Backend for Frontend Architecture

中文：BFF 架构。

存在意义：为不同前端体验提供专属后端聚合层。

解决痛点：

- Web、iOS、Android、IoT 对数据形态需求不同。
- 通用 API 过于粗糙或过于复杂。
- 前端为了一个页面调用大量接口。

典型需求：

- 多端产品。
- 页面聚合复杂、交互频繁的业务。

常见组合：

- Client -> BFF -> API Gateway -> Microservices。

### 5.5 Event-Driven Architecture

中文：事件驱动架构，简称 EDA。

存在意义：通过事件解耦系统，让一个业务动作可以异步触发多个后续流程。

解决痛点：

- 下单后要通知库存、支付、积分、消息、风控、数据分析。
- 同步调用链太长，响应慢且容易级联失败。
- 新业务订阅旧事件，不想改原系统。

典型需求：

- 电商订单、物流状态、支付通知、用户行为、IoT、实时风控。

核心组件：

- Event Producer：事件生产者。
- Event Broker：Kafka、Pulsar、SNS/SQS、EventBridge 等。
- Event Consumer：事件消费者。
- Event Schema：事件契约。

大厂价值：

- Amazon、Netflix、Uber、LinkedIn、阿里、美团等都有大量事件驱动系统。

### 5.6 CQRS Architecture

中文：命令查询职责分离架构。

存在意义：把写模型和读模型分离。

解决痛点：

- 写入需要强一致和复杂校验，读取需要高性能和灵活查询。
- 一个数据库模型同时服务交易写入和报表查询，互相拖累。
- 高并发读场景下，写库压力过大。

典型需求：

- 订单、账户、库存、报表、搜索、推荐、交易系统。

常见组合：

- 写库负责交易一致性。
- 通过事件或 CDC 同步到读库、缓存、搜索引擎、OLAP。

### 5.7 Event Sourcing Architecture

中文：事件溯源架构。

存在意义：把状态变化作为事件序列保存，通过重放事件得到当前状态。

解决痛点：

- 需要完整审计历史。
- 需要知道状态为什么变成现在这样。
- 需要支持回放、补偿、重建投影。

典型需求：

- 金融账务、审计、订单状态机、协作编辑、风控。

代价：

- 事件版本治理复杂。
- 查询通常需要配合 CQRS。

### 5.8 Saga Architecture

中文：Saga 分布式事务架构。

存在意义：用一系列本地事务和补偿动作处理跨服务业务流程。

解决痛点：

- 微服务之间无法轻易使用传统数据库事务。
- 下单、扣库存、支付、发货等流程跨多个服务。

典型需求：

- 电商交易链路。
- 支付、履约、库存、物流流程。

两种模式：

- Choreography：服务之间通过事件协作。
- Orchestration：由流程编排器统一推进。

### 5.9 Service Mesh Architecture

中文：服务网格架构。

存在意义：把服务间通信治理从业务代码中下沉到基础设施层。

解决痛点：

- 每个服务都要自己处理重试、超时、熔断、mTLS、流量切分。
- 多语言服务治理不一致。
- 微服务数量多后，通信安全和可观测性难统一。

典型能力：

- 服务发现、mTLS、流量治理、重试、熔断、灰度、可观测性。

典型需求：

- 大规模 Kubernetes 微服务集群。
- 多语言、多团队服务治理。

代表：

- Istio、Linkerd、Consul。

## 6. Cloud Native Architecture

### 6.1 Cloud Native Architecture

中文：云原生架构。

存在意义：利用容器、服务网格、不可变基础设施、声明式 API，让系统具备弹性、韧性、自动化交付能力。

解决痛点：

- 手工部署不稳定。
- 环境差异导致“本地能跑，线上不行”。
- 扩缩容、故障恢复、滚动发布成本高。

典型需求：

- 大规模互联网服务。
- 云上系统、多集群系统、全球部署系统。

常见组合：

- Docker + Kubernetes + Helm/Kustomize + GitOps + Observability。

### 6.2 Kubernetes Architecture

中文：Kubernetes 容器编排架构。

存在意义：统一管理容器部署、调度、扩缩容、服务发现、健康检查。

解决痛点：

- 服务实例数量太多，人工管理不可行。
- 机器故障后需要自动拉起。
- 发布、回滚、扩容需要标准化。

典型需求：

- 微服务平台。
- 多云、混合云、私有云。

大厂价值：

- Kubernetes 源自 Google 多年大规模容器编排经验。
- 现在是云原生事实标准之一。

### 6.3 Serverless Architecture

中文：无服务器架构。

存在意义：开发者只关注函数或业务逻辑，不直接管理服务器。

解决痛点：

- 峰谷流量明显，长期预留机器浪费。
- 小功能、事件处理、定时任务不值得维护完整服务。
- 希望按调用付费、自动扩缩容。

典型需求：

- 图片处理、Webhook、数据清洗、定时任务、异步事件消费。

常见产品：

- AWS Lambda、Azure Functions、Google Cloud Functions、Cloud Run。

### 6.4 Cell-Based Architecture

中文：单元化架构。

存在意义：把大系统拆成多个相对独立的 Cell，每个 Cell 服务一部分用户或租户。

解决痛点：

- 单个大集群故障影响全站。
- 超大规模系统扩容边界不清。
- 多租户之间需要隔离。

典型需求：

- SaaS 平台、支付、云服务、全球化业务。

价值：

- 故障爆炸半径更小。
- 可以按 Cell 独立扩容、发布、迁移。

### 6.5 Multi-Region Active-Active Architecture

中文：多地域双活/多活架构。

存在意义：让系统在多个地域同时提供服务。

解决痛点：

- 单地域故障导致业务不可用。
- 全球用户访问延迟高。
- 灾备切换时间长。

典型需求：

- 金融支付、全球 SaaS、游戏、内容平台、云服务。

关键难点：

- 数据一致性。
- 流量调度。
- 跨地域延迟。
- 灾备演练。

### 6.6 Edge Architecture

中文：边缘架构。

存在意义：把计算、缓存、安全能力前移到离用户更近的位置。

解决痛点：

- 全球用户访问中心机房延迟高。
- 静态资源、视频、API 都打到源站，成本和压力高。
- IoT 和实时交互需要低延迟。

典型需求：

- CDN、直播、短视频、游戏、IoT、全球 Web 应用。

常见能力：

- Edge Cache、Edge Function、WAF、Bot 防护、图片转码。

## 7. Data Architecture

### 7.1 Data Warehouse Architecture

中文：数据仓库架构。

存在意义：把企业数据清洗、建模后用于 BI、报表、经营分析。

解决痛点：

- 业务库只适合交易，不适合复杂分析。
- 各部门数据口径不一致。
- 管理层需要稳定指标。

典型需求：

- 财务报表、经营分析、销售分析、用户增长分析。

代表产品：

- BigQuery、Snowflake、Redshift、ClickHouse、Hive。

### 7.2 Data Lake Architecture

中文：数据湖架构。

存在意义：低成本存储原始结构化、半结构化、非结构化数据。

解决痛点：

- 数据格式多样，提前建模困难。
- 日志、图片、文本、音频、行为数据量巨大。
- 机器学习需要保留原始数据。

典型需求：

- 用户行为日志、AI 训练数据、IoT 数据、内容数据。

风险：

- 缺少治理会变成 Data Swamp。

### 7.3 Lakehouse Architecture

中文：湖仓一体架构。

存在意义：融合 Data Lake 的低成本和 Data Warehouse 的事务、治理、查询能力。

解决痛点：

- 数据湖便宜但治理弱。
- 数据仓库强但成本高、数据类型受限。
- 同一份数据要同时服务 BI、AI、实时分析。

典型需求：

- 统一数据平台。
- AI + BI 一体化分析。

代表技术：

- Delta Lake、Apache Iceberg、Apache Hudi。

### 7.4 Streaming Architecture

中文：流式架构。

存在意义：让数据在产生后秒级或毫秒级被处理。

解决痛点：

- T+1 离线报表无法满足实时决策。
- 风控、推荐、监控需要实时数据。
- 用户行为需要实时触发营销、推荐、告警。

典型需求：

- 实时风控、实时推荐、实时数仓、监控告警、IoT。

代表技术：

- Kafka、Flink、Spark Streaming、Pulsar。

### 7.5 Lambda Architecture

中文：Lambda 数据架构。

存在意义：同时使用批处理和流处理，兼顾准确性和实时性。

解决痛点：

- 实时数据快但可能不完整。
- 批处理慢但结果更准确。

典型需求：

- 早期实时数仓。
- 对准确性和实时性都敏感的业务。

代价：

- 同一逻辑要维护批处理和流处理两套代码。

### 7.6 Kappa Architecture

中文：Kappa 数据架构。

存在意义：只用流处理处理实时和历史数据，通过重放日志修正结果。

解决痛点：

- Lambda 架构维护两套逻辑太复杂。
- Kafka 等日志系统可以长期保存并重放数据。

典型需求：

- 事件日志完整、业务可通过重放修复的场景。

### 7.7 Change Data Capture Architecture

中文：变更数据捕获架构，简称 CDC。

存在意义：捕获数据库变更并同步到消息队列、搜索、缓存、数仓。

解决痛点：

- 业务库和搜索、分析系统之间数据同步困难。
- 定时全量同步成本高、延迟高。
- 业务代码里手写同步逻辑容易漏。

典型需求：

- MySQL -> Kafka -> Elasticsearch / ClickHouse / Data Lake。

代表技术：

- Debezium、Canal、Flink CDC。

### 7.8 Data Mesh Architecture

中文：数据网格架构。

存在意义：把数据当作产品，由领域团队负责数据质量、契约和治理。

解决痛点：

- 中心化数据团队成为瓶颈。
- 业务团队不对数据质量负责。
- 数据口径、血缘、权限治理困难。

典型需求：

- 多事业部、多业务线的大型组织。

核心理念：

- Domain-oriented ownership。
- Data as a product。
- Federated governance。
- Self-serve data platform。

## 8. AI Architecture

### 8.1 Machine Learning Platform Architecture

中文：机器学习平台架构。

存在意义：把数据、特征、训练、评估、部署、监控串成标准化平台。

解决痛点：

- 算法工程师重复搭训练和发布流程。
- 模型从 Notebook 到生产环境困难。
- 模型效果、数据漂移、线上性能难追踪。

典型需求：

- 推荐、搜索排序、广告、风控、预测、智能客服。

常见组件：

- Feature Store、Model Registry、Training Pipeline、Serving、Monitoring。

### 8.2 MLOps Architecture

中文：机器学习运维架构。

存在意义：用工程化方式管理模型生命周期。

解决痛点：

- 模型上线不可重复。
- 数据版本、模型版本、实验结果无法追踪。
- 模型漂移后没有告警和回滚机制。

典型需求：

- 企业级 AI 平台。
- 多模型、多团队、多环境协作。

### 8.3 RAG Architecture

中文：检索增强生成架构，简称 RAG。

存在意义：让大模型回答时先检索企业知识或外部资料，再基于上下文生成答案。

解决痛点：

- 大模型不知道企业私有知识。
- 模型幻觉严重。
- 重新训练模型成本高。
- 知识更新频繁。

典型需求：

- 企业知识库、智能客服、代码助手、合同问答、投研分析。

核心链路：

- 文档采集 -> 清洗切分 -> Embedding -> Vector Database -> Retrieval -> Rerank -> LLM Generation -> Evaluation。

### 8.4 Agentic Architecture

中文：智能体架构。

存在意义：让 LLM 不只回答问题，还能规划任务、调用工具、执行动作、观察结果并迭代。

解决痛点：

- 复杂任务不是一次问答能完成。
- 需要跨系统操作，例如查资料、写代码、调用 API、发消息、生成报告。
- 用户希望 AI 能承担多步骤工作流。

典型需求：

- 编程 Agent、数据分析 Agent、运维 Agent、销售助手、办公自动化。

核心组件：

- Planner、Tool Calling、Memory、Retriever、Executor、Guardrails、Evaluator。

风险：

- 权限边界。
- 工具误用。
- 长链路错误累积。
- 成本和延迟不可控。

### 8.5 LLMOps Architecture

中文：大模型运维架构。

存在意义：管理提示词、模型版本、评测集、上下文、成本、安全和上线流程。

解决痛点：

- Prompt 改了但效果不可追踪。
- 不同模型输出差异大。
- 线上质量、延迟、成本缺少监控。
- 敏感信息可能被泄露。

典型需求：

- 企业级大模型应用。
- 多模型路由、灰度、A/B 测试。

### 8.6 Vector Database Architecture

中文：向量数据库架构。

存在意义：支持语义检索、相似度搜索和 RAG 上下文召回。

解决痛点：

- 传统关键词搜索无法理解语义。
- 非结构化内容难以按含义查找。
- LLM 需要外部知识召回。

典型需求：

- 知识库、推荐、图片检索、代码搜索、问答系统。

代表：

- Milvus、Pinecone、Weaviate、pgvector、Elasticsearch vector search。

## 9. Engineering Productivity Architecture

### 9.1 DevOps Architecture

中文：开发运维一体化架构。

存在意义：打通开发、测试、部署、运维反馈链路。

解决痛点：

- 开发和运维割裂，交付慢。
- 手工发布容易出错。
- 线上问题反馈不到研发流程。

典型需求：

- CI/CD、自动化测试、持续交付、监控反馈。

### 9.2 GitOps Architecture

中文：GitOps 架构。

存在意义：用 Git 作为基础设施和应用部署的声明式事实来源。

解决痛点：

- 环境变更不可追踪。
- 人工 kubectl 改线上配置导致漂移。
- 回滚和审计困难。

典型需求：

- Kubernetes 集群交付。
- 多环境、多集群配置管理。

代表：

- Argo CD、Flux。

### 9.3 Platform Engineering Architecture

中文：平台工程架构。

存在意义：为研发团队提供自助式平台能力，让业务团队更快、更安全地交付。

解决痛点：

- 每个团队重复搭 CI/CD、监控、权限、模板。
- 基础设施复杂度暴露给业务研发。
- 运维团队成为交付瓶颈。

典型需求：

- 大型研发组织。
- 多团队、多服务、多环境。

核心产物：

- Internal Developer Platform。
- Service Template。
- Golden Path。
- Developer Portal。

### 9.4 Internal Developer Platform Architecture

中文：内部开发者平台架构，简称 IDP。

存在意义：把创建服务、申请资源、查看监控、发布应用、查文档等研发动作统一到一个平台。

解决痛点：

- 新服务创建慢。
- 文档分散，服务归属不清。
- 研发需要在多个平台之间切换。

典型需求：

- 微服务数量多的大公司。

代表：

- Backstage 生态、企业自研研发平台。

### 9.5 Observability Architecture

中文：可观测性架构。

存在意义：通过指标、日志、链路追踪、事件和 Profiling 理解系统运行状态。

解决痛点：

- 线上问题只能靠猜。
- 微服务调用链太长，定位慢。
- 不知道用户体验和系统指标之间的关系。

典型需求：

- 微服务、云原生、高并发系统。

核心信号：

- Metrics、Logs、Traces、Events、Profiles。

### 9.6 SRE Architecture

中文：站点可靠性工程架构。

存在意义：用软件工程方法管理可靠性，平衡稳定性和迭代速度。

解决痛点：

- 系统稳定性靠人工值班。
- 业务想快速迭代，运维想减少变更，目标冲突。
- 故障复盘和可靠性指标不体系化。

核心概念：

- SLI、SLO、Error Budget、Toil Reduction、Incident Response。

大厂价值：

- Google SRE 是该领域标志性实践。

## 10. Security Architecture

### 10.1 Zero Trust Architecture

中文：零信任架构。

存在意义：默认不信任任何网络位置、设备或用户，每次访问都要验证和授权。

解决痛点：

- 传统内网边界被云、远程办公、移动设备打破。
- 一旦进入内网，攻击者可横向移动。
- 多云、多 SaaS 权限治理复杂。

典型需求：

- 大型企业、金融、云服务、远程办公、多租户平台。

核心原则：

- Verify explicitly。
- Least privilege。
- Assume breach。

### 10.2 IAM Architecture

中文：身份与访问管理架构。

存在意义：统一管理用户、服务、角色、权限、凭证和访问策略。

解决痛点：

- 权限散落在各系统，难审计。
- 离职、转岗、外包账号管理风险高。
- 服务之间调用缺少身份边界。

典型需求：

- 企业后台、云平台、SaaS、多租户系统。

常见能力：

- SSO、RBAC、ABAC、OAuth2、OIDC、MFA、Service Account。

### 10.3 Policy as Code Architecture

中文：策略即代码架构。

存在意义：把安全、合规、权限、部署规则写成可测试、可审计的代码。

解决痛点：

- 规则靠人工审批，慢且不稳定。
- 合规规则难复用、难追踪。
- 多团队执行标准不一致。

典型需求：

- 云资源治理、Kubernetes admission control、CI/CD 安全门禁。

代表：

- Open Policy Agent、Kyverno、Sentinel。

### 10.4 Audit Architecture

中文：审计架构。

存在意义：记录关键操作、权限变更、数据访问和系统行为，支持追责和合规。

解决痛点：

- 出问题后不知道谁做了什么。
- 敏感数据访问不可追踪。
- 合规检查缺少证据链。

典型需求：

- 金融、医疗、政企、支付、云平台。

## 11. High Performance and Resilience Architecture

### 11.1 Cache Architecture

中文：缓存架构。

存在意义：用更快的存储承载热点读请求，降低数据库压力。

解决痛点：

- 数据库无法承受高并发读。
- 页面、接口、推荐结果重复计算。
- 跨地域访问延迟高。

典型需求：

- 首页、商品详情、用户会话、配置、排行榜、热点内容。

常见层次：

- Browser Cache、CDN Cache、Gateway Cache、Application Cache、Redis Cache、Local Cache。

### 11.2 Sharding Architecture

中文：分库分表架构。

存在意义：把数据按规则拆到多个数据库或表中。

解决痛点：

- 单库容量和 QPS 到达瓶颈。
- 大表索引维护和查询变慢。
- 数据库横向扩展困难。

典型需求：

- 用户、订单、交易、消息、日志等超大表。

关键难点：

- 分片键选择。
- 跨分片查询。
- 分布式事务。
- 扩容和迁移。

### 11.3 Read-Write Splitting Architecture

中文：读写分离架构。

存在意义：主库负责写，从库承担读，提高读吞吐。

解决痛点：

- 读请求远多于写请求。
- 报表和查询拖慢交易写入。

典型需求：

- 电商、内容平台、社交、企业后台。

风险：

- 主从延迟导致读到旧数据。

### 11.4 Circuit Breaker and Bulkhead Architecture

中文：熔断与舱壁隔离架构。

存在意义：当依赖异常时快速失败或隔离资源，避免级联故障。

解决痛点：

- 一个下游服务慢，拖垮整个调用链。
- 线程池、连接池被故障流量耗尽。
- 局部故障扩散成全站故障。

典型需求：

- 微服务、高并发、第三方依赖多的系统。

### 11.5 Rate Limiting and Degradation Architecture

中文：限流与降级架构。

存在意义：在突发流量或部分依赖不可用时保护核心链路。

解决痛点：

- 秒杀、热点事件、攻击流量导致系统崩溃。
- 非核心功能影响核心交易。

典型需求：

- 电商大促、直播、支付、抢票、社交热点。

常见策略：

- Token Bucket、Leaky Bucket、滑动窗口、热点 Key 保护、开关降级。

## 12. Frontend and Client Architecture

### 12.1 Micro Frontends Architecture

中文：微前端架构。

存在意义：把大型前端应用拆成多个可独立开发、部署、演进的子应用。

解决痛点：

- 大型后台或平台前端代码膨胀。
- 多团队在同一个前端仓库冲突严重。
- 技术栈升级困难。

典型需求：

- 企业中后台、云控制台、超级 App、SaaS 平台。

风险：

- 运行时集成复杂。
- 样式隔离、状态共享、性能治理要求高。

### 12.2 Component-Based Architecture

中文：组件化架构。

存在意义：用可复用组件构建 UI，提高一致性和开发效率。

解决痛点：

- 页面重复开发。
- UI 风格不一致。
- 改一个交互要改很多地方。

典型需求：

- Design System、前端工程化、跨业务线复用。

### 12.3 Design System Architecture

中文：设计系统架构。

存在意义：统一设计语言、组件、交互规范和工程实现。

解决痛点：

- 产品体验不一致。
- 设计稿和代码实现脱节。
- 前端重复造组件。

典型需求：

- 大型产品矩阵、多端产品、多业务线协作。

## 13. Typical Big Tech Architecture Combinations

### 13.1 电商平台

常见组合：

- DDD + Microservices + API Gateway + Event-Driven Architecture。
- CQRS + Cache + Sharding + Saga。
- Streaming Architecture + Data Warehouse + Recommendation Architecture。

解决痛点：

- 商品、订单、库存、支付、履约、售后边界复杂。
- 大促高并发。
- 交易一致性和用户体验都重要。

### 13.2 短视频/内容平台

常见组合：

- Microservices + CDN/Edge + Recommendation Architecture。
- Streaming Architecture + Data Lake + Feature Store。
- Observability + SRE + A/B Testing Platform。

解决痛点：

- 视频分发成本高。
- 推荐需要实时反馈。
- 内容审核和风控链路复杂。

### 13.3 云服务平台

常见组合：

- Kubernetes + Service Mesh + Multi-Tenant Architecture。
- IAM + Zero Trust + Audit Architecture。
- Platform Engineering + GitOps + Observability。

解决痛点：

- 多租户隔离。
- 资源编排和计量计费。
- 高可靠和安全合规。

### 13.4 金融支付系统

常见组合：

- Clean Architecture + DDD + Event Sourcing + CQRS。
- Saga + Audit Architecture + Zero Trust。
- Multi-Region Disaster Recovery + Observability。

解决痛点：

- 账务准确性。
- 审计和合规。
- 高可用和强风控。

### 13.5 企业 SaaS

常见组合：

- Modular Monolith 或 Microservices。
- Multi-Tenant Architecture + IAM + RBAC/ABAC。
- Data Warehouse + Observability + Platform Engineering。

解决痛点：

- 多租户隔离。
- 权限复杂。
- 定制化和标准化之间的平衡。

### 13.6 AI 应用平台

常见组合：

- RAG Architecture + Agentic Architecture + LLMOps。
- Vector Database + Feature Store + Evaluation Pipeline。
- API Gateway + Rate Limiting + Audit Architecture。

解决痛点：

- 企业知识接入。
- 模型效果评估。
- 成本、权限、安全和可追踪。

## 14. 如何判断该用哪种架构

| 你的问题 | 优先考虑的 Architecture |
|---|---|
| 团队小，业务还在探索 | Modular Monolith Architecture |
| 代码职责混乱 | Layered / Clean / Hexagonal Architecture |
| 业务规则复杂 | Domain-Driven Design Architecture |
| 多团队互相阻塞 | Microservices Architecture |
| 同步链路太长 | Event-Driven Architecture |
| 读写压力差异大 | CQRS Architecture |
| 跨服务事务复杂 | Saga Architecture |
| 高并发读压力大 | Cache + Read-Write Splitting Architecture |
| 单库扛不住 | Sharding Architecture |
| 服务治理复杂 | Service Mesh Architecture |
| 部署和扩容困难 | Cloud Native + Kubernetes Architecture |
| 流量波峰波谷明显 | Serverless Architecture |
| 全球访问延迟高 | Edge + Multi-Region Architecture |
| 数据分析口径混乱 | Data Warehouse / Data Mesh Architecture |
| 需要实时数据 | Streaming / Kappa Architecture |
| 企业知识问答 | RAG Architecture |
| AI 执行多步骤任务 | Agentic Architecture |
| 模型上线不可控 | MLOps / LLMOps Architecture |
| 大团队交付慢 | Platform Engineering / IDP Architecture |
| 线上问题定位慢 | Observability / SRE Architecture |
| 权限和合规复杂 | Zero Trust / IAM / Audit Architecture |

## 15. 架构学习建议

建议学习顺序：

1. Application Architecture：Layered、Clean、Hexagonal、Modular Monolith。
2. Domain Architecture：DDD、Bounded Context、Aggregate。
3. Service Architecture：Microservices、API Gateway、BFF、Event-Driven。
4. Reliability Architecture：Cache、Sharding、Circuit Breaker、Rate Limiting、Observability。
5. Cloud Native Architecture：Docker、Kubernetes、Service Mesh、GitOps。
6. Data Architecture：Warehouse、Lakehouse、Streaming、CDC、Data Mesh。
7. AI Architecture：RAG、Agentic、MLOps、LLMOps。
8. Security Architecture：IAM、Zero Trust、Audit、Policy as Code。

实践路线：

- 第一阶段：把一个单体项目做成 Modular Monolith。
- 第二阶段：按 DDD 拆出清晰业务边界。
- 第三阶段：只把最需要独立扩容或独立发布的模块拆成 Microservices。
- 第四阶段：引入消息队列，把同步链路改造成 Event-Driven。
- 第五阶段：加缓存、限流、熔断、监控、链路追踪。
- 第六阶段：容器化，上 Kubernetes，做 CI/CD 和 GitOps。
- 第七阶段：建设数据链路和 AI/RAG 能力。
- 第八阶段：补齐安全、审计、SRE 和平台工程。

## 16. 重要提醒

架构不是越复杂越好。

常见误区：

- 小团队一开始就全量上 Microservices。
- 没有领域边界就拆服务。
- 为了追求“高并发”过早分库分表。
- 上了 Kubernetes 但没有可观测性和发布治理。
- 做 RAG 只建向量库，不做数据质量、权限、评测和更新机制。
- 做 Agent 只关注工具调用，不关注权限、审计、回滚和成本。

更健康的判断标准：

- 架构是否降低了当前真实复杂度？
- 是否让团队协作更清晰？
- 是否让系统更容易测试、发布和回滚？
- 是否降低了故障影响范围？
- 是否让业务变化更容易落地？
- 是否能被团队长期维护？

## 17. References

以下资料用于校准报告中的主流术语和行业实践方向：

- AWS Architecture Center: https://aws.amazon.com/architecture/
- AWS Prescriptive Guidance - Event-driven architecture: https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-integrating-microservices/event-driven.html
- AWS Serverless: https://aws.amazon.com/serverless/
- Google Cloud Architecture Center: https://cloud.google.com/architecture
- Google Cloud - Microservices architecture on Google Cloud: https://cloud.google.com/architecture/microservices-architecture-on-google-cloud
- Microsoft Azure Architecture Center: https://learn.microsoft.com/azure/architecture/
- Microsoft Azure Architecture Styles: https://learn.microsoft.com/azure/architecture/guide/architecture-styles/
- CNCF Cloud Native Definition: https://github.com/cncf/toc/blob/main/DEFINITION.md
- Kubernetes Documentation: https://kubernetes.io/docs/concepts/overview/
- Istio Documentation: https://istio.io/latest/docs/concepts/what-is-istio/
- Martin Fowler - Microservices: https://martinfowler.com/articles/microservices.html
- Martin Fowler - CQRS: https://martinfowler.com/bliki/CQRS.html
- Martin Fowler - Event Sourcing: https://martinfowler.com/eaaDev/EventSourcing.html
- Google SRE Book: https://sre.google/sre-book/table-of-contents/
- Uber Engineering - Domain-Oriented Microservice Architecture: https://www.uber.com/blog/microservice-architecture/
- Netflix Tech Blog: https://netflixtechblog.com/
- OpenAI Platform Documentation: https://platform.openai.com/docs/
- OpenAI Agents Guide: https://platform.openai.com/docs/guides/agents
- OpenAI Retrieval Guide: https://platform.openai.com/docs/guides/retrieval
