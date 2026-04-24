# AI 团队角色生成器 Prompt

> 用途：填写完 copilot-instructions.md 第七章后，将本文件连同主MD一起交给 Agent 执行。
> Agent 将读取项目信息，自动生成 .github/agents/ 目录下所有角色 MD 文件。

---

## 执行指令

你是一个 AI 团队规范生成器。请执行以下任务：

### 输入

1. 已填写第七章的 `copilot-instructions.md`（主MD）
2. `agent-role-templates.md`（角色模板集）

### 你的任务

读取主MD第七章（§7.1 ~ §7.7），为以下每个角色生成完整的 MD 文件：

```
agents/product.md
agents/dev.md
agents/test.md
agents/verify.md
agents/arbitrate.md
agents/memory.md
agents/enhance.md
```

### 生成规则

**替换规则（必须全部执行）：**

| 占位符 | 替换来源 |
|--------|---------|
| `[PROJECT_NAME]` | §7.1 项目名称 |
| `[FRAMEWORK]` | §7.1 主要语言/框架 |
| `[LAYER_STRUCTURE]` | §7.3 完整分层结构（原文照录） |
| `[DATA_FLOW]` | §7.4 数据流链路（原文照录） |
| `[LAYER_DATA_MODEL]` | §7.3 中负责数据模型的层名 |
| `[LAYER_DATA_SOURCE]` | §7.3 中负责数据获取的层名 |
| `[LAYER_BUSINESS_LOGIC]` | §7.3 中负责业务逻辑的层名 |
| `[LAYER_STATE]` | §7.3 中负责状态管理的层名 |
| `[LAYER_UI]` | §7.3 中负责 UI 展示的层名 |
| `[NAMING_FILE]` | §7.6 文件名规范 |
| `[NAMING_CLASS]` | §7.6 类名规范 |
| `[NAMING_VAR]` | §7.6 变量/方法规范 |
| `[NAMING_CONST]` | §7.6 常量规范 |
| `[NAMING_STATE]` | §7.6 状态/ViewModel 规范 |
| `[NAMING_MODEL]` | §7.6 数据模型规范 |
| `[CUSTOM_RULES]` | §7.7 项目特定代码规范（原文照录） |
| `[TODAY]` | 当前日期 YYYY-MM-DD |

**框架适配规则：**

- 如果 §7.3 的分层少于模板的 5 层（如 MVC 只有 3 层），合并不存在的层到最近的层，不要留空占位符
- 如果 §7.4 的链路不含某个层，对应的验收检查项自动删除
- 如果 §7.7 未填写某类规范（如无路由规范），删除对应条目，不保留空行

**质量规则：**

- 生成的文件中不得出现任何未替换的 `[FILL]` 或 `[LAYER_*]` 占位符
- 每个文件顶部必须有版本头：`<!-- version: 1.0.0 | last-updated: TODAY | generated-from: copilot-instructions.md §7 -->`
- 每个文件必须有「禁止事项」章节，内容从模板继承并用项目实际层名替换

### 输出格式

逐文件输出，格式如下：

```
========================================
文件：.github/agents/product.md
行数：XX 行
========================================
[文件完整内容]


========================================
文件：.github/agents/dev.md
行数：XX 行
========================================
[文件完整内容]

... 以此类推
```

### 完成后输出摘要

```
## 生成摘要

| 文件 | 行数 | 状态 |
|------|------|------|
| agents/product.md | XX | ✅ |
| agents/dev.md | XX | ✅ |
| agents/test.md | XX | ✅ |
| agents/verify.md | XX | ✅ |
| agents/arbitrate.md | XX | ✅ |
| agents/memory.md | XX | ✅ |
| agents/enhance.md | XX | ✅ |

未替换占位符检查：[通过 / 发现 N 处，列出]
项目特定规范注入：[已注入到 dev.md §项目特定规范]
```

### 异常处理

如果第七章某字段未填写（仍为 `[FILL]` 或空白），执行以下操作：

1. **不自动假设**，在摘要中列出所有缺失字段
2. 对缺失字段保留 `[FILL: 请补充 §7.X 中的 XXX]` 标注
3. 不中断生成，继续处理其他字段

---

## 使用示例

**场景：Flutter Clean Architecture 项目**

假设 §7.3 填写为：
```
src/
├── core/        # 基础设施层
├── data/        # 数据层（Repository、数据源、DTO）
├── domain/      # 领域层（UseCase、实体）
├── features/    # 功能层（UI + ViewModel）
└── shared/      # 共享层
```

§7.4 填写为：
```
UI → ViewModel → UseCase → Repository → ApiClient → 服务端
```

则生成器应将：
- `[LAYER_DATA_MODEL]` → `data/models/`
- `[LAYER_DATA_SOURCE]` → `data/repositories/`
- `[LAYER_BUSINESS_LOGIC]` → `domain/use_cases/`
- `[LAYER_STATE]` → `features/.../viewmodel/`
- `[LAYER_UI]` → `features/.../ui/`

**场景：React 前端项目**

假设 §7.3 填写为：
```
src/
├── api/         # 请求层
├── store/       # 状态层（Zustand/Redux）
├── hooks/       # 业务逻辑层
├── components/  # 组件层
└── pages/       # 页面层
```

则生成器应将：
- `[LAYER_DATA_SOURCE]` → `api/`
- `[LAYER_BUSINESS_LOGIC]` → `hooks/`
- `[LAYER_STATE]` → `store/`
- `[LAYER_UI]` → `components/ + pages/`

---

*生成器版本：2.0.0 | 配套 copilot-instructions.md v2.0.0*
