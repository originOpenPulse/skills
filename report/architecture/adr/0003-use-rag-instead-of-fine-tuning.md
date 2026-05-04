# ADR-0003: Use RAG Instead of Fine-Tuning for Knowledge QA

专题首页：[Big Tech Architecture Atlas](../README.md)

## Status

Accepted

## Context

系统需要回答企业内部文档、产品手册、项目报告和知识库中的问题。

知识更新频繁，且需要回答时引用最新资料。直接 Fine-Tuning 成本高、更新慢，也不容易保证答案来源。

## Decision

优先采用 **RAG Architecture**。

核心链路：

```text
Document -> Chunking -> Embedding -> Vector DB -> Retrieval -> Rerank -> LLM Generation -> Evaluation
```

## Options Considered

| Option | Pros | Cons |
|---|---|---|
| Prompt Only | 实现最快 | 上下文有限，无法覆盖大量私有知识 |
| Fine-Tuning | 可学习风格和任务格式 | 不适合频繁更新知识，成本高，溯源弱 |
| RAG | 知识可更新、可引用、成本相对低 | 检索质量决定回答质量，需要评测和权限过滤 |

## Consequences

### Positive

- 文档更新后可以通过重建索引快速生效。
- 回答可以绑定检索上下文。
- 不需要每次知识变化都重新训练模型。

### Negative

- 需要治理文档质量、切分策略和检索效果。
- 如果检索错了，模型大概率也会答错。

### Risks

- 私有文档权限过滤不到位会造成数据泄露。
- 只做向量召回，不做评测，会让质量不可控。

## Review Trigger

当需求从“知识问答”变成“稳定执行固定任务格式”时，可以评估 Fine-Tuning 或模型蒸馏是否更合适。

