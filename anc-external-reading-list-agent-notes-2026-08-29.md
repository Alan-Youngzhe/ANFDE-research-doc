# ANC / Enterprise Agent Harness 外部必读清单

> 整理日期：2026-08-29  
> 资料标准：只列原作者文章、公司官网、官方 GitHub、官方招聘页和官方研究报告；不列媒体转述。  
> 目的：先建立你自己的 ANC 心智模型，再判断 Lawted 的方案、QM、Block、OpenAI、Anthropic、SAP 和咨询公司的方法究竟在哪些地方相同、哪些地方不同。

## 明天怎么读

不要按公司一个个纵向读。建议按下面这条链路：

```text
Lawted 的组织主张
→ Context 如何沉淀
→ 企业 Harness 如何实现
→ 权限、审批、审计如何补齐
→ 为什么要重构组织
→ FDE 怎样把系统部署进企业
→ 大型咨询公司如何把改革规模化
```

### 90 分钟核心路径

按顺序读下面 8 项，先不要打开 P1/P2：

1. HA7CH《0 中层公司》：12 分钟
2. HA7CH《The Enterprise AlphaGo Moment》：12 分钟
3. Karpathy《LLM Wiki》：12 分钟
4. YC Software `qm` README：15 分钟
5. Block《From Hierarchy to Intelligence》：12 分钟
6. SAP × Anthropic：8 分钟
7. OpenAI Deployment Company：8 分钟
8. Anthropic FDE 官方职位页：6 分钟

余下时间用 5 分钟写四个答案：

- ANC 的树干究竟负责什么？
- 哪些东西仍然应该留在 ERP、CRM、财务系统里？
- 什么行为才算“使用即搭建”？
- 活水实业的第一个真实闭环应该是什么？

如果只有 30 分钟，读第 1、3、4、6 项。

---

## P0：明天必须读

### 1. HA7CH：0 中层公司——AI Native Company 的下一种组织形态

- **链接**：[中文原文](https://www.ha7ch.com/writing/zero-middle-management/zh)
- **优先级 / 时间**：P0 / 12 分钟
- **为什么读**：这是目前最接近 ANC 组织定义的一手文章。它没有把 ANC 定义成一个 Agent，而是把传统中层的信息路由职能拆给 Company Context、Skills、Agents、Evals、权限系统与动态 DRI。
- **重点看**：
  - “组织架构本来就是一种信息技术”；
  - “中层不会被一个 Agent 替代，而是被一套系统拆解”；
  - Owner、DRI、Builder、player-coach 四类角色；
  - 48 小时、48 天、48 周分别改变什么。
- **读完应能回答**：为什么 ANC 不是 Agent SaaS，以及“树干”为什么同时是技术问题和组织问题。

### 2. HA7CH：The Enterprise AlphaGo Moment

- **链接**：[官方原文](https://www.ha7ch.com/writing/enterprise-alphago-moment)
- **优先级 / 时间**：P0 / 12 分钟
- **为什么读**：这是 Lawted 48 理论最清楚的交付说明，也给出了何时不应先上 RAG：先吸收 Context，再让 Context 决定架构。
- **重点看**：
  - 48 小时不是交付承诺，而是找第一份可验证的价值证据；
  - 48 天如何沉淀员工的接受、修改、拒绝和异常；
  - 48 周如何重划人和 Agent 的责任、权限与风险；
  - RAG 的启用条件；IM 为什么只是 Gateway。
- **读完应能回答**：ANC 如何从一个真实业务点长出来，而不是先建一套大平台。

### 3. Andrej Karpathy：LLM Wiki

- **链接**：[原始 Gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- **优先级 / 时间**：P0 / 12 分钟
- **为什么读**：这是 ANC 的 Company Context / Organizational Memory 层最直接的公开技术原型。它解释了为什么不是每次查询时临时 RAG，而是把知识持续编译成可维护的 Markdown Wiki。
- **重点看**：
  - `The core idea`：RAG 与 persistent, compounding artifact 的区别；
  - `Architecture`：Raw sources / Wiki / Schema 三层；
  - `Operations`：Ingest / Query / Lint；
  - `Indexing and logging`：`index.md`、`log.md`；
  - 何时才加 BM25/vector search。
- **读完应能回答**：LLM Wiki 解决什么、不解决什么；为什么它不能代替 ERP 的交易事实和企业权限系统。

### 4. YC Software：QM — Multiplayer agent harness for work

- **链接**：[官方 GitHub README](https://github.com/yc-software/qm)
- **优先级 / 时间**：P0 / 15 分钟
- **为什么读**：这是与直播描述最接近的公开“多人企业 Agent Harness”实现。它不是一个 Company Brain 数据库，而是给每个人和每个协作空间独立的 memory、files、permissions、keychain、sandbox、skills 与后台任务。
- **重点看**：
  - `What is QM?` 与 personal/shared scopes；
  - shared skills 如何授权、分享和晋升到全组织；
  - `Architecture` 中 core、Postgres、per-scope sandbox 的边界；
  - company-specific 内容为什么放在很薄的 deployment layer；
  - `Security and secrets` 的三种 posture。
- **读完应能回答**：如果让 ANC 真的多人使用，身份、Scope、Skill、沙箱和持久化状态怎么落地。

### 5. Block：From Hierarchy to Intelligence

- **链接**：[Block 官方文章](https://block.xyz/inside/from-hierarchy-to-intelligence)
- **优先级 / 时间**：P0 / 12 分钟
- **为什么读**：这是“Company World Model + DRI + player-coach”的原始来源。Block 的目标不是给每个人加 Copilot，而是让系统承接原本由组织层级完成的信息协调。
- **重点看**：
  - 公司为什么需要自己的 world model；
  - 远程工作产生的 decisions、discussions、plans、problems、progress 如何成为原料；
  - capabilities / world model / intelligence layer / interfaces 四层；
  - IC、DRI、player-coach 三种角色。
- **读完应能回答**：ANC 的 Company Context 为什么必须服务行动和资源协调，而不只是“能问答的知识库”。

### 6. SAP × Anthropic：SAP and Anthropic Plan to Bring Claude to SAP Business AI Platform

- **链接**：[SAP 官方文章](https://news.sap.com/2026/05/sap-anthropic-to-bring-claude-sap-business-ai-platform/)
- **优先级 / 时间**：P0 / 8 分钟
- **为什么读**：这是 ANC 的重要对照组。SAP/Anthropic 的路线不是把企业推倒重建，而是让 Claude 在企业已投资的系统、数据、流程、控制和治理中推理与行动。
- **重点看**：
  - 开头“Enterprises don't need to be rebuilt around AI”；
  - SAP 提供 process/data/governance，Claude 提供 reasoning/action；
  - S/4HANA、SuccessFactors、Ariba 以及 MCP 的连接方式；
  - 调整订单、触发审批等动作为什么沿用已有控制。
- **读完应能回答**：如果企业现有 ERP 已经很强，ANC 应该覆盖它、替换它，还是成为其上的轻量 Harness。

### 7. OpenAI：OpenAI launches the OpenAI Deployment Company

- **链接**：[OpenAI 官方公告](https://openai.com/index/openai-launches-the-deployment-company/)
- **优先级 / 时间**：P0 / 8 分钟
- **为什么读**：这是目前最完整的一手 FDE engagement 描述：focused diagnostic → 少数 priority workflows → 嵌入客户组织 → 连接 data/tools/controls/processes → build/test/deploy → adoption 与 durable system。
- **重点看**：`Why deployment matters`、`Building for where frontier AI is headed`，以及典型 engagement 的描述。
- **读完应能回答**：FDE 的交付单位为什么是可测量的生产工作流，而不是功能清单或一个 demo。

### 8. Anthropic：Forward Deployed Engineer

- **链接**：[Anthropic 官方招聘页](https://job-boards.greenhouse.io/anthropic/jobs/5302966008)
- **优先级 / 时间**：P0 / 6 分钟
- **为什么读**：职位页比品牌文章更具体：FDE 在客户系统里交付 production applications、MCP servers、sub-agents、agent skills 和 eval frameworks，并把重复模式编码回产品与工程。
- **重点看**：`About the role`、`Responsibilities`、`You May Be a Good Fit If`。
- **读完应能回答**：Anthropic 怎样把现场叶子沉淀成 Skill、部署模式和产品反馈。
- **用词校正**：当前一手材料使用 **Forward Deployed Engineer / Applied AI**，没有找到 Anthropic 正式把这一岗位称为 FSE 的证据。

---

## P1：理解技术架构和安全边界

### 9. QM Security Policy

- **链接**：[官方 SECURITY.md](https://github.com/yc-software/qm/blob/main/SECURITY.md)
- **优先级 / 时间**：P1 / 10 分钟
- **为什么读**：README 告诉你架构能做什么，SECURITY.md 告诉你它不能保证什么。企业 Harness 最大的风险不是模型答错一句，而是跨 Scope 泄露、错误身份、过度授权和不可追责的副作用。
- **重点看**：scope、protected assets、operator/admin trust、published app boundary、known limitations。

### 10. Anthropic：Building Effective AI Agents

- **链接**：[Anthropic 工程文章](https://www.anthropic.com/engineering/building-effective-agents)
- **优先级 / 时间**：P1 / 15 分钟
- **为什么读**：给出从 workflow 到 agent 的最基础技术判断，强调从最简单可行方案开始，工具设计通常比堆复杂 orchestration 更重要。
- **重点看**：workflow 与 agent 的区别；prompt chaining、routing、parallelization、orchestrator-workers、evaluator-optimizer；何时使用自治 Agent。

### 11. Anthropic：Demystifying evals for AI agents

- **链接**：[Anthropic 工程文章](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)
- **优先级 / 时间**：P1 / 15 分钟
- **为什么读**：ANC 不能只有 Context、Agent 和 Skill；必须把“什么叫做完、做对、没有越权”编码成 Evals。文章也明确区分 agent harness 与 evaluation harness。
- **重点看**：task/trial/grader/trace/outcome；代码、模型和人工 grader；20–50 个真实失败任务如何形成第一套 eval；生产监控与 eval 的关系。

### 12. YC Within：Company Brain for people and agents to work in harmony

- **链接**：[YC 官方公司页](https://www.ycombinator.com/companies/within)
- **优先级 / 时间**：P1 / 6 分钟
- **为什么读**：Within 把 Company Brain 定义为“工作怎样完成、决策怎样产生、审批怎样流动、控制怎样执行”的映射，而不只是把公司文档放进搜索框。
- **重点看**：Discover / Structure / Improve 三步；如何把实际工作重新分配给人和 Agent。

### 13. YC Agentic Fabriq：The control plane for AI agents

- **链接**：[YC 官方公司页](https://www.ycombinator.com/companies/agentic-fabriq)
- **优先级 / 时间**：P1 / 8 分钟
- **为什么读**：这是 ANC 权限层的最简洁外部参照：每个 Agent 有身份，Agent 与代表的用户形成权限对，执行时传播身份、兑换短期令牌、执行最小权限并产生审计日志。
- **重点看**：agent identity、user identity、token exchange、least privilege、per-user permission、revocation、audit trail。

### 14. Block：Protecting Our Systems with Intelligence

- **链接**：[Block Engineering 官方文章](https://engineering.block.xyz/blog/protecting-our-systems-with-intelligence)
- **优先级 / 时间**：P1 / 12 分钟
- **为什么读**：它把“world model”落实到工程：全局 Context、局部 `AGENTS.md`、按需加载的 checks、Skills Marketplace、持续演化的政策与人类最终批准。
- **重点看**：progressive disclosure；hyperlocal context + global world model；Skills Marketplace；protector 与 assistant 的区别；heartbeat 如何提出新检查。

### 15. SAP：Announcing New Joule Studio

- **链接**：[SAP 官方技术说明](https://news.sap.com/2026/05/new-joule-studio-enterprise-scale-agentic-development/)
- **优先级 / 时间**：P1 / 10 分钟
- **为什么读**：用于逐层压力测试轻量 ANC：runtime、sandbox、policy、guardrail、observability、lifecycle、long-term memory 和用户 Gateway 中哪些可以暂缓，哪些生产环境不能省。
- **重点看**：`Build and orchestrate complex AI scenarios`、`Deploy enterprise-ready agents securely`、`Bring agents into the flow of everyday work`。

### 16. AWS：LLM Wiki 企业实践

- **链接**：[AWS 中国官方博客](https://aws.amazon.com/cn/blogs/china/llm-wiki-enterprise-practice/)
- **优先级 / 时间**：P1 / 12 分钟
- **为什么读**：Karpathy 的原文是模式，不是生产规格。AWS 的实践暴露团队规模后才出现的问题：并行编译重复页、检索召回、人类纠正被覆盖、不同受众权限隔离，以及 Wiki-first / RAG-fallback。
- **重点看**：Ingest → Compile → Serve；并发写入；human correction；按受众分别编译；何时保留 RAG fallback。

---

## P1：理解 FDE 怎样把架构送进企业

### 17. OpenAI：Forward Deployed Engineer 职位说明

- **链接**：[OpenAI 官方职位页](https://openai.com/careers/forward-deployed-engineer-%28fde%29-sf-san-francisco/)
- **优先级 / 时间**：P1 / 6 分钟
- **为什么读**：给出了 FDE 的完整 owner 边界：discovery、technical scoping、system design、build、production rollout；成功由 production adoption、measurable workflow impact 和 eval-driven feedback 衡量。
- **重点看**：`About the role` 和职责列表。

### 18. OpenAI：Introducing OpenAI Frontier

- **链接**：[OpenAI 官方文章](https://openai.com/index/introducing-openai-frontier/)
- **优先级 / 时间**：P1 / 10 分钟
- **为什么读**：这是 OpenAI 与 ANC 最接近的公开平台对照：enterprise context、identity/permissions、runtime、observability，再加 FDE 驻场方法和向研究反馈的闭环。
- **重点看**：平台的 context / execution / governance 结构；`Combining technology with know-how`。

### 19. Anthropic：Technical Deployment Lead

- **链接**：[Anthropic 官方招聘页](https://job-boards.greenhouse.io/anthropic/jobs/5017903008)
- **优先级 / 时间**：P1 / 6 分钟
- **为什么读**：Anthropic 没让一个 FDE 同时做完商业、组织和工程。TDL 负责 SOW、discovery、MVP、stakeholder、ROI、security/legal/procurement/compliance；FDE 负责生产代码。这个分工对你复盘商务弱势尤其重要。
- **重点看**：success metrics、executive sponsor、ROI、部署阻力和跨部门协调职责。

### 20. Anthropic：Building a new enterprise AI services company

- **链接**：[Anthropic 官方文章](https://www.anthropic.com/news/enterprise-ai-services-company)
- **优先级 / 时间**：P1 / 6 分钟
- **为什么读**：这是比大型 SAP 项目更接近中型企业现场的方法：小团队坐在一线人员旁边，先找到时间和价值在哪里流失，再围绕现有 workflow 构建并长期支持。
- **重点看**：`Why we're building this`、`What the work will look like` 和医疗集团示例。

### 21. HA7CH：十三问 FDE

- **链接**：[官方访谈](https://www.ha7ch.com/writing/thirteen-questions-on-fde)
- **优先级 / 时间**：P1 / 12 分钟
- **为什么读**：用于理解 Lawted 对 FDE 身份、客户现场、问题判断和交付方式的原始表述，避免只根据二手归纳理解他。
- **重点看**：他为何从工程转向企业现场；怎样理解真实 workflow；FDE 与传统外包、咨询、SaaS 的区别。

### 22. HA7CH：中国四城 FDE 行业观察

- **链接**：[中文原文](https://www.ha7ch.com/writing/four-cities-fde-report/zh)
- **优先级 / 时间**：P1 / 15 分钟
- **为什么读**：包含真实项目失败和落地样本，也明确指出传统 RAG 的知识库存在准确度退化、异构知识整合和持续演化问题。
- **重点看**：深圳项目样本；知识库 + data skills 的判断；获客、行业选择和项目定价的讨论。

---

## P2：理解咨询公司如何做企业级改革

这部分不是用来照搬“大咨询方案”，而是补齐 ANC 容易忽略的价值治理、Operating Model、组织责任和规模化采用。

### 23. Accenture：Reinvention in the age of generative AI

- **链接**：[Accenture 官方报告](https://www.accenture.com/us-en/insights/consulting/total-enterprise-reinvention)
- **优先级 / 时间**：P2 / 12 分钟
- **为什么读**：Accenture 不主张堆零散 use case，而是围绕端到端 business capability 同时改变 process、people、technology，并以安全 digital core 承接。
- **重点看**：`Lead with Value`、AI-enabled secure digital core、C-suite imperatives。

### 24. Accenture：Forward Deployed Engineer

- **链接**：[Accenture 官方职位页](https://www.accenture.com/us-en/careers/jobdetails?id=R00339500_en&title=Forward+Deployed+Engineer)
- **优先级 / 时间**：P2 / 6 分钟
- **为什么读**：看传统咨询公司怎样吸收 FDE：从模糊问题到 sprint、数天原型、数周生产，现场连接 legacy system、regulated data、identity 和 security，同时把失败模式带回更大实践。
- **重点看**：它为什么明确说这不是 support role，也不是 advisory role。

### 25. McKinsey：A generative AI reset — Rewiring to turn potential into value

- **链接**：[McKinsey / QuantumBlack 官方文章](https://www.mckinsey.com/capabilities/tech-and-ai/our-insights/a-generative-ai-reset-rewiring-to-turn-potential-into-value-in-2024)
- **优先级 / 时间**：P2 / 12 分钟
- **为什么读**：McKinsey 的核心不是选一个模型，而是选高价值业务域，重做 workflow、跨职能产品团队、数据架构、人才与 adoption。
- **重点看**：业务域选择和六项 Rewired capability；现场服务/调度案例。

### 26. McKinsey：The state of AI — How organizations are rewiring to capture value

- **链接**：[McKinsey / QuantumBlack 官方调查](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai-how-organizations-are-rewiring-to-capture-value)
- **优先级 / 时间**：P2 / 12 分钟
- **为什么读**：用调查数据检查我们的观点：workflow redesign 与 EBIT impact 的关系，以及组织怎样设置责任、治理、培训、反馈和 rollout。
- **重点看**：组织负责人；adoption/scaling practices；workflow redesign 与财务影响。

### 27. McKinsey：Seizing the agentic AI advantage

- **链接**：[McKinsey / QuantumBlack 官方文章](https://www.mckinsey.com/capabilities/quantumblack/our-insights/seizing-the-agentic-ai-advantage)
- **优先级 / 时间**：P2 / 12 分钟
- **为什么读**：用于压力测试“轻树干”：McKinsey 的 agentic mesh 强调共享 Context、Agent 协调、身份治理、observability 与生命周期，防止 agent sprawl 和 autonomy drift。
- **重点看**：seven capabilities of the agentic mesh；foundation model requirements；cross-functional transformation squads。

### 28. OpenAI：Introducing Frontier Alliances

- **链接**：[OpenAI 官方文章](https://openai.com/index/frontier-alliance-partners/)
- **优先级 / 时间**：P2 / 7 分钟
- **为什么读**：它非常清楚地划分责任：OpenAI/FDE 提供平台与现场部署；McKinsey/BCG 负责战略和 Operating Model；Accenture/Capgemini 负责集成、全球交付和 change management。
- **重点看**：不要把“一个 FDE”想象成同时完成战略咨询、系统集成、组织改革和生产工程的超级个人。

---

## 读完后的对照表

| 资料 | 它主要解释 ANC 的哪一部分 |
|---|---|
| HA7CH《0 中层公司》 | 组织结构与树干职责 |
| HA7CH《Enterprise AlphaGo Moment》 | 从现场验证到“使用即搭建”的交付路径 |
| Karpathy LLM Wiki | Context 的持续编译与组织记忆 |
| YC QM | 多人 Harness、Scope、Skill、Sandbox、持久状态 |
| Within | 工作、决策、审批、控制怎样进入 Company Brain |
| Agentic Fabriq | Agent 身份、权限、令牌与审计 |
| Block | Company World Model、DRI、player-coach |
| SAP × Anthropic | 现有 ERP/流程/治理作为可信运行基础 |
| OpenAI Frontier / DeployCo | 平台 + FDE + 生产采用 + 研究反馈 |
| Anthropic FDE / TDL | 工程交付与商业/组织交付的角色分工 |
| Accenture / McKinsey | 价值治理、Operating Model、变革与规模化 |

## 阅读时不要混淆的五组概念

1. **Company Context 不等于数据库。** 数据库是存储零件；Context 是经过来源、状态、责任和权限组织后，可用于决策和行动的企业状态。
2. **LLM Wiki 不等于完整 ANC。** 它解决组织记忆，不直接解决交易真相、实时状态、权限、审批和副作用控制。
3. **Context Graph 不等于 Palantir 式全局 ontology。** 可以从真实任务和业务对象增量长结构，不必先穷举全公司的对象和关系。
4. **Agent Harness 不等于一个 Agent。** Harness 是身份、Scope、Context 组装、工具、Skill、运行、审批、追踪和 Eval 的共同运行环境。
5. **FDE 不等于单兵版麦肯锡。** 生产工程、商业价值、组织授权、法务合规和变革管理通常由跨职能小队共同承担。

## 建议做的阅读笔记模板

每篇只写五行，避免重新抄文章：

```markdown
### 标题
- 它认为企业现在的根本问题是什么：
- 它提出的树干/基础设施是什么：
- 它认为哪些东西不应该重建：
- 它怎样把一次使用变成组织资产：
- 对活水实业最直接的启发或冲突：
```

