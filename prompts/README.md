# prompts/

> 即开即用的 Prompt 入口。

## 文件清单

| 文件 | 用途 | 推荐场景 |
| --- | --- | --- |
| [generate-ai-team.prompt.md](generate-ai-team.prompt.md) | **一键生成 AI 团队规范**：扫描项目 → 推断技术栈 → 输出 9 个角色规范文件 | 任意新/旧项目，零配置启动 |

## 使用方式

### 在 GitHub Copilot Chat 中

1. 打开你的项目（任意语言/框架均可）
2. 复制 [generate-ai-team.prompt.md](generate-ai-team.prompt.md) 全部内容
3. 粘贴到 Copilot Chat（推荐 Agent 模式 / Claude Sonnet 4 及以上）
4. Agent 会自动扫描项目并生成 `.github/copilot-instructions.md` + `.github/agents/*.md` + `.github/memory/*.md`

### 在 Cursor / Cline / 其他 Agent 中

同样适用。Prompt 不依赖 IDE 特性，只要 Agent 具备读文件 + 写文件能力即可。

### 在 VS Code 中作为 `.prompt.md` 文件

将文件复制到 `~/.config/Code/User/prompts/` 后，在 Copilot Chat 中输入 `/generate-ai-team` 即可调用。

## 想要更细颗粒度的控制？

请改用 [`templates/`](../templates/) 中的分离式模板。
