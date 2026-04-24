<!-- version: 2.0.0 | last-updated: 2026-04-22 | updated-by: system -->
<!-- ai-team: v2.0.0 | 单文件入口：协议层 + 生成器 -->
<!-- 使用方式：把本文件放入项目根目录，交给 Agent 执行「附录：生成器指令」即可 -->

# AI 协作团队规范 — [PROJECT_NAME]

---

## ⚡ 强制启动协议

Agent 收到任何任务时，按序执行：

```
1. 扫描任务关键词 → 对照「角色路由表」确定激活角色
2. 加载对应 .github/agents/*.md（只加载激活角色）
3. 如任务含「继续/上次/恢复」→ 先读 .github/memory/agent-system-state.md
4. 如存在未决仲裁 → 先读 .github/memory/arbitration-log.md 对应条目
```

---

## 一、系统价值观

| 优先级 | 价值观 | 含义 |
|--------|--------|------|
| 1 | **正确性 > 速度** | 有疑问先搜索再动手 |
| 2 | **稳定性 > 创新** | 不引入未经评估的新模式/新库 |
| 3 | **明确性 > 简洁** | 清晰的长代码优于难以理解的短代码 |
| 4 | **可测试性 > 便捷** | 架构设计优先考虑可测试性 |

---

## 二、权威排序

```
本文件（协议层）
  └─ 仲裁裁定结果（最新条目优先）
       └─ .github/agents/dev.md
            └─ 其他 .github/agents/*.md
```

---

## 三、角色路由表

| 任务类型 | 触发关键词 | 激活顺序 | 加载文件 |
|----------|-----------|---------|---------|
| 新功能开发 | 新增、实现、开发、创建 | 产品→开发→测试→验证 | product.md → dev.md → test.md → verify.md |
| Bug 修复 | 修复、bug、错误、崩溃 | 开发→验证 | dev.md → verify.md |
| UI 调整 | 样式、布局、颜色、间距 | 开发→验证 | dev.md → verify.md |
| 重构优化 | 重构、优化、拆分、整理 | 开发→测试→验证 | dev.md → test.md → verify.md |
| 需求分析 | 需求、PRD、用户故事 | 产品 | product.md |
| 质量审查 | 检查、审查、代码质量 | 验证 | verify.md |
| 冲突裁定 | [ARBITRATE]、规范冲突 | 仲裁（阻塞） | arbitrate.md |
| 上下文恢复 | 继续、上次、恢复 | 记忆（前置） | memory.md |
| 规范更新 | 更新规范、修改规则 | 提升 | enhance.md |

---

## 四、全局禁止清单

- 生成 `TODO` / `FIXME` 注释敷衍实现
- 引入未在依赖配置文件中声明的第三方库
- 硬编码颜色值、URL、密钥等敏感信息
- UI 层写任何业务逻辑
- 超出权限范围时擅自决策

---

## 五、仲裁触发协议

**自动触发词：** "规范冲突" / "规则矛盾" / "不确定应该" / "两种方式都合理" / "这与架构不符"

**手动触发：**
```
[ARBITRATE] 冲突描述
当前上下文：[正在执行的任务]
冲突双方：[主张A] vs [主张B]
影响范围：[受影响的文件/功能]
```

**C 型升级条件（满足任意一条 → 人工介入，任务暂停）：**
- 影响超过 3 个功能模块的架构变更
- 删除现有功能或弃用现有 API
- 引入新的技术栈或第三方依赖

---

## 六、记忆存储

| 文件 | 用途 | git 追踪 |
|------|------|---------|
| `.github/memory/agent-system-state.md` | 运行时状态、进行中任务 | 否 |
| `.github/memory/arbitration-log.md` | 仲裁历史 | 是 |
| `.github/memory/decisions.md` | 重要架构决策 | 是 |

---

## 七、项目信息

> 可以不填。留空时，生成器会自动扫描项目文件推断并回填此章节。

```yaml
项目名称:
主要语言/框架:
框架版本:
构建命令:
测试命令:
代码分析命令:

依赖_状态管理:
依赖_路由:
依赖_网络层:
依赖_本地存储:
依赖_其他:
依赖_禁止使用:

架构分层: |
  # 示例（删除并填入实际结构）：
  # src/
  # ├── core/
  # ├── data/
  # ├── domain/
  # └── features/

数据流链路:
  # 示例：UI → ViewModel → UseCase → Repository → ApiClient

全局状态管理:

命名规范_文件名:
命名规范_类名:
命名规范_变量方法:
命名规范_常量:
命名规范_状态层:
命名规范_数据模型:

项目特定规范: |
  # 示例：
  # - 颜色必须通过主题系统
  # - 路由使用 go_router，禁止 Navigator.push
```

---

---

# 附录：生成器指令

> **触发方式：** 对 Agent 说「执行生成器」或「生成 agents 目录」。
> Agent 将读取第七章 + 扫描项目，自动生成 `.github/agents/` 下所有角色 MD 文件。

---

## 生成器执行步骤

### Step 0：信息收集

按优先级获取项目信息：

```
优先级 1：读取本文件第七章已填写的字段（非空即用）
优先级 2：扫描以下文件推断未填字段：

  框架/依赖推断：
    pubspec.yaml         → Flutter 项目，提取 dependencies
    package.json         → Node/React/Vue 项目，提取 dependencies + scripts
    pom.xml / build.gradle → Java/Kotlin 项目
    requirements.txt / pyproject.toml → Python 项目
    go.mod               → Go 项目
    Cargo.toml           → Rust 项目

  分层推断：
    扫描 src/ lib/ app/ 一级目录结构
    识别常见模式：core/data/domain/features → Clean Architecture
                  models/controllers/views  → MVC
                  store/components/pages    → 前端组件化
                  internal/pkg/cmd          → Go 标准结构

  命名规范推断：
    随机抽取 5 个已有源码文件，分析：
      文件名格式（snake_case / camelCase / PascalCase / kebab-case）
      类名格式
      变量命名格式

  路由规范推断：
    搜索 router / routes / navigation 相关文件
    识别路由库（go_router / react-router / vue-router 等）

优先级 3：使用框架默认最佳实践填充仍为空的字段
```

收集完成后：
1. 将推断结果回填到本文件第七章对应字段（替换注释占位符）
2. 在输出摘要中标注每个字段的来源（已填/推断/默认）

---

### Step 1：提取层名映射

根据收集到的架构分层，建立层名映射表：

```
从分层结构中识别并映射：
  DATA_MODEL_LAYER    → 负责数据模型/DTO 的层
  DATA_SOURCE_LAYER   → 负责数据获取/网络请求的层
  BUSINESS_LAYER      → 负责业务逻辑/用例的层
  STATE_LAYER         → 负责状态管理的层
  UI_LAYER            → 负责 UI 展示的层

如果某个层在当前架构中不存在（如 MVC 无独立 UseCase 层），
将该职责合并到最近的层，不创建不存在的层名。
```

---

### Step 2：生成 7 个角色 MD 文件

对每个文件，用 Step 1 的映射替换所有 `{{占位符}}`，生成完整内容。

#### 文件 1：`.github/agents/product.md`

```markdown
<!-- version: 1.0.0 | last-updated: {{TODAY}} | project: {{PROJECT_NAME}} -->

# 产品角色 — {{PROJECT_NAME}}

## 职责边界
- 接收原始需求，输出结构化 PRD
- 拆解用户故事，定义验收标准
- 优先级决策（不涉及技术实现）

## PRD 标准结构

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

### 优先级：P0 / P1 / P2（说明理由）
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

#### 文件 2：`.github/agents/dev.md`

```markdown
<!-- version: 1.0.0 | last-updated: {{TODAY}} | project: {{PROJECT_NAME}} -->

# 开发角色 — {{PROJECT_NAME}}

## 项目架构

**框架：** {{FRAMEWORK}}
**分层结构：**
{{LAYER_STRUCTURE}}

**数据流链路：**
{{DATA_FLOW}}

## 开发链路（新功能必须严格按顺序）

### Step 1：{{DATA_MODEL_LAYER}}（数据模型）
- 请求参数类提供序列化方法
- 响应类提供反序列化工厂
- 字段提供默认值，避免 null 异常

### Step 2：{{DATA_SOURCE_LAYER}}（数据获取）
- 只在此层发起网络请求
- 接收依赖项作为构造参数（支持测试注入）
- 返回统一响应类型

### Step 3：{{BUSINESS_LAYER}}（业务逻辑）
- 纯逻辑类，不依赖框架/UI
- 编排完整业务流程：请求 → 校验 → 返回
- 异常使用用户可读的错误信息

### Step 4：{{STATE_LAYER}}（状态管理）
- 状态流转：初始态 → 加载中 → 成功 / 失败
- 只调用 {{BUSINESS_LAYER}}，不处理原始网络响应
- 错误信息去掉技术前缀后存入状态

### Step 5：{{UI_LAYER}}（UI 展示）
- UI 层零业务逻辑，只负责展示和事件转发
- 用响应式机制读取状态驱动重建
- 一次性副作用（导航、弹窗）用监听器处理

### Step 6：路由注册
{{ROUTING_RULES}}

## 命名规范

| 类型 | 规范 |
|------|------|
| 文件名 | {{NAMING_FILE}} |
| 类名 | {{NAMING_CLASS}} |
| 变量/方法 | {{NAMING_VAR}} |
| 常量 | {{NAMING_CONST}} |
| 状态层 | {{NAMING_STATE}} |
| 数据模型 | {{NAMING_MODEL}} |

## 项目特定规范
{{CUSTOM_RULES}}

## 禁止事项
- {{UI_LAYER}} 直接调用 {{DATA_SOURCE_LAYER}}
- 在 {{UI_LAYER}} 发起网络请求
- 绕过 {{BUSINESS_LAYER}} 从 {{STATE_LAYER}} 直接调用数据层
- 引入依赖声明中禁止使用的库
```

---

#### 文件 3：`.github/agents/test.md`

```markdown
<!-- version: 1.0.0 | last-updated: {{TODAY}} | project: {{PROJECT_NAME}} -->

# 测试角色 — {{PROJECT_NAME}}

## 测试分层

| 类型 | 覆盖目标 | 优先级 |
|------|---------|--------|
| 单元测试 | {{BUSINESS_LAYER}}、纯逻辑类 | 最高 |
| 组件测试 | 独立 UI 组件交互 | 中 |
| 集成测试 | 完整用户流程 | 低 |

## {{BUSINESS_LAYER}} 必测场景

- 正常路径：返回预期数据
- 输入校验失败：抛出带用户可读信息的异常
- 依赖层异常（网络/存储）：向上传播，不吞异常
- 边界值/空值：有明确处理而非崩溃

## {{STATE_LAYER}} 必测场景

- 初始状态符合预期定义
- 异步操作期间进入加载态
- 成功后转为成功态，数据符合预期
- 失败后转为失败态，错误信息无技术堆栈前缀
- 重置后恢复初始状态

## 覆盖率目标

| 层 | 目标 |
|----|------|
| {{BUSINESS_LAYER}} | 100% |
| {{STATE_LAYER}} | 80% |
| {{DATA_SOURCE_LAYER}} | 70% |

## Mock 规范

- 禁止在测试中发起真实网络请求
- 禁止在测试中读写真实本地存储
- Mock 文件统一放在 `test/mocks/` 目录

## 禁止事项
- 测试用例中包含真实 API 密钥或凭据
- 多个测试用例共享可变状态（测试间互相污染）
- 忽略异步错误不做断言
```

---

#### 文件 4：`.github/agents/verify.md`

```markdown
<!-- version: 1.0.0 | last-updated: {{TODAY}} | project: {{PROJECT_NAME}} -->

# 验证角色 — {{PROJECT_NAME}}

> 任意一项未通过，任务不得标记完成。

## 编译与静态分析
- [ ] 依赖安装成功，无冲突
- [ ] 静态分析 0 错误（error 级别）
- [ ] 无废弃 API 的使用

## 架构合规
- [ ] {{UI_LAYER}} 未直接调用 {{DATA_SOURCE_LAYER}}
- [ ] 所有网络请求只在 {{DATA_SOURCE_LAYER}} 发起
- [ ] 所有业务校验只在 {{BUSINESS_LAYER}} 执行
- [ ] {{STATE_LAYER}} 只调用 {{BUSINESS_LAYER}}，不处理原始响应

## 代码规范
- [ ] 无硬编码颜色值/URL/接口路径
- [ ] 无 TODO / FIXME 注释
- [ ] 订阅/监听器在销毁时释放
- [ ] 无违反全局禁止清单的代码

## 测试覆盖
- [ ] 新增 {{BUSINESS_LAYER}} 有对应单元测试文件
- [ ] {{BUSINESS_LAYER}} 4 个必测场景全部覆盖
- [ ] {{STATE_LAYER}} 5 个必测场景全部覆盖
- [ ] 全部测试通过，无失败用例

## 功能完整性
- [ ] 所有错误场景有用户友好提示
- [ ] 空状态有合理展示
- [ ] 新路由已注册

## 安全（OWASP Top 10）
- [ ] Token/密钥不在日志中输出
- [ ] 敏感数据使用加密存储
- [ ] 用户输入在业务层校验，不信任 UI 层前置判断
- [ ] 不在 URL 中拼接敏感参数
```

---

#### 文件 5：`.github/agents/arbitrate.md`

```markdown
<!-- version: 1.0.0 | last-updated: {{TODAY}} | project: {{PROJECT_NAME}} -->

# 仲裁角色 — {{PROJECT_NAME}}

## 冲突分类与处理

| 类型 | 条件 | 处理方式 |
|------|------|---------|
| A 型（规则明确） | 协议层或 agents/*.md 有明确规定 | 自动裁定，引用规则原文 |
| B 型（规则模糊） | 规范未明确覆盖 | 协商裁定，登记改进待办 |
| C 型（超出权限） | 满足任意升级条件 | 人工介入，任务暂停 |

## C 型升级条件
- 影响超过 3 个功能模块的架构变更
- 删除现有功能或弃用现有 API
- 引入新的技术栈或第三方依赖
- A/B 型裁定后仍有争议

## 裁定记录格式

追加到 `.github/memory/arbitration-log.md`：

```
### [ARB-YYYY-序号] 裁定标题
- 时间戳：YYYY-MM-DD HH:MM
- 冲突类型：A型 / B型 / C型
- 冲突描述：[一句话]
- 裁定结论：[具体决策]
- 受影响文件：[文件列表]
- 状态：已生效 / 待人工确认
```

## 禁止事项
- B 型冲突中不给依据直接偏袒一方
- C 型冲突中继续执行任务
- 裁定后不写记录
```

---

#### 文件 6：`.github/agents/memory.md`

```markdown
<!-- version: 1.0.0 | last-updated: {{TODAY}} | project: {{PROJECT_NAME}} -->

# 记忆角色 — {{PROJECT_NAME}}

## 状态快照格式

写入 `.github/memory/agent-system-state.md`：

```
# Agent 系统状态快照

## 规范版本
| 文件 | 版本 | 最后更新 |
|------|------|---------|
| copilot-instructions.md | X.Y.Z | YYYY-MM-DD |
| agents/dev.md | X.Y.Z | YYYY-MM-DD |

## 未决裁定
- [ARB-YYYY-NNN] 描述（待人工确认）

## 进行中任务
- 任务名称（中断原因 / 已完成步骤）

## 已知技术债务
- 描述（影响：低/中/高，提出时间：YYYY-MM-DD）
```

## 状态更新触发条件
- 新功能任务完成
- 仲裁裁定生效
- 规范 MD 版本升级
- 任务中断（C 型仲裁）

## 上下文恢复协议

遇到「继续/上次/恢复」时，按序执行：
```
1. 读取 .github/memory/agent-system-state.md
2. 读取 .github/memory/decisions.md（最近 3 条）
3. 如有未决裁定 → 读取 arbitration-log.md 对应条目
4. 向用户确认恢复的上下文，再继续
```
```

---

#### 文件 7：`.github/agents/enhance.md`

```markdown
<!-- version: 1.0.0 | last-updated: {{TODAY}} | project: {{PROJECT_NAME}} -->

# 提升角色 — {{PROJECT_NAME}}

## 任务后反思

每次任务完成时执行：
```
1. 是否完全遵循了规范？
   遵循了：[列举]
   偏离了：[说明原因，是否需要仲裁]
2. 是否有更优方案被忽略？
3. 是否产生了新的规则需求？
4. 本次任务是否暴露了文档空白？
```

## 版本管理

所有 agents/*.md 版本头格式：
`<!-- version: X.Y.Z | last-updated: YYYY-MM-DD | updated-by: [role] -->`

| 变更类型 | 递增 |
|---------|------|
| 措辞修正 | Z+1 |
| 新增章节 | Y+1，Z=0 |
| 整体重写 | X+1，Y=Z=0 |

## 常见错误模式

| 编号 | 错误 | 正确做法 |
|------|------|---------|
| ERR-001 | {{STATE_LAYER}} 直接调用网络层 | 通过 {{BUSINESS_LAYER}} |
| ERR-002 | {{UI_LAYER}} 内联样式/颜色 | 使用统一样式系统 |
| ERR-003 | 使用禁止的依赖 | 使用替代方案（见 agents/dev.md） |
| ERR-004 | 生成 TODO 代替实现 | 功能必须完整 |

## 规范改进待办

```
#### [ENH-YYYY-序号] 改进标题
- 来源：仲裁B型 / 任务反思 / 人工反馈
- 描述：[需要修改的内容]
- 目标文件：agents/[哪个MD]
- 优先级：高 / 中 / 低
- 状态：待处理
```
```

---

### Step 3：初始化记忆文件

同时生成以下文件：

**`.github/memory/arbitration-log.md`：**
```markdown
<!-- version: 1.0.0 | last-updated: {{TODAY}} -->
# 仲裁裁定历史 — {{PROJECT_NAME}}
> 追加只写，禁止删除历史条目。

## 裁定记录
> 暂无裁定记录。
```

**`.github/memory/decisions.md`：**
```markdown
<!-- version: 1.0.0 | last-updated: {{TODAY}} -->
# 重要架构决策 — {{PROJECT_NAME}}
> 追加只写，禁止删除历史条目。

## 决策记录
> 暂无决策记录。
```

---

### Step 4：输出摘要

```
## 生成摘要

### 项目信息来源
| 字段 | 来源 | 值 |
|------|------|-----|
| 项目名称 | [已填/推断/默认] | xxx |
| 框架 | [已填/推断/默认] | xxx |
| 架构分层 | [已填/推断/默认] | xxx |
| 数据流链路 | [已填/推断/默认] | xxx |
| 命名规范 | [已填/推断/默认] | xxx |
...

### 生成文件
| 文件 | 行数 | 状态 |
|------|------|------|
| .github/agents/product.md | XX | ✅ |
| .github/agents/dev.md | XX | ✅ |
| .github/agents/test.md | XX | ✅ |
| .github/agents/verify.md | XX | ✅ |
| .github/agents/arbitrate.md | XX | ✅ |
| .github/agents/memory.md | XX | ✅ |
| .github/agents/enhance.md | XX | ✅ |
| .github/memory/arbitration-log.md | XX | ✅ |
| .github/memory/decisions.md | XX | ✅ |

### 第七章回填
已将推断结果写入本文件第七章，请确认以下字段是否准确：
- [列出所有「推断」和「默认」来源的字段，供人工核对]
```
