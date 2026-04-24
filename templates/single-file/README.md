# 单文件版模板

一个 `copilot-instructions.template.md` 同时承载：

- **协议层**：价值观 / 权威排序 / 角色路由表 / 全局禁止 / 仲裁协议 / 记忆位置
- **第七章项目信息**：填写区（也可留空让 Agent 自动推断）
- **附录生成器**：Agent 读到「执行生成器」时，自动产出 `.github/agents/*.md`

## 用法

1. 把 [copilot-instructions.template.md](copilot-instructions.template.md) 拷贝到你的项目 `.github/copilot-instructions.md`
2. （可选）填写第七章项目信息；不填也行
3. 对 Copilot Chat 说："执行生成器"或"生成 agents 目录"
4. Agent 输出 7 个角色 MD + 2 个记忆 MD

## 优点

- 单文件，便于在仓库间复制粘贴
- 所有契约和生成规则集中可见

## 缺点

- 文件偏长（约 600+ 行），编辑成本略高
- 多人协作时容易冲突
