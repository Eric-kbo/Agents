# 手填版模板（v1.0）

最早期的版本：所有内容都在一个文件里，所有 `[FILL]` 占位符需要人工填写，不依赖任何 Agent 推断。

## 用法

1. 拷贝 [ai-team-template.md](ai-team-template.md) 到你的项目 `.github/copilot-instructions.md`
2. 全文搜索 `[FILL`，逐项替换为你项目的实际信息
3. 完成

## 何时选它？

- 你**不想**让 Agent 自动推断任何内容
- 你想从零开始**完全手写**项目规范
- 你需要**最小化的 Agent 介入**作为审计/合规要求

## 否则

更推荐：
- 想要快：用 [`prompts/generate-ai-team.prompt.md`](../../prompts/generate-ai-team.prompt.md)
- 想要可维护：用 [`templates/split/`](../split/)
