# templates/

> 模板源文件 —— 你可以直接拷贝、二次裁剪、或者作为生成器的"参考语料"。

## 三个版本，按需选用

| 目录 | 形态 | 适合谁 |
| --- | --- | --- |
| [single-file/](single-file/) | **单文件版**：一个 `copilot-instructions.md` 容纳协议层 + 生成器指令 | 喜欢"一个文件搞定"的极简主义者 |
| [split/](split/) | **分离式版**：协议层 + 角色模板集 + 独立生成器 prompt | 想要细粒度复用、模板可独立维护的团队 |
| [legacy/](legacy/) | **手填版（v1.0）**：传统的"挖空 → 人工填空"模板 | 不想用 Agent 自动生成、希望逐项手写规范的团队 |

## 选择决策树

```
你想要什么？
├─ 立即生成、零配置          → 用 ../prompts/generate-ai-team.prompt.md
├─ 一个文件、可二次魔改      → single-file/
├─ 模块化、多人协同维护规范  → split/
└─ 全程手写、不依赖 Agent    → legacy/
```

## 与 `prompts/` 的关系

- `prompts/generate-ai-team.prompt.md` 是 **执行器**：扫描项目 → 推断 → 输出
- `templates/` 是 **物料库**：定义最终产物的结构和契约
- 你完全可以 fork 本仓库，自定义 `templates/` 后，让生成器吐出符合你团队风格的规范
