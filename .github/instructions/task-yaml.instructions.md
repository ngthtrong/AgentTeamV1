---
description: "Use when creating or editing sprint task files, dependency chains, or task metadata for TASK-xxxxx YAML files."
applyTo: ".github-copilot/workspace/tasks/**/*.yaml"
---
# TASK YAML Instructions

- Task ID phai theo dinh dang `TASK-00001` (5 digits).
- Moi feature nen co chuoi: `unit-test` -> `implementation` -> `e2e-test`.
- `implementation` phai depend vao `unit-test` cung feature.
- `e2e-test` phai depend vao `implementation`.
- Mo ta task phai action-oriented, testable, va co output artifact ro rang.
- Khong tao task trung ID, khong nhay coc sequence neu khong co ly do.
