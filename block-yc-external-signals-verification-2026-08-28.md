# Block / YC 三组外部信号核验

> 核验日期：2026-08-28  
> 口径：优先 Block、Y Combinator、相关公司的官方页面。重点区分“官方原词”“YC 投资组合公司”“第三方二次包装”和“未找到证据”。

## 结论总表

| 截图归因 | 核验结论 | 最准确的说法 |
|---|---|---|
| Block：Company World Model；DRI；Player-Coach | **有直接官方来源，但成熟度容易被夸大** | `company world model` 是 Block 官方公开的组织设计概念，不是已证实的独立软件产品；DRI 和 player-coach 是其目标组织中的官方三类角色之二，但 Block 明说转型仍处于早期。 |
| YC：Within / Company Brain；工作、决策、审批 | **产品与描述属实，但不是 YC 自己的方法论** | Within 是 YC S18 投资组合公司（原 Klarity）；Company Brain 是 Within 的官方产品定位，确实映射工作、决策、审批、控制与交接。 |
| YC：Agentic Fabric；身份、权限、审批、审计 | **核心描述几乎逐字属实，但公司名拼写应为 Agentic Fabriq** | Agentic Fabriq 是 YC W26 投资组合公司；其官方定位就是 agent 的 identity、scoped permissions、approval flows、audit logs。不是 YC 自己的产品或治理框架。 |

## 1. Block：Company World Model / DRI / Player-Coach

### 官方原词

Block 官方文章 [From Hierarchy to Intelligence](https://block.xyz/inside/from-hierarchy-to-intelligence) 由 Jack Dorsey 与 Roelof Botha 联名发表，页面元数据日期为 2026-03-31。文章明确使用：

- `company world model`：持续理解公司自身运营、绩效、优先级、正在构建什么以及哪里受阻的模型。
- `Directly Responsible Individuals (DRI)`：对跨团队问题、机会或客户结果负责，并可调动多个团队的资源。
- `Player-coaches`：继续写代码、建模型或做设计，同时负责培养周围的人。

文章提出未来组织“收敛为三种角色”：IC、DRI、player-coach；其中 world model 承担 alignment，DRI 承担 strategy/priority，player-coach 承担 craft/people。

### 证据边界

- **可确认**：这些不是第三方杜撰，而是 Block 官方公开的组织方向。
- **不可确认**：没有找到 `Company World Model` 作为独立产品的发布页、SKU、技术文档、仓库或版本。原文也一直把它作为概念性普通短语使用，而不是大写的正式产品名。
- **成熟度限定**：原文明确说 Block 仍处于这场转型的早期，一些部分可能先失败再奏效。因此应称为“官方宣布并正在推进的目标组织设计”，不应称为“已全面落地、验证成熟的 Block operating system”。

补充官方工程信号：[Protecting Our Systems with Intelligence](https://engineering.block.xyz/blog/protecting-our-systems-with-intelligence) 介绍 Block 用 Builderbot、AGENTS.md、Agent Skills、统一 review CLI 和全局/局部 checks，让 agent 依据组织的 global world model 审查代码。这证明 world-model 语言已进入具体工程实践，但仍不等于存在一个名为 Company World Model 的单独产品。

### 对截图归因的判定

- “Block 提出 company world model”：**准确**。
- “Company World Model 是 Block 已发布的产品/成熟方法论”：**证据不足，属于二次包装**。
- “DRI、Player-Coach 是 Block 官方组织原则”：**基本准确**；更严谨地说，是其公开的目标三角色组织设计，且尚在早期转型。

## 2. YC / Within：Company Brain

### 官方原词

[YC 官方公司页](https://www.ycombinator.com/companies/within) 显示：

- 公司名：Within，原名 Klarity。
- YC 批次：Summer 2018，当前状态 Active。
- 官方定位：为人和 agent 协同工作的 `Company Brain`。
- YC 页面对其结构层的描述包括：工作如何完成、决策如何产生、审批如何流转、控制如何执行，以及工作何时真正发生。

[Within 官方 Company Brain 页面](https://www.within.ai/platform/company-brain) 将其定义为一个由 people、processes、systems 组成的 context graph；其 FAQ 进一步说明它记录实际 workflow、决策、审批、交接、专业知识与耗时，并随工作持续更新。

### 证据边界

- “核心是工作、决策、审批”：**有直接证据，但概括不完整**。官方还强调人员、流程、系统、控制、交接、判断与自动化 ROI。
- “YC 有一个叫 Within 的投资组合公司，其产品叫 Company Brain”：**准确**。
- “Within / Company Brain 是 YC 自己的内部方法、产品或官方组织操作系统”：**未找到证据**。YC 官方页面是在介绍被投公司，不代表 YC 自己采用了该产品或把它作为 YC 方法论。

### 对截图归因的判定

如果截图标题把它简写为“YC — Within / Company Brain”，这只能理解为“YC-backed company”。若读者会理解成“YC 自己发明或运行的 Company Brain”，则属于**归因过度**。

## 3. YC / Agentic Fabriq：身份、权限、审批、审计

### 官方原词

[YC 官方公司页](https://www.ycombinator.com/companies/agentic-fabriq) 显示：

- 正式公司名：`Agentic Fabriq`（结尾是 **q**）。
- YC 批次：Winter 2026，当前状态 Active。
- 官方定位：AI agents 的 control plane。
- YC 页面直接列出四个核心能力：agent identity、scoped permissions、approval flows、audit logs。
- 其 launch post 还将产品描述为 agent 的 identity、governance、visibility layer，负责身份传播、授权、least-privilege access 和集中审计。

[Agentic Fabriq 官方产品页](https://www.agenticfabriq.com/) 进一步写明：agent 的调用经过统一控制层；每个请求依据 agent、代表的 user 与目标 resource 进行 allow/block 判断，并记录完整 audit trail。

### 证据边界

- “核心是身份、权限、审批、审计”：**准确，几乎是 YC 官方公司页的逐项概括**。
- “名字叫 Agentic Fabric”：**不够准确**。当前官方公司/产品品牌是 `Agentic Fabriq`；宣传材料可能把底层能力泛称 fabric，但引用时应保留 q，避免和 SailPoint 的 `Agentic Fabric` 产品混淆。
- “这是 YC 自己的产品、标准或 agent 治理方法论”：**未找到证据**。它是 YC 投资组合公司提供的商业产品。

### 对截图归因的判定

如果截图写“YC — Agentic Fabric”，建议改成：

> **YC-backed Agentic Fabriq：以 agent 身份、细粒度权限、审批流和审计日志为核心的治理控制层。**

## 4. 最终归因修正

最稳妥的三行表述是：

1. **Block 官方组织实验**：以 `company world model` 替代部分层级信息路由，目标角色包括 IC、DRI、player-coach；仍处早期转型，尚不能称为成熟产品或定型方法论。
2. **YC 投资组合公司 Within**：其 `Company Brain` 用 context graph 映射真实工作、决策、审批、控制和交接，为 agent 提供组织上下文；不是 YC 自有方法论。
3. **YC 投资组合公司 Agentic Fabriq**：为 agent 提供身份、细粒度权限、审批与审计的控制层；品牌拼写是 Fabriq，不是 YC 自有 `Agentic Fabric` 框架。

## 未找到的证据

- Block 将 `Company World Model` 作为独立商业产品正式发布的证据。
- Block 已在全公司完成 DRI / player-coach 转型并有量化成效的独立官方材料。
- YC 自己部署 Within，或把 Company Brain 作为 YC 内部统一方法论的证据。
- YC 自己拥有或运营名为 Agentic Fabric 的产品/标准的证据。
- 三组概念属于一套共同来源、相互协调的企业 AI/FDE 方法论的证据。
