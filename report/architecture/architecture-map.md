# Architecture Map

> A visual map of how this atlas is organized.

专题首页：[Big Tech Architecture Atlas](README.md)

## Learning Flow

```mermaid
flowchart LR
  Start[Start: Architecture Overview] --> Boundary[Separate Core / Boundary / Dependency]
  Boundary --> Diagram[Read Architecture Diagrams]
  Diagram --> IO[Understand Standard I/O]
  IO --> Demo[Read Minimal GitHub Demos]
  Demo --> Review[Use Review Checklist]
  Review --> ADR[Write Architecture Decision Records]
  Review --> Case[Apply Case Studies]
```

## Architecture Categories

```mermaid
flowchart TB
  Atlas[Big Tech Architecture Atlas]

  Atlas --> App[Application Architecture]
  Atlas --> Service[Service Architecture]
  Atlas --> Cloud[Cloud Native Architecture]
  Atlas --> Data[Data Architecture]
  Atlas --> AI[AI Architecture]
  Atlas --> Eng[Engineering Productivity]
  Atlas --> Sec[Security Architecture]
  Atlas --> Res[Resilience Architecture]
  Atlas --> FE[Frontend Architecture]

  App --> AppItems[Monolith<br/>Modular Monolith<br/>Layered<br/>Clean<br/>Hexagonal<br/>DDD]
  Service --> ServiceItems[SOA<br/>Microservices<br/>API Gateway<br/>BFF<br/>Event-Driven<br/>CQRS<br/>Event Sourcing<br/>Saga<br/>Service Mesh]
  Cloud --> CloudItems[Cloud Native<br/>Kubernetes<br/>Serverless<br/>Cell-Based<br/>Multi-Region<br/>Edge]
  Data --> DataItems[Warehouse<br/>Lake<br/>Lakehouse<br/>Streaming<br/>Lambda<br/>Kappa<br/>CDC<br/>Data Mesh]
  AI --> AIItems[ML Platform<br/>MLOps<br/>RAG<br/>Agentic<br/>LLMOps<br/>Vector DB]
  Eng --> EngItems[DevOps<br/>GitOps<br/>Platform Engineering<br/>IDP<br/>Observability<br/>SRE]
  Sec --> SecItems[Zero Trust<br/>IAM<br/>Policy as Code<br/>Audit]
  Res --> ResItems[Cache<br/>Sharding<br/>Read-Write Splitting<br/>Circuit Breaker<br/>Rate Limiting]
  FE --> FEItems[Micro Frontends<br/>Component-Based<br/>Design System]
```

## How To Read Any Architecture

```mermaid
flowchart LR
  A[Architecture Name] --> C[Core]
  A --> B[Boundary]
  A --> D[Dependencies]
  A --> S[Supporting Capabilities]
  A --> I[Standard Input]
  A --> O[Standard Output]
  A --> F[Failure Path]

  C --> Q1[What makes this architecture valid?]
  B --> Q2[What does it isolate?]
  D --> Q3[What can be replaced?]
  S --> Q4[What helps it operate safely?]
  I --> Q5[What does it consume?]
  O --> Q6[What does it produce?]
  F --> Q7[How does it fail?]
```

## From Concept To Practice

```mermaid
flowchart TB
  Concept[Concept] --> Boundary[Boundary and Dependency]
  Boundary --> Diagram[Component Diagram]
  Diagram --> Sequence[Sequence Diagram]
  Sequence --> IO[I/O Contract]
  IO --> Demo[Minimal Demo]
  Demo --> Anti[Anti-Patterns]
  Anti --> Checklist[Review Checklist]
  Checklist --> ADR[ADR]
```

