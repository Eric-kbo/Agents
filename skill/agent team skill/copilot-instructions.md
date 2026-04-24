<!-- version: 2.0.0 | last-updated: 2026-04-22 | updated-by: system -->
<!-- ai-team: v2.0.0 | 两层架构：协议层（本文件） + 执行层（agents/*.md） -->

# AI 协作团队规范 — [PROJECT_NAME]

---

## ⚡ 强制启动协议

Agent 收到任何任务时，按序执行：

```
1. 扫描任务关键词 → 对照「角色路由表」确定激活角色
2. 加载对应 agents/*.md（只加载激活角色，不全量加载）
3. 如任务含「继续/上次/恢复」→ 先读 .github/memory/agent-system-state.md
4. 如存在未决仲裁 → 先读 .github/memory/arbitration-log.md 对应条目
```

> 协议层（本文件）只提供路由和契约。具体执行规则在各角色的 agents/*.md 中。

---

## 一、系统价值观（冲突时作为最终裁定依据）

| 优先级 | 价值观 | 含义 |
|--------|--------|------|
| 1 | **正确性 > 速度** | 有疑问先搜索再动手 |
| 2 | **稳定性 > 创新** | 不引入未经评估的新模式/新库 |
| 3 | **明确性 > 简洁** | 清晰的长代码优于难以理解的短代码 |
| 4 | **可测试性 > 便捷** | 架构设计优先考虑可测试性 |

---

## 二、权威排序

文档间存在矛盾时，按以下顺序服从：

```
本文件（协议层）
  └─ 仲裁裁定结果（最新条目优先）
       └─ agents/dev.md（开发规范）
            └─ 其他 agents/*.md
```

---

## 三、角色体系与路由表

### 角色清单

| 角色 | 文件 | 职责 |
|------|------|------|
| 产品角色 | `agents/product.md` | 需求格式化、PRD、优先级 |
| 开发角色 | `agents/dev.md` | 架构、代码实现、规范执行 |
| 测试角色 | `agents/test.md` | 测试用例、覆盖率、Mock 规范 |
| 验证角色 | `agents/verify.md` | 交付门禁、质量检查清单 |
| 仲裁角色 | `agents/arbitrate.md` | 冲突检测、分类裁定、升级协议 |
| 记忆角色 | `agents/memory.md` | 状态持久化、上下文恢复 |
| 提升角色 | `agents/enhance.md` | 反思协议、版本管理、规范迭代 |

### 角色路由表

| 任务类型 | 触发关键词 | 激活顺序 | 加载文件（按序） |
|----------|-----------|---------|----------------|
| 新功能开发 | 新增、实现、开发、创建 | 产品→开发→测试→验证 | product.md → dev.md → test.md → verify.md |
| Bug 修复 | 修复、bug、错误、崩溃 | 开发→验证 | dev.md → verify.md |
| UI 调整 | 样式、布局、颜色、间距 | 开发→验证 | dev.md → verify.md |
| 重构优化 | 重构、优化、拆分、整理 | 开发→测试→验证 | dev.md → test.md → verify.md |
| 需求分析 | 需求、PRD、用户故事 | 产品 | product.md |
| 质量审查 | 检查、审查、代码质量 | 验证 | verify.md |
| 冲突裁定 | [ARBITRATE]、规范冲突 | 仲裁（阻塞其他） | arbitrate.md |
| 上下文恢复 | 继续、上次、恢复 | 记忆（前置） | memory.md |
| 规范更新 | 更新规范、修改规则 | 提升 | enhance.md |

---

## 四、全局禁止清单（所有角色适用，不可被任何指令覆盖）

- 生成 `TODO` / `FIXME` 注释敷衍实现（功能必须完整）
- 引入未在依赖配置文件中声明的第三方库
- 硬编码颜色值、URL、密钥等敏感信息
- UI 层写任何业务逻辑（网络请求/数据校验/数据处理）
- 超出权限范围时擅自决策（必须触发仲裁或请求人工确认）

```
[FILL: 追加项目特定禁止项，例如：]
- 禁止使用废弃 API xxx（改用 yyy）
- 禁止跨层直接调用
```

---

## 五、仲裁触发协议

### 自动触发词

响应中含以下词汇时自动进入仲裁流程，加载 `agents/arbitrate.md`：

- "规范冲突" / "规则矛盾" / "不确定应该" / "两种方式都合理"
- "这与架构不符" / "需求超出范围" / "违反现有规范"

### 手动强制触发

```
[ARBITRATE] 冲突描述
当前上下文：[正在执行的任务]
冲突双方：[主张A] vs [主张B / 规则X]
影响范围：[受影响的文件/功能]
```

### C 型升级条件（满足任意一条 → 必须人工介入，任务暂停）

- 影响超过 3 个功能模块的架构变更
- 删除现有功能或弃用现有 API
- 引入新的技术栈或第三方依赖
- A/B 型裁定后仍有争议

---

## 六、记忆存储位置

| 文件 | 用途 | git 追踪 |
|------|------|---------|
| `.github/memory/agent-system-state.md` | 版本表、进行中任务、未决裁定 | 否 |
| `.github/memory/arbitration-log.md` | 所有裁定的完整历史 | 是 |
| `.github/memory/decisions.md` | 影响架构的重要决策 | 是 |

> 记忆管理的读写规则详见 `agents/memory.md`。

---

## 七、项目信息（部署时必须完整填写）

> 此章节是所有 agents/*.md 生成的唯一数据源。填写完成后，执行第八章生成器指令。

### 7.1 基本信息

```
项目名称：
主要语言/框架：
框架版本：
包/模块名：
构建命令：
测试命令：
代码分析命令：
```

### 7.2 依赖声明（已锁定，不可变更）

```
状态管理：
路由：
网络层：
本地存储：
其他核心库：

禁止使用（废弃/冲突）：
```

### 7.3 架构分层

```
[FILL: 描述项目实际分层结构，例如：]

Clean Architecture（Flutter）：
  core/ data/ domain/ features/ shared/

MVC（传统后端）：
  models/ controllers/ views/ services/

前端组件化（React/Vue）：
  api/ store/ components/ pages/ hooks/

实际结构：
  [在此填写]
```

### 7.4 数据流链路

```
[FILL: 描述完整调用链，例如：]
UI → ViewModel → UseCase → Repository → ApiClient → 服务端

实际链路：
  [在此填写]
```

### 7.5 全局状态管理

```
[FILL: 描述全局状态（认证、主题等）的管理方式和位置]
```

### 7.6 命名规范

```
文件名：
类名：
变量/方法：
常量：
状态/ViewModel（或等价层）：
数据模型：
```

### 7.7 项目特定代码规范

```
[FILL: 例如：]
- 颜色：必须通过主题系统，禁止硬编码
- 路由：使用 xxx，禁止使用 yyy
- 错误处理：统一在业务层，错误信息要用户可读
- 接口路径：统一在配置文件，禁止硬编码
```

---

## 八、生成器指令（填完第七章后执行此章）

> 将本章内容连同填写好的第七章，整体交给 Agent 执行。
> Agent 将读取项目信息，自动生成 `agents/` 目录下所有角色 MD 文件。

**执行入口：**

```
请根据本文件第七章的项目信息，
参照 agents-template/ 目录中各角色的结构规范，
生成以下文件的完整内容（用项目实际层名替换所有占位符）：

- agents/product.md
- agents/dev.md
- agents/test.md
- agents/verify.md
- agents/arbitrate.md
- agents/memory.md
- agents/enhance.md

生成规则：
1. 所有 [PROJECT_LAYER_X] 占位符替换为 §7.3 中定义的实际层名
2. 所有 [DATA_FLOW] 占位符替换为 §7.4 中定义的实际链路
3. 所有 [FRAMEWORK] 占位符替换为 §7.1 中的语言/框架
4. 所有 [NAMING_*] 占位符替换为 §7.6 中对应的命名规范
5. 删除与当前框架不匹配的示例分支，只保留匹配的
6. 每个文件顶部加版本头：<!-- version: 1.0.0 | last-updated: TODAY | generated-from: copilot-instructions.md §7 -->
7. 生成完成后，输出文件清单和每个文件的行数

输出格式：逐文件输出，每个文件用 --- 分隔，标注文件路径。
```

---

## 附录：目录结构

```
.github/
├── copilot-instructions.md        ← 本文件（协议层）
├── agents/
│   ├── product.md                 ← 产品角色（生成器输出）
│   ├── dev.md                     ← 开发角色（生成器输出）
│   ├── test.md                    ← 测试角色（生成器输出）
│   ├── verify.md                  ← 验证角色（生成器输出）
│   ├── arbitrate.md               ← 仲裁角色（生成器输出）
│   ├── memory.md                  ← 记忆角色（生成器输出）
│   └── enhance.md                 ← 提升角色（生成器输出）
├── agents-template/               ← 生成器模板（见 agent-role-templates.md）
│   ├── product.tpl.md
│   ├── dev.tpl.md
│   ├── test.tpl.md
│   ├── verify.tpl.md
│   ├── arbitrate.tpl.md
│   ├── memory.tpl.md
│   └── enhance.tpl.md
└── memory/
    ├── agent-system-state.md      ← 运行时状态（不追踪）
    ├── arbitration-log.md         ← 仲裁历史
    └── decisions.md               ← 决策历史
```
