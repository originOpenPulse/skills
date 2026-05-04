# Architecture Review Checklist

> 目标：把架构知识变成读代码、评审系统、分析项目的工具。  
> 使用方式：看任何项目时，先不要陷入函数细节，先用这份 Checklist 找到系统的边界、依赖、I/O 和失败路径。

专题首页：[Big Tech Architecture Atlas](README.md)

## 1. 30-Minute Quick Review

适合第一次看陌生项目。

| Question | What To Look For |
|---|---|
| 系统入口是什么？ | HTTP、RPC、Message、CLI、Scheduler、UI |
| 核心业务在哪里？ | Domain、Use Case、Service、Aggregate |
| 边界在哪里？ | Module、Service、Bounded Context、Port、API |
| 状态在哪里？ | DB、Cache、Event Store、Read Model、Vector DB |
| 依赖有哪些？ | Queue、DB、LLM、Kubernetes、External API |
| 请求怎么流动？ | Controller -> Service -> Repository，或 Event -> Consumer |
| 失败怎么处理？ | Retry、Timeout、Circuit Breaker、Compensation、Fallback |
| 如何观测？ | Metrics、Logs、Traces、Audit Events |

## 2. Boundary Checklist

- 是否能清楚说出这个模块或服务负责什么？
- 是否能清楚说出它不负责什么？
- 是否有公开 API 或 Port？
- 是否有其他模块直接访问它的内部类、内部表或内部状态？
- 是否存在多个模块共同修改同一张核心业务表？
- 是否存在“工具类”承载大量业务逻辑？
- 是否能独立测试核心业务规则？

## 3. Dependency Checklist

- 业务核心是否依赖框架、数据库、HTTP、消息队列？
- 外部系统是否通过接口或 Adapter 隔离？
- 数据库访问是否泄漏到 Controller 或 UI 层？
- 第三方 API 失败是否会拖垮核心链路？
- 是否有依赖方向规则？
- 是否有循环依赖？
- 是否能替换一个依赖而不大改业务核心？

## 4. I/O Contract Checklist

- 每个边界的输入是什么？
- 每个边界的输出是什么？
- Command 和 Query 是否混在一起？
- Event 是否有清晰语义和 Schema？
- API 是否有版本策略？
- 错误响应是否标准化？
- 是否区分同步返回和异步结果？

## 5. Data Checklist

- 数据 Owner 是谁？
- 交易数据和分析数据是否隔离？
- 读模型和写模型是否承担了不同需求？
- 是否存在重复数据？重复数据如何同步？
- CDC、事件、批处理链路是否有延迟和失败处理？
- 指标口径是否统一？
- 敏感数据是否有权限和审计？

## 6. Reliability Checklist

- 是否设置超时？
- 是否设置重试上限和退避？
- 是否有熔断？
- 是否有资源隔离？
- 是否有降级路径？
- 是否有幂等设计？
- 是否有死信队列或失败补偿？
- 是否能局部失败而不影响全局？

## 7. Security Checklist

- 身份认证在哪里做？
- 权限判断在哪里做？
- 是否遵守最小权限？
- 服务间调用是否有身份？
- 敏感操作是否审计？
- Secret 是否安全管理？
- Agent 或自动化工具是否有权限边界？
- 多租户数据是否隔离？

## 8. AI Architecture Checklist

- RAG 是否有文档清洗、切分、Embedding、检索、重排、生成和评测？
- 检索是否做权限过滤？
- Prompt 是否版本化？
- LLM 调用是否有 Trace？
- 是否评估回答质量、成本和延迟？
- Agent 工具是否有白名单？
- Agent 执行动作是否有审计和回滚？
- 模型或 Embedding 变化是否可回归测试？

## 9. Architecture Smell List

看到这些信号要警惕：

- 一个 Service 文件超过很多业务职责。
- Controller 里有复杂业务判断。
- 数据库表被多个模块随意读写。
- 微服务共享同一个数据库。
- 事件没有 Owner，也没有 Schema。
- 所有错误都靠重试解决。
- 缓存没有失效策略。
- 监控只有日志，没有 Trace 和指标。
- RAG 只有向量库，没有评测。
- Agent 能调用太多工具但没有权限控制。

## 10. Review Output Template

```markdown
# Architecture Review

## Summary

一句话描述系统当前架构。

## Architecture Identified

- Primary:
- Supporting:

## Core

- 核心业务组件：

## Boundaries

- 清晰的边界：
- 模糊的边界：

## Dependencies

- 外部依赖：
- 泄漏到核心的依赖：

## I/O

- 关键输入：
- 关键输出：

## Risks

| Risk | Impact | Suggestion |
|---|---|---|
| | | |

## Next Actions

1. 
2. 
3. 
```

