---
name: agentic-sprint
description: 'Run full sprint workflow for Agentic Software Team: requirements, planning, implementation, review, testing, and release gates.'
argument-hint: 'REQ-ID/TASK-ID or sprint objective'
user-invocable: true
---
# Agentic Sprint Skill

## When to Use
- Can mot luong sprint end-to-end tu requirement den release.
- Can dam bao team follow dung command order va checkpoints.

## Procedure
1. Validate input va xac dinh phase hien tai.
2. Chon command tiep theo trong `.github-copilot/commands/`.
3. Thuc thi va cap nhat artifact trong `.github-copilot/workspace/`.
4. Dung lai tai moi human checkpoint de xin phe duyet.
5. Tong hop ket qua va next action.

## Required References
- `.github-copilot/USAGE-GUIDE.md`
- `.github-copilot/copilot-instructions.md`
- `.github-copilot/commands/*.md`
- `.github/AGENTS.md`
