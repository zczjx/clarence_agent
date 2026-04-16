# clarence_agent

演示用 **本地开发运维（local-devops）** Agent 配置，兼容 **Claude Code**、**OpenClaw** 与 **ZeroClaw**（技能均为 [Agent Skills](https://agentskills.io/) 布局）。

## 内容说明

| 路径 | 用途 |
|------|------|
| `.claude/agents/local-devops.md` | Claude Code 子代理（含 `skills:` 引用） |
| `.agents/skills/*/SKILL.md` | 共享技能：`ubuntu-devops`、`safe-shell`、`ops-triage` |
| `.claude/skills/*` | 指向 `.agents/skills` 的符号链接，避免重复维护 |
| `contrib/openclaw-local-devops.json5` | OpenClaw `agents.list` 片段示例 |

## Claude Code

1. 在本仓库根目录打开 Claude Code。
2. 使用 `/agents` 确认存在 **local-devops**，或在对话中说明由该子代理处理本机运维问题。
3. 技能可通过 `/ubuntu-devops`、`/safe-shell`、`/ops-triage` 调用（与 `SKILL.md` 中 `name` 一致）。

## OpenClaw

- 将 OpenClaw 工作区指向本仓库根目录时，会加载项目内 **`/.agents/skills`**（见 [OpenClaw Skills 文档](https://docs.openclaw.ai/skills/)）。
- 若使用多 agent 并希望单独限制技能列表，将 `contrib/openclaw-local-devops.json5` 中 `agents.list` 合并进 `~/.openclaw/openclaw.json`（按需调整）。

## ZeroClaw

- ZeroClaw 使用全局配置（如 `~/.zeroclaw/config.toml`）；技能本身为通用 `SKILL.md` 目录。
- 可将本仓库 `.agents/skills/` 复制到工具允许的技能目录，或在支持 **AgentSkills** 路径时指向本目录；CLI  autonomy 建议开发机使用 `read_only` 或 `supervised` 以降低误操作风险。

## 技能一览

- **ubuntu-devops**：systemd/journalctl、资源与网络只读排查。
- **safe-shell**：破坏性命令前确认、默认只读。
- **ops-triage**：「起不来 / 连不上 / 很慢」的分诊顺序。
