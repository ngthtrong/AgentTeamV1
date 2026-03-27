# Agent Registry for GitHub Copilot

Tai lieu nay la ban do nhanh de Copilot va team hieu cach map role sang custom agents.

## Agent map
- BA -> `.github/agents/ba.agent.md`
- PM -> `.github/agents/pm.agent.md`
- Developer -> `.github/agents/developer.agent.md`
- Tech Lead -> `.github/agents/techlead.agent.md`
- Tester -> `.github/agents/tester.agent.md`
- Analyst -> `.github/agents/analyst.agent.md`
- Doc Sync -> `.github/agents/docsync.agent.md`
- Orchestrator -> `.github/agents/orchestrator.agent.md`

## Rule
- Mỗi agent chi giai quyet mot loai trach nhiem chinh.
- Neu task lon, Orchestrator duoc phep delegate cho subagent phu hop.
- Phan source-of-truth cua workflow van o `.github-copilot/commands/`.
