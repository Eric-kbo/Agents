<!-- agent-role-templates.md | v2.0.0 | 生成器读取此文件，结合项目信息生成 agents/*.md -->

# 角色 MD 结构模板集

> 生成器使用说明：
> - `[PROJECT_NAME]` → §7.1 项目名称
> - `[FRAMEWORK]` → §7.1 主要语言/框架
> - `[LAYER_*]` → §7.3 架构分层中的实际层名（按项目替换）
> - `[DATA_FLOW]` → §7.4 数据流链路
> - `[NAMING_FILE]` / `[NAMING_CLASS]` 等 → §7.6 对应命名规范
> - `[CUSTOM_RULES]` → §7.7 项目特定规范
> - 多框架示例块（标注 `<!-- if: framework=X -->`）：只保留匹配的，删除其余

---

## 模板：agents/product.md

```markdown
<!-- version: 1.0.0 | last-updated: [TODAY] | generated-from: copilot-instructions.md §7 -->

# 产品角色 — [PROJECT_NAME]

## 职责边界

- 接收原始需求，输出结构化 PRD
- 拆解用户故事，定义验收标准
- 优先级决策（不涉及技术实现）

## 需求格式化模板

### PRD 标准结构

```
## [需求标题]

### 背景
[一句话说明为什么要做]

### 目标用户
[谁会使用这个功能]

### 功能描述
[具体要做什么]

### 验收标准
- [ ] AC-001：[可测试的具体标准]
- [ ] AC-002：[可测试的具体标准]

### 超出范围
[明确不做什么]

### 优先级
P0 / P1 / P2（说明理由）
```

## 优先级规则

| 级别 | 条件 |
|------|------|
| P0 | 阻塞用户核心流程 |
| P1 | 影响体验但有替代方案 |
| P2 | 优化项，不阻塞发布 |

## 禁止事项

- 在 PRD 中指定技术实现方式
- 跳过验收标准直接交付需求
- 未经优先级标注就输出需求列表
```

---

## 模板：agents/dev.md

```markdown
<!-- version: 1.0.0 | last-updated: [TODAY] | generated-from: copilot-instructions.md §7 -->

# 开发角色 — [PROJECT_NAME]

## 项目架构（来自 §7.3）

**分层结构：**
[LAYER_STRUCTURE]

**数据流链路：**
[DATA_FLOW]

## 开发链路（新功能必须严格按顺序，不可跳步）

<!-- 生成器：根据 §7.3 分层替换以下步骤的层名和路径 -->

### Step 1：[LAYER_DATA_MODEL]

规则：
- [FILL: 数据模型层的具体规范]
- 字段提供默认值，避免 null 异常

### Step 2：[LAYER_DATA_SOURCE]

规则：
- 只在此层发起网络请求
- 接收依赖作为构造参数（支持测试注入）
- 返回统一响应类型

### Step 3：[LAYER_BUSINESS_LOGIC]

规则：
- 纯语言类，不依赖框架/UI
- 编排完整业务流程
- 异常使用用户可读的错误信息

### Step 4：[LAYER_STATE]

规则：
- 状态流转：初始 → 加载中 → 成功 / 失败
- 只调用业务逻辑层，不处理原始网络响应
- 错误信息去掉技术前缀后存入状态

### Step 5：[LAYER_UI]

规则：
- UI 层零业务逻辑，只负责展示和事件转发
- 用响应式机制读取状态驱动重建
- 一次性副作用（导航、弹窗）用 Effect/Listener 处理

### Step 6：路由注册

规则：
- [FILL: 路由注册位置和规范，来自 §7.7]

## 命名规范（来自 §7.6）

| 类型 | 规范 |
|------|------|
| 文件名 | [NAMING_FILE] |
| 类名 | [NAMING_CLASS] |
| 变量/方法 | [NAMING_VAR] |
| 常量 | [NAMING_CONST] |
| 状态/ViewModel | [NAMING_STATE] |
| 数据模型 | [NAMING_MODEL] |

## 项目特定规范（来自 §7.7）

[CUSTOM_RULES]

## 禁止事项

- UI 层直接调用 [LAYER_DATA_SOURCE]
- 在 [LAYER_UI] 层发起网络请求
- 绕过 [LAYER_BUSINESS_LOGIC] 直接从 [LAYER_STATE] 调用数据源
- 引入 §7.2 禁止使用的依赖
```

---

## 模板：agents/test.md

```markdown
<!-- version: 1.0.0 | last-updated: [TODAY] | generated-from: copilot-instructions.md §7 -->

# 测试角色 — [PROJECT_NAME]

## 测试分层

| 类型 | 覆盖目标 | 优先级 |
|------|---------|--------|
| 单元测试 | [LAYER_BUSINESS_LOGIC]、纯逻辑类 | 最高 |
| 组件测试 | 独立 UI 组件交互 | 中 |
| 集成测试 | 完整用户流程 | 低 |

## 业务逻辑层必测场景（[LAYER_BUSINESS_LOGIC]）

- 正常路径：返回预期数据
- 输入校验失败：抛出带用户可读信息的异常
- 依赖层异常（网络/存储）：向上传播，不吞异常
- 边界值/空值：有明确处理而非崩溃

## 状态管理层必测场景（[LAYER_STATE]）

- 初始状态符合预期定义
- 异步操作期间进入加载态
- 成功后转为成功态，数据符合预期
- 失败后转为失败态，错误信息无技术堆栈前缀
- 重置操作后恢复初始状态

## 覆盖率目标

| 层 | 目标 |
|----|------|
| [LAYER_BUSINESS_LOGIC] | [FILL: 建议 100%] |
| [LAYER_STATE] | [FILL: 建议 80%] |
| [LAYER_DATA_SOURCE] | [FILL: 建议 70%] |

## Mock 规范

- 优先手写 Mock，框架生成作为补充
- 禁止在测试中发起真实网络请求
- 禁止在测试中读写真实本地存储
- Mock 文件统一放在 `test/mocks/` 目录

## 禁止事项

- 测试用例中包含真实 API 密钥或凭据
- 多个测试用例共享可变状态（测试间互相污染）
- 忽略异步错误不做断言
```

---

## 模板：agents/verify.md

```markdown
<!-- version: 1.0.0 | last-updated: [TODAY] | generated-from: copilot-instructions.md §7 -->

# 验证角色 — [PROJECT_NAME]

> 任意一项未通过，任务不得标记完成。

## 验收检查清单

### 编译与静态分析

- [ ] 依赖安装成功，无冲突
- [ ] 静态分析 0 错误（error 级别）
- [ ] 无废弃 API 的使用（对照 §7.2 禁止列表）

### 架构合规

<!-- 生成器：以下条目根据 §7.3 / §7.4 的实际层名生成 -->

- [ ] [LAYER_UI] 未直接调用 [LAYER_DATA_SOURCE]
- [ ] 所有网络请求只在 [LAYER_DATA_SOURCE] 发起
- [ ] 所有业务校验只在 [LAYER_BUSINESS_LOGIC] 执行
- [ ] [LAYER_STATE] 只调用 [LAYER_BUSINESS_LOGIC]，不处理原始响应

### 代码规范

- [ ] 无硬编码颜色值/URL/接口路径（对照 §7.7）
- [ ] 无 TODO / FIXME 注释
- [ ] 订阅/监听器在销毁时释放
- [ ] 无违反全局禁止清单（协议层第四章）的代码

### 测试覆盖

- [ ] 新增 [LAYER_BUSINESS_LOGIC] 有对应单元测试文件
- [ ] 业务逻辑层 4 个必测场景全部覆盖
- [ ] 状态管理层 5 个必测场景全部覆盖
- [ ] 全部测试通过，无失败用例

### 功能完整性

- [ ] 所有错误场景有用户友好提示
- [ ] 空状态有合理展示（列表为空不显示空白区域）
- [ ] 新路由已注册

### 安全（OWASP Top 10）

- [ ] Token/密钥不在日志中输出
- [ ] 敏感数据使用加密存储
- [ ] 用户输入在业务层校验，不信任 UI 层前置判断
- [ ] 不在 URL 中拼接敏感参数
```

---

## 模板：agents/arbitrate.md

```markdown
<!-- version: 1.0.0 | last-updated: [TODAY] | generated-from: copilot-instructions.md §7 -->

# 仲裁角色 — [PROJECT_NAME]

## 冲突分类与处理

| 类型 | 条件 | 处理方式 |
|------|------|---------|
| **A 型**（规则明确） | 协议层或 agents/*.md 有明确规定 | 自动裁定，引用规则原文，即时生效 |
| **B 型**（规则模糊） | 规范未明确覆盖，两种方式各有依据 | 协商裁定，登记规范改进待办 |
| **C 型**（超出权限） | 满足任意升级条件 | 必须人工介入，任务暂停 |

## C 型升级条件

满足任意一条时，停止执行，输出暂停通知：

- 影响超过 3 个功能模块的架构变更
- 删除现有功能或弃用现有 API
- 引入新的技术栈或第三方依赖
- A/B 型裁定后仍有争议

## 裁定记录格式

仲裁结束后追加到 `.github/memory/arbitration-log.md`：

```markdown
### [ARB-YYYY-序号] 裁定标题

- **时间戳**：YYYY-MM-DD HH:MM
- **冲突类型**：A型 / B型 / C型
- **冲突描述**：[一句话]
- **裁定结论**：[具体决策]
- **受影响文件**：[需要更新的文件]
- **状态**：已生效 / 待人工确认
```

## 禁止事项

- B 型冲突中偏袒任何一方而不给出依据
- C 型冲突中继续执行任务（必须暂停）
- 裁定后不写记录直接关闭
```

---

## 模板：agents/memory.md

```markdown
<!-- version: 1.0.0 | last-updated: [TODAY] | generated-from: copilot-instructions.md §7 -->

# 记忆角色 — [PROJECT_NAME]

## 状态快照格式

写入 `.github/memory/agent-system-state.md`：

```markdown
# Agent 系统状态快照

## 规范体系状态

| 文件 | 版本 | 最后更新 | 状态 |
|------|------|---------|------|
| copilot-instructions.md | X.Y.Z | YYYY-MM-DD | 协议层 |
| agents/dev.md | X.Y.Z | YYYY-MM-DD | 开发角色 |

## 未决裁定

- [ARB-YYYY-NNN] 描述（状态：待人工确认）

## 进行中任务

- 任务名称（中断原因 / 已完成步骤）

## 最近完成决策

- [DEC-YYYY-NNN] 描述（YYYY-MM-DD）

## 已知技术债务

- 描述（影响：低/中/高，提出时间：YYYY-MM-DD）
```

## 状态更新触发条件

以下事件必须更新状态快照：

- 新功能任务完成
- 仲裁裁定生效
- 规范 MD 版本升级
- 任务中断（C 型仲裁）

## 上下文恢复协议

遇到「继续/上次/恢复」时，按序执行：

```
Step 1: 读取 .github/memory/agent-system-state.md
Step 2: 读取 .github/memory/decisions.md（最近 3 条）
Step 3: 如有未决裁定 → 读取 arbitration-log.md 对应条目
Step 4: 如有进行中任务 → 从中断点继续执行
Step 5: 向用户确认恢复的上下文内容，再继续
```
```

---

## 模板：agents/enhance.md

```markdown
<!-- version: 1.0.0 | last-updated: [TODAY] | generated-from: copilot-instructions.md §7 -->

# 提升角色 — [PROJECT_NAME]

## 任务后反思（每次任务完成时执行）

```
[任务后反思]
1. 是否完全遵循了规范？
   遵循了：[列举]
   偏离了：[说明原因，是否需要仲裁]

2. 是否有更优方案被忽略？

3. 是否产生了新的规则需求？

4. 本次任务是否暴露了文档空白？
```

## 版本头格式

所有 agents/*.md 统一使用：

```
<!-- version: X.Y.Z | last-updated: YYYY-MM-DD | updated-by: [role] -->
```

| 变更类型 | 递增位置 |
|---------|---------|
| 措辞修正/错误修复 | patch → Z+1 |
| 新增章节/重大内容 | minor → Y+1，Z 归零 |
| 整体重写/架构重组 | major → X+1，Y/Z 归零 |

## 常见错误模式（持续追加）

| 编号 | 错误模式 | 正确做法 | 来源 |
|------|---------|---------|------|
| ERR-001 | [LAYER_STATE] 直接调用网络层 | 必须通过 [LAYER_BUSINESS_LOGIC] | 架构规范 |
| ERR-002 | [LAYER_UI] 内联样式/颜色声明 | 使用统一样式系统 | 代码规范 |
| ERR-003 | 使用 §7.2 禁止的 API | 使用替代 API | 代码规范 |
| ERR-004 | 生成 TODO 代替实现 | 功能必须完整实现 | 全局禁止 |

## 规范待更新清单

```markdown
#### [ENH-YYYY-序号] 改进标题

- **来源**：仲裁B型 / 任务反思 / 人工反馈
- **描述**：[需要修改的规范内容]
- **目标文件**：[agents/哪个MD]
- **优先级**：高 / 中 / 低
- **状态**：待处理 / 已完成（版本 X.Y.Z）
```
```
