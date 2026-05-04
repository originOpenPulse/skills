# Architecture Decision Records

> ADR 用来记录一次架构决策：当时面对什么问题、有哪些选择、为什么选这个、代价是什么、什么时候需要重新评估。

专题首页：[Big Tech Architecture Atlas](../README.md)

## Why ADR Matters

架构不是“知道很多模式”，而是在约束下做选择。

ADR 解决的问题：

- 防止架构决策只存在某个人脑子里。
- 让后来者知道为什么系统不是另一种设计。
- 记录取舍，而不是只记录最终方案。
- 当业务规模变化时，可以回头判断是否需要重构。

## ADR Template

```markdown
# ADR-000X: Decision Title

## Status

Proposed / Accepted / Deprecated / Superseded

## Context

我们面对什么业务问题、技术约束、团队约束？

## Decision

我们决定采用什么架构或设计？

## Options Considered

| Option | Pros | Cons |
|---|---|---|
| Option A | | |
| Option B | | |

## Consequences

### Positive

- 带来什么收益？

### Negative

- 引入什么代价？

### Risks

- 未来可能踩什么坑？

## Review Trigger

什么情况下需要重新评估这个决策？
```

## Example ADRs

- [ADR-0001: Use Modular Monolith First](0001-use-modular-monolith-first.md)
- [ADR-0002: Introduce Event-Driven Architecture for Cross-Module Notifications](0002-introduce-event-driven-architecture.md)
- [ADR-0003: Use RAG Instead of Fine-Tuning for Knowledge QA](0003-use-rag-instead-of-fine-tuning.md)

