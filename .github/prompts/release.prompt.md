---
name: Release
description: "Thuc hien release gate theo workflow va xac nhan sign-off/checkpoints."
argument-hint: "v1.0.0"
agent: agent-orchestrator
tools: [read, search, execute, todo, agent]
---
Thuc hien `/release` cho version duoc truyen vao.

Yeu cau:
1. Verify smoke sign-off va commit hash cross-check.
2. Verify tat ca human checkpoints da dat.
3. Neu thieu dieu kien, dung lai va neu ro blocker.
4. Chi cho phep tiep tuc khi co xac nhan `YES`.
