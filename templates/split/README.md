# 分离式版模板

把"协议"、"角色模板"、"生成器"拆成 3 个文件，各司其职。

## 文件结构

| 文件 | 角色 |
| --- | --- |
| [copilot-instructions.template.md](copilot-instructions.template.md) | **协议层**：路由表、价值观、记忆位置、第七章项目信息 |
| [agent-role-templates.md](agent-role-templates.md) | **角色模板集**：7 个角色 MD 的结构定义（带占位符） |
| [agent-generator-prompt.md](agent-generator-prompt.md) | **生成器 Prompt**：把上面两份喂给 Agent 即可输出最终 `.github/agents/*.md` |

## 用法

```
1. 拷贝 copilot-instructions.template.md → 你的项目 .github/copilot-instructions.md
2. 填写第七章项目信息
3. 把 .github/copilot-instructions.md + agent-role-templates.md + agent-generator-prompt.md
   一起交给 Agent
4. Agent 输出 7 个角色 MD
```

## 优点

- 单一职责：协议、模板、生成逻辑各自独立维护
- 团队可以只 review 协议层，不看模板细节
- 自定义角色模板时只改 `agent-role-templates.md`

## 缺点

- 上手成本略高，需要理解三个文件的协作关系
