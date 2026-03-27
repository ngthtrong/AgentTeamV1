# Agentic Software Team - GitHub Copilot Runtime Instructions

Tai lieu trong `.github-copilot/` la nguon tri thuc va workflow chuan cua team.
Cac file trong `.github/` la lop chat customization native cho GitHub Copilot Chat.

## Muc tieu
- Giu ky luat TDD-first, Git discipline, va human checkpoints.
- Dung dung role (BA/PM/Developer/Tech Lead/Tester/Analyst/Doc Sync) cho dung cong viec.
- Khong de planning artifact chi ton tai local.

## Nguon su that (source of truth)
1. Persona: `.github-copilot/agents/*.md`
2. Workflow command: `.github-copilot/commands/*.md`
3. Template: `.github-copilot/templates/*`
4. Workspace artifacts: `.github-copilot/workspace/**`

Neu co xung dot giua custom prompt va command docs, uu tien command docs.

## Quy tac branch bat buoc
- Khong push truc tiep len `main` hoac `develop`.
- Development phai tren `feature/TASK-xxxxx-*` hoac `fix/BUG-xxxxx-*`.
- Planning artifacts can commit vao `develop` theo workflow PM.

## Cach xu ly khi user goi slash command
Khi user nhac den `/command-name`, phai:
1. Doc `.github-copilot/commands/command-name.md`
2. Doc persona lien quan trong `.github-copilot/agents/`
3. Thuc thi dung thu tu buoc, neu thieu input thi hoi toi thieu
4. Bao cao ket qua voi duong dan file duoc tao/cap nhat

## Human checkpoints khong duoc bo qua
1. Stack review sau `/detect-stack`
2. Spec approval (DRAFT-REQ -> REQ)
3. Untracked files truoc planning commit
4. ADR review truoc code thay doi kien truc
5. Bug P1 triage
6. Smoke test sign-off
7. Release confirmation (`YES`)

## Chat customization map
- Custom agents: `.github/agents/*.agent.md`
- File instructions: `.github/instructions/*.instructions.md`
- Prompt files: `.github/prompts/*.prompt.md`
- Skills: `.github/skills/*/SKILL.md`
- Hooks: `.github/hooks/*.json`
- MCP/plugins/settings: `.vscode/*`
