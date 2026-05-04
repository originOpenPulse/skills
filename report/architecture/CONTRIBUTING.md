# Contributing

专题首页：[Big Tech Architecture Atlas](README.md)

Thanks for helping improve **Big Tech Architecture Atlas**.

This project is a practical architecture learning atlas. Contributions should make architecture easier to understand, review, compare, or apply in real systems.

## Good Contributions

- Add a missing architecture pattern with clear boundaries and I/O.
- Improve an architecture diagram or sequence diagram.
- Add a minimal GitHub demo that is easier to run or understand.
- Add a real-world case study.
- Add an anti-pattern with a clear correction direction.
- Improve the 30-day learning path.
- Fix broken links, outdated demos, or unclear explanations.

## Contribution Principles

- Prefer clarity over completeness.
- Separate architecture core from dependencies.
- Explain trade-offs, not just benefits.
- Use concrete I/O wherever possible.
- Keep diagrams small enough to read in GitHub Markdown.
- Avoid vendor lock-in unless the architecture specifically depends on a vendor capability.

## Document Style

- Use English architecture names.
- Chinese explanations are welcome and currently preferred for the main body.
- Use relative links for internal documents.
- Use Mermaid for diagrams when possible.
- Use tables for comparison, I/O, boundaries, and checklists.

## Adding A New Architecture

When adding a new architecture, update these files when relevant:

1. `big-tech-architecture-report.md`
2. `architecture-boundaries-and-dependencies.md`
3. `architecture-diagrams-and-io.md`
4. `architecture-minimal-github-demos.md`
5. `architecture-anti-patterns.md`
6. `README.md`

Recommended structure:

```text
Architecture:
  Core:
  Boundary:
  Dependencies:
  Supporting Capabilities:
  Standard Input:
  Standard Output:
  Common Anti-Patterns:
  Minimal GitHub Demo:
```

## Pull Request Checklist

- [ ] The change has a clear learning value.
- [ ] Internal links are relative and valid.
- [ ] Mermaid diagrams render in GitHub.
- [ ] The architecture core is separated from dependencies.
- [ ] Any new GitHub demo link points to a useful and public repository.
- [ ] The README or navigation is updated if needed.

