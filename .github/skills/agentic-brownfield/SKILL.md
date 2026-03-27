---
name: agentic-brownfield
description: 'Discover and document existing systems with 10-phase brownfield analysis and drift detection workflows.'
argument-hint: 'analysis scope (full/module/api/database)'
user-invocable: true
---
# Agentic Brownfield Skill

## When to Use
- Moi tiep quan mot codebase co san.
- Can cap nhat phan tich va drift report sau nhieu thay doi.

## Procedure
1. Chay discover-codebase theo thu tu 10 giai doan.
2. Tao/cap nhat tai lieu trong `.github-copilot/workspace/analysis/`.
3. Danh dau fragile zones voi risk level.
4. Doi chieu tai lieu hien tai va code de tao drift report.
5. De xuat uu tien khac phuc theo risk.

## Required References
- `.github-copilot/commands/discover-codebase.md`
- `.github-copilot/commands/doc-sync.md`
- `.github-copilot/agents/analyst.md`
- `.github-copilot/agents/docsync.md`
