# Huong Dan Su Dung Agentic Software Team voi GitHub Copilot

> Phien ban da tinh chinh de tan dung native chat customizations: agents, instructions, prompts, skills, hooks, MCP servers, va VS Code plugins.

---

## Muc Luc

1. [Tong quan](#tong-quan)
2. [Kien truc 3 lop](#kien-truc-3-lop)
3. [Config dat o dau](#config-dat-o-dau)
4. [Cai dat nhanh](#cai-dat-nhanh)
5. [Doi ngu Agent](#doi-ngu-agent)
6. [Quy trinh chuan](#quy-trinh-chuan)
7. [Prompt va Skill](#prompt-va-skill)
8. [Hooks va Runtime Guardrails](#hooks-va-runtime-guardrails)
9. [MCP Servers va Plugins](#mcp-servers-va-plugins)
10. [Human Checkpoints](#human-checkpoints)
11. [Troubleshooting](#troubleshooting)
12. [Best Practices](#best-practices)

---

## Tong quan

**Agentic Software Team** duoc chia ro 2 loai tri thuc:

- **Workflow knowledge**: nam trong `.github-copilot/` (persona, command, template, artifacts)
- **Runtime customization**: nam trong `.github/` va `.vscode/` (agents, instructions, prompts, skills, hooks, MCP, plugin settings)

Triet ly van giu nguyen:

> AI thuc thi ky luat, con nguoi giu quyen phan doan.

---

## Kien truc 3 lop

```text
Lop 1 - Workflow/Knowledge (Source of truth)
.github-copilot/
  agents/
  commands/
  templates/
  workspace/
  PROJECT-PROFILE.md

Lop 2 - Copilot Chat Customizations (Native)
.github/
  copilot-instructions.md
  AGENTS.md
  agents/*.agent.md
  instructions/*.instructions.md
  prompts/*.prompt.md
  skills/*/SKILL.md
  hooks/*.json

Lop 3 - IDE Runtime Integration
.vscode/
  settings.json
  mcp.json
  extensions.json
```

---

## Config dat o dau

| Nhom config | Vi tri khuyen nghi | Muc dich |
|-------------|--------------------|----------|
| Persona va workflow command | `.github-copilot/agents`, `.github-copilot/commands` | Nguon su that de thuc thi quy trinh |
| Always-on project instruction | `.github/copilot-instructions.md` | Rule runtime chung cho Copilot Chat |
| Agent modes (chat custom agents) | `.github/agents/*.agent.md` | Chon role nhanh trong Chat |
| File-scoped guidance | `.github/instructions/*.instructions.md` | Rule theo file pattern (`applyTo`) |
| Slash prompts tai su dung | `.github/prompts/*.prompt.md` | Mau prompt chay nhanh qua `/` |
| Workflow skill da dong goi | `.github/skills/*/SKILL.md` | Tac vu lap lai, co quy trinh ro |
| Runtime policy hooks | `.github/hooks/*.json` + scripts | Chan hanh vi rui ro o cap tool |
| MCP server definitions | `.vscode/mcp.json` | Ket noi cong cu/nguon du lieu ben ngoai |
| Plugin recommendations | `.vscode/extensions.json` | Dong bo moi truong lam viec cho team |
| Workspace-level chat settings | `.vscode/settings.json` | Bat/tat tinh nang chat theo du an |

**Luu y quan trong**:
- Khong dat custom agent/prompt/instruction/hook vao `.github-copilot/`.
- `.github-copilot/` la noi luu workflow docs, KHONG phai runtime config directory cua Copilot Chat.

---

## Cai dat nhanh

### 1. Kiem tra cac thu muc can thiet

- `.github-copilot/` da co du `agents/commands/templates/workspace`
- `.github/` da co `copilot-instructions`, `agents`, `instructions`, `prompts`, `skills`, `hooks`
- `.vscode/` da co `settings.json`, `mcp.json`, `extensions.json`

### 2. Cai extension khuyen nghi

Mo VS Code -> Extensions -> cai theo `.vscode/extensions.json`:
- GitHub Copilot
- GitHub Copilot Chat
- GitHub Pull Requests and Issues
- GitLens
- PowerShell

### 3. Bat instruction files va prompt files

Dam bao workspace dang dung `.vscode/settings.json` va extension Copilot Chat da active.

### 4. Khoi dong voi orchestrator

Trong Chat:

```text
@workspace Su dung agent-orchestrator va thuc hien /setup-project
```

Hoac go `/` de chon prompt da dinh nghia (Analyze Requirements, Plan Sprint, Implement Task, ...).

---

## Doi ngu Agent

### Nguon persona

- Persona goc: `.github-copilot/agents/*.md`
- Chat custom agents: `.github/agents/*.agent.md`

### Mapping role

| Role | Custom agent | Persona goc |
|------|--------------|-------------|
| BA | `agent-ba` | `.github-copilot/agents/ba.md` |
| PM | `agent-pm` | `.github-copilot/agents/pm.md` |
| Developer | `agent-developer` | `.github-copilot/agents/developer.md` |
| Tech Lead | `agent-techlead` | `.github-copilot/agents/techlead.md` |
| Tester | `agent-tester` | `.github-copilot/agents/tester.md` |
| Analyst | `agent-analyst` | `.github-copilot/agents/analyst.md` |
| Doc Sync | `agent-docsync` | `.github-copilot/agents/docsync.md` |
| Orchestrator | `agent-orchestrator` | `.github/AGENTS.md` + command routing |

---

## Quy trinh chuan

```text
1) Setup va discovery
   /setup-project -> /detect-stack -> /discover-codebase -> /update-agents

2) Requirements
   Tao BRIEF -> /analyze-requirements -> PO doi ten DRAFT-REQ -> REQ

3) Planning
   /plan-sprint -> commit planning artifacts vao develop

4) Development loop
   /write-unit-tests -> /implement-task -> /run-tests -> /check-coverage
   -> /create-pr -> /techlead-review -> /merge-pr

5) Release
   /smoke-test -> PO sign-off -> /release (xac nhan YES)
```

### Rule branch bat buoc

- Khong push truc tiep len `main`/`develop`
- Feature tren `feature/TASK-xxxxx-*`
- Bug fix tren `fix/BUG-xxxxx-*`

---

## Prompt va Skill

### Prompt files (`.github/prompts/*.prompt.md`)

Duoc goi nhanh qua `/` trong Chat:
- Analyze Requirements
- Plan Sprint
- Implement Task
- Tech Lead Review
- Smoke Test
- Release

### Skills (`.github/skills/*/SKILL.md`)

- `agentic-sprint`: dieu phoi sprint end-to-end
- `agentic-brownfield`: brownfield discovery + drift detection

Khi task lap lai, uu tien skill thay vi viet prompt tu do moi lan.

---

## Hooks va Runtime Guardrails

### Hook files

- Config: `.github/hooks/policy.json`
- Scripts: `.github/hooks/scripts/*.ps1`

### Muc tieu

- Inject system message ngay SessionStart
- Chan command rui ro cao truoc khi tool chay (PreToolUse)

### Cac command dang bi chan (mac dinh)

- `git reset --hard`
- `git checkout --`
- `git clean -fd`
- `git push origin main`
- `git push origin develop`

Ban co the tuy chinh them pattern theo quy dinh team.

---

## MCP Servers va Plugins

### MCP config

File `.vscode/mcp.json` dang co sample servers:
- `filesystem`
- `github`
- `sequential-thinking`

**Luu y**:
- Server `github` can token (`GITHUB_PERSONAL_ACCESS_TOKEN`)
- Neu chua can dung MCP nao, co the xoa/tat server do trong `mcp.json`

### Plugin recommendations

File `.vscode/extensions.json` giup team cai cung bo extension de tranh lech moi truong.

---

## Human Checkpoints

7 diem khong duoc bo qua:

1. Stack review sau `/detect-stack`
2. Spec approval (DRAFT-REQ -> REQ)
3. Untracked files truoc planning commit
4. ADR review truoc architectural change
5. Bug P1 triage
6. Smoke test sign-off
7. Release confirmation (`YES`)

---

## Troubleshooting

### Khong thay custom agents trong chat

- Kiem tra file `.github/agents/*.agent.md` co frontmatter hop le
- Kiem tra extension GitHub Copilot Chat da bat
- Reload window trong VS Code

### Prompt/Skill khong hien khi go `/`

- Kiem tra ten file dung duoi `.prompt.md` hoac `SKILL.md`
- Kiem tra `description` co du ro rang trigger phrase
- Kiem tra `name` trong SKILL trung voi ten folder

### Instruction khong duoc ap dung

- Kiem tra `applyTo` pattern co match file dang sua
- Kiem tra setting instruction files da bat trong `.vscode/settings.json`

### Hook khong chay

- Kiem tra JSON syntax trong `.github/hooks/policy.json`
- Kiem tra script path ton tai va co the thuc thi bang `pwsh`

### MCP server khong ket noi

- Kiem tra da cai runtime can thiet (`node`, `npx`)
- Kiem tra env vars va token
- Thu chay command server thu cong trong terminal de debug

---

## Best Practices

1. Dung `.github-copilot` cho tri thuc workflow, dung `.github` cho chat runtime customization.
2. Moi role co custom agent rieng, tranh mot agent lam tat ca.
3. Prompt cho tac vu don, Skill cho workflow lap lai.
4. Hooks de enforce policy bat buoc; instructions chi de huong dan.
5. Luon cap nhat `PROJECT-PROFILE.md` sau moi lan thay doi stack/command.
6. Commit planning artifacts som de tranh mat state giua cac phien chat.
7. Khong bo qua human checkpoints du deadline gap.

---

*Agentic Software Team v1.1 for GitHub Copilot Chat*  
*Ky luat workflow o `.github-copilot`, runtime customizations o `.github` va `.vscode`.*
