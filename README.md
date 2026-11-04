# liziyan

做 **MCP / Agent Skill** 的开发者 · Shanghai · 使用 GitHub 自 2019 年

## 项目

### 🧾 [mcd-allergy-safe-advisor](https://github.com/ziyanTOP/mcd-allergy-safe-advisor)

**麦麦安心点单官** —— 基于麦当劳官方 MCP 的过敏原与忌口安全点单顾问

> 让「我有花生过敏，还能吃麦当劳吗」这个问题，在 30 秒内得到可执行答案。

- 实时拉取门店在售菜单（实测 135 个餐品，含套餐引用的隐藏子项），做**三层过敏原风险分级**：
  安心可选 / 去除配料后可选 / 需到店确认 / 不建议选择
- 输出三件套：**能不能吃 · 怎么改**（例如「不加辣椒黄油风味酱」）**· 到店怎么说**
- 麦当劳 MCP 开发者大赛参赛项目（官方登记 Issue 已通过校验）
- 46 项单元测试，测试夹具直接采用 `mcp.mcd.cn` 的**真实响应原文**

`Python` · `MCP` · `Agent Skill`

### 📺 [bilibili-mcp](https://github.com/ziyanTOP/bilibili-mcp)

**B 站 MCP Server** —— 搜索 / 评论 / 字幕 / 弹幕 / 回复（fork 上游并贡献改动）

`Python` · `MCP`

### 🔐 [skills-secure-code-game](https://github.com/ziyanTOP/skills-secure-code-game)

Agent Skill 安全编码挑战的复刻实现

`Python` · `Agent Skill`

## MCP 实战笔记

做麦当劳那个项目时挖出来的三条结论——**全部来自实测响应，不是抄文档**：

1. **`query-meals` 不返回餐品名。**
   它只给分类和餐品编码，价格还分散在另一张映射表里；餐品名、套餐组成与可去除配料
   全在 `query-meal-detail`。所以真实链路必须是两跳调用。

2. **`modification` 里的配料分三档，语义完全不同。**
   `minQuantity ≥ 1` 是必选配料（不可去除）；`minQuantity == 0` 表示可以要求「不加」。
   把这两者混为一谈，误判率会直接翻倍——比如「奶油球」默认加了但能去掉，
   不该和「辣椒黄油风味酱」判成同一档。

3. **套餐子项会引用没有单列的餐品。**
   它们只在 `rounds[].choices[].code` 里留了个编码。顺着 code 再拉一轮 detail，
   菜单覆盖能从 68 个餐品变成 107 个，套餐也才能真正继承子餐品的过敏原。

完整实现、夹具与踩坑记录见
[mcd-allergy-safe-advisor](https://github.com/ziyanTOP/mcd-allergy-safe-advisor)。
