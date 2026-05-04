# ADR-0001: Use Modular Monolith First

专题首页：[Big Tech Architecture Atlas](../README.md)

## Status

Accepted

## Context

业务还在快速变化，团队规模较小，但系统已经出现用户、订单、支付、库存等明显业务边界。

如果一开始直接拆 Microservices，会立刻引入服务治理、分布式事务、链路追踪、部署复杂度和本地调试成本。

## Decision

优先采用 **Modular Monolith Architecture**。

系统保持一个部署单元，但代码按业务模块隔离：

- User Module
- Order Module
- Payment Module
- Inventory Module

模块之间通过公开接口或事件交互，禁止直接访问彼此内部实现。

## Options Considered

| Option | Pros | Cons |
|---|---|---|
| Traditional Monolith | 开发快、部署简单 | 边界容易失控，后期变成 Big Ball of Mud |
| Modular Monolith | 部署简单，边界清晰，未来可拆服务 | 需要纪律约束模块依赖 |
| Microservices | 团队自治、独立扩容、独立发布 | 当前阶段运维和治理成本过高 |

## Consequences

### Positive

- 保持开发和部署简单。
- 业务边界可以先在代码层面验证。
- 未来真正需要时，可以按模块拆服务。

### Negative

- 所有模块仍共享一个进程，无法做到完全独立发布。
- 如果没有依赖规则，仍可能退化成普通单体。

### Risks

- 共享数据库容易破坏模块边界。
- 团队可能绕过模块接口直接调用内部类。

## Review Trigger

当出现以下情况时，重新评估是否拆成 Microservices：

- 某个模块需要独立扩容。
- 某个模块发布频率明显高于其他模块。
- 不同团队需要独立拥有不同模块。
- 单体构建、测试、发布周期已经影响交付效率。

