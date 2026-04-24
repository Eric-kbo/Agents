<div align="center">

# 🤖 AI Team — 一键为你的项目装上「九角色协作团队」

**让 GitHub Copilot / Cursor / Cline 不再是单兵作战的实习生，而是一支有规范、有记忆、会仲裁的工程团队。**

[English](README.en.md) · 中文 · [快速开始](#-快速开始30-秒) · [模板对比](#-三种模板按需选用) · [设计理念](#-设计理念)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#-贡献)
[![Made for Copilot](https://img.shields.io/badge/Made%20for-Copilot%20%2F%20Cursor%20%2F%20Cline-blue)](#)
[![No Dependencies](https://img.shields.io/badge/dependencies-zero-success)](#)

</div>

---

## 🧩 它解决了什么问题？

你是否经历过以下场景：

- ❌ Copilot 写出的代码 **每次风格都不一样**，命名混乱、架构跳层
- ❌ Agent 中途丢失上下文，**说"继续"它却从头再来**
- ❌ 让它修 bug，结果**顺手把架构重构了**还引入新依赖
- ❌ 多人协作时，**每个人的 AI 输出都不像一个项目里的代码**
- ❌ 想要约束 AI，写了一堆 `copilot-instructions.md`，**越写越乱越长**

**AI Team** 用一份 Prompt，给你的项目装上一个 **结构化的 9 角色 AI 团队**：

| 角色 | 职责 | 避免什么 |
| --- | --- | --- |
| 🎯 产品 | 把模糊需求结构化为带验收标准的 PRD | 需求口口相传、没有边界 |
| 🛠 开发 | 强制按"数据 → 业务 → 状态 → UI"分层落地 | UI 层直接 fetch、跨层耦合 |
| 🧪 测试 | 必测场景清单 + 覆盖率门槛 | 写完功能没测试 |
| ✅ 验证 | 交付前的硬性检查清单 | "应该没问题"就 push |
| ⚖ 仲裁 | 规范冲突时分类裁定（A/B/C 型） | 两种方案都对、Agent 自由发挥 |
| 🧠 记忆 | 持久化任务状态、支持「继续/恢复」 | 上下文丢失、重新解释 |
| 📈 提升 | 任务后反思 + 规范版本管理 | 规范一写就僵化 |
| 🚦 路由 | 根据关键词自动加载对应角色 | 一次性灌输全部规则导致幻觉 |
| 📜 协议 | 价值观 + 权威排序 + 全局禁令 | 角色之间互相打架 |

---

## ⚡ 快速开始（30 秒）

### 方式一：在 GitHub Copilot Chat / Cursor / Cline 中

```bash
# 1. 进入你的项目（任意语言、任意框架）
cd your-project

# 2. 复制本仓库的入口 prompt
curl -O https://raw.githubusercontent.com/<your-username>/Agents/main/prompts/generate-ai-team.prompt.md

# 3. 在 Copilot Chat / Cursor 中粘贴 generate-ai-team.prompt.md 全部内容
#    （建议 Agent 模式 + Claude Sonnet 4 / GPT-5 / 同级模型）

# 4. 喝口水。Agent 会自动：
#    ✓ 扫描 package.json / pubspec.yaml / pom.xml / go.mod ...
#    ✓ 推断架构分层、命名规范、数据流
#    ✓ 生成 .github/copilot-instructions.md
#    ✓ 生成 .github/agents/{product,dev,test,verify,arbitrate,memory,enhance}.md
#    ✓ 初始化 .github/memory/{arbitration-log,decisions}.md
```

完成后，你的 `.github/` 目录长这样：

```
.github/
├── copilot-instructions.md          ← 协议层：路由、价值观、禁止清单
├── agents/
│   ├── product.md                   ← 产品角色规范
│   ├── dev.md                       ← 开发规范（含项目专属架构 / 命名）
│   ├── test.md                      ← 测试规范
│   ├── verify.md                    ← 验收检查清单
│   ├── arbitrate.md                 ← 冲突仲裁
│   ├── memory.md                    ← 记忆管理
│   └── enhance.md                   ← 反思与版本管理
└── memory/
    ├── arbitration-log.md           ← 历史裁定（git 追踪）
    └── decisions.md                 ← 架构决策（git 追踪）
```

之后 Copilot 处理任意任务，都会**先按关键词加载对应角色**，按规范执行，输出可预期、可审计。

### 方式二：作为 VS Code 用户级 Prompt

把 [`prompts/generate-ai-team.prompt.md`](prompts/generate-ai-team.prompt.md) 复制到：

- **Windows**: `%APPDATA%\Code\User\prompts\`
- **macOS**: `~/Library/Application Support/Code/User/prompts/`
- **Linux**: `~/.config/Code/User/prompts/`

之后在 Copilot Chat 输入 `/generate-ai-team` 即可一键调用。

---

## 🎁 三种模板，按需选用

| 你想要 | 用这个 | 路径 |
| --- | --- | --- |
| 🚀 **30 秒一键生成**，零配置 | 一键 Prompt | [`prompts/generate-ai-team.prompt.md`](prompts/generate-ai-team.prompt.md) |
| 📦 **单文件**含协议+生成器，便于复用 | 单文件版模板 | [`templates/single-file/`](templates/single-file/) |
| 🧱 **模块化**，团队可分别维护协议/角色/生成器 | 分离式模板 | [`templates/split/`](templates/split/) |
| ✍️ **全程手写**，不依赖 Agent 推断 | 手填版模板 | [`templates/legacy/`](templates/legacy/) |

> 决策不下？默认走第一种 —— 大多数项目都适用。

---

## 🌍 框架兼容性

测试覆盖（持续扩展中）：

| 类别 | 已验证 | 自动推断依据 |
| --- | --- | --- |
| 移动端 | Flutter | `pubspec.yaml` |
| 前端 | React、Vue、Next.js、Svelte | `package.json` |
| 后端 | Node.js、Java(Spring)、Kotlin、Python(FastAPI/Django)、Go、Rust | 对应清单文件 |
| 桌面 | Electron、Tauri、Flutter Desktop | `package.json` / `Cargo.toml` |

> 没列出的框架？大多数情况一样能跑 —— Agent 通过目录结构 + 抽样源码即可推断分层和命名规范。如果跑挂了，[提个 issue](#-贡献) 我们会优先支持。

---

## 💡 设计理念

### 1. 角色化优于一锅烩

传统 `copilot-instructions.md` 把所有规则混在一起，AI 每次都要"通读 + 自行筛选"，**幻觉频发、上下文爆炸**。

我们用 **路由表** 让 Agent 只加载当前任务相关的角色：

```
"修复登录 bug"   → 加载 dev.md + verify.md          （略过产品、测试模板）
"设计新需求"     → 加载 product.md                  （不污染开发上下文）
"继续上次任务"   → 加载 memory.md（前置）+ 状态文件  （恢复中断点）
```

### 2. 仲裁优于自由发挥

当两种方案都"看起来合理"时，Agent 通常会**自己拍板**。我们引入 **三型仲裁**：

- **A 型**：规范有明确规定 → Agent 自动裁定，引用原文
- **B 型**：规范模糊 → 协商裁定，登记规范改进 TODO
- **C 型**：架构变更 / 删除功能 / 新依赖 → **强制人工介入，任务暂停**

### 3. 记忆优于失忆

每次「继续」都从零解释？我们让 Agent 把状态写入 `.github/memory/agent-system-state.md`，恢复时按协议读取，**像 IDE 关闭重开一样保留现场**。

### 4. 自我演进

每个任务结束后，「提升角色」会反思：

- 是否偏离规范？为什么？
- 是否暴露了文档空白？
- 是否产生了新规则需求？

反思结果累积到 `enhance.md` 的"规范改进待办"，让你的团队规范**随项目一起成长**。

---

## 📚 目录结构

```
.
├── README.md                    ← 你正在看这个
├── README.en.md                 ← English version
├── LICENSE
├── prompts/
│   ├── README.md
│   └── generate-ai-team.prompt.md   ← 🌟 主入口：一键生成
└── templates/
    ├── README.md                    ← 三种模板的对比与选型
    ├── single-file/
    │   ├── README.md
    │   └── copilot-instructions.template.md
    ├── split/
    │   ├── README.md
    │   ├── copilot-instructions.template.md
    │   ├── agent-role-templates.md
    │   └── agent-generator-prompt.md
    └── legacy/
        ├── README.md
        └── ai-team-template.md
```

---

## ❓ FAQ

**Q：和直接写一个 `copilot-instructions.md` 有什么不同？**
A：单文件方案随着规则增多会很快崩溃 —— Agent 要么忽略一半，要么把所有规则都强加到当前任务。我们用「路由 + 角色 + 仲裁」让规范**可扩展、可仲裁、可演进**。

**Q：必须用 Claude / GPT 顶级模型吗？**
A：生成阶段建议用顶级模型（Sonnet 4 / GPT-5 级别），保证推断准确。日常使用阶段，中等模型也能 follow 已生成的规范。

**Q：会增加 Token 消耗吗？**
A：**反而会降低**。路由机制让每次任务只加载相关角色（通常 1~3 个 MD），而不是把整个项目规范塞满 context。

**Q：能用在已有规范的项目里吗？**
A：可以。生成器会读取已有 `copilot-instructions.md` 的内容并合并到第七章。或者你手动把已有规则贴到生成的 `agents/dev.md` 的"项目特定规范"小节即可。

**Q：能不能只用一部分角色？**
A：可以。生成完 `.github/agents/` 后，删除你不需要的角色 MD 文件，并在 `copilot-instructions.md` 的"角色路由表"中删掉对应行即可。

---

## 🛠 进阶玩法

- **Fork 后定制 templates/**：把模板改成你团队的风格，让生成器吐出你团队专属的规范
- **接入 CI**：用 `verify.md` 的检查清单作为 PR 模板，让 reviewer 一项项勾选
- **多项目复用**：把生成好的 `.github/` 沉淀为"组织级模板"，新项目直接 fork 即用
- **跨 Agent 复用**：同一份规范可在 Copilot、Cursor、Cline、Continue、Aider 之间无缝迁移

---

## 🤝 贡献

非常欢迎 Issue / PR：

- 🌐 适配新框架（提供该框架的样例项目和推断规则即可）
- 🎨 优化角色模板（语言更精炼、规则更通用）
- 🐛 报告 Agent 推断失误的真实案例
- 📖 翻译文档到其他语言

提交前请先看 [`CONTRIBUTING.md`](CONTRIBUTING.md)（如未提供则参考 GitHub 通用 PR 流程）。

## 📜 License

MIT © [Contributors](https://github.com/<your-username>/Agents/graphs/contributors)

---

<div align="center">

**如果这个项目帮到了你的团队，请点个 ⭐ Star —— 这是最大的鼓励。**

[⬆ 回到顶部](#-ai-team--一键为你的项目装上九角色协作团队)

</div>
