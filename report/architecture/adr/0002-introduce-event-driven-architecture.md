# ADR-0002: Introduce Event-Driven Architecture for Cross-Module Notifications

专题首页：[Big Tech Architecture Atlas](../README.md)

## Status

Accepted

## Context

订单创建后，需要触发库存预留、优惠券核销、通知发送、积分发放和数据分析。

如果全部使用同步调用，Order 模块会依赖多个下游模块，导致调用链变长、响应变慢、失败影响扩大。

## Decision

引入 **Event-Driven Architecture** 处理跨模块或跨服务通知。

Order 模块只发布 `OrderCreated` 事件，其他模块按需订阅。

## Options Considered

| Option | Pros | Cons |
|---|---|---|
| Synchronous Calls | 实现直观，结果即时 | 强耦合、链路长、容易级联失败 |
| Event-Driven | 解耦、可扩展、适合新增订阅者 | 最终一致、调试复杂、需要事件契约治理 |
| Workflow Orchestration | 流程清晰、状态可追踪 | 对简单通知场景偏重 |

## Consequences

### Positive

- Order 模块不需要知道所有下游订阅者。
- 新业务可以通过订阅事件扩展。
- 下游失败不直接阻塞主流程。

### Negative

- 系统从强一致变成最终一致。
- 需要处理重复消费、乱序、失败重试和死信。

### Risks

- 事件 Schema 随意变化会破坏消费者。
- 事件语义不清会导致多个团队理解不一致。

## Review Trigger

当事件链路变成复杂业务流程，并需要可视化状态、补偿和人工干预时，评估是否引入 Saga 或 Workflow Engine。

