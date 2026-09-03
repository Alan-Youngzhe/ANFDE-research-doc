# ANC 全面介绍：从 AI 工具堆积到企业运行 Harness

日期：2026-08-29  
版本：v1.1  
定位：面向企业老板、业务负责人、FDE 与技术团队的共同说明书。

> 重要边界：本文中的 ANC 指 HA7CH / Lawted 语境下的 **AI Native Company**。它不是已经形成统一标准、已有唯一参考实现的行业术语。本文区分两层：一是 Lawted 公开分享中已经跑通的轻量形态；二是面向更大企业时可能需要补齐的治理架构。后者是本文的工程推演，不是对 HA7CH 私有源码的声明。

---

## 0. 一页结论

### ANC 是什么

ANC 不是一个更大的 AI SaaS，不是企业知识库，也不是把所有员工换成 Agent。

它首先是一套由 **共享 files/markdown、Agent workspace 和协作规则**组成的公司 Context。多个 AI Native Person（ANP）通过 Claude Code、Codex、Hermes 一类 code agent 搜索、读取和修改它，再把稳定方法蒸馏成 Skill。

在小团队里，Agent workspace 本身就是最小 Harness；随着组织扩大，才逐步补任务、权限、责任、审批、Eval 和审计：

```text
Data → Context → Harness → Gateway
         ↑           │
         └── Artifact / Eval / Feedback ──┘
```

- **Data** 保留原始事实和业务系统；
- **Context** 最小可以只是结构清楚、可直接读取的 files/markdown；
- **Harness** 最小可以直接复用 Claude Code、Codex、Hermes 等 Agent workspace，企业级能力按风险添加；
- **Gateway** 可以是本地终端，也可以是飞书、钉钉、Web、API；
- 每次真实执行都留下 Artifact，反馈再更新 Context、Skill 和 Eval。

### 它解决什么

1. 数据在系统里，但公司不知道“现在发生了什么、谁负责、下一步做什么”；
2. Agent、知识库和 SaaS 各自聪明，却共享不了 Context、权限和反馈；
3. 关键判断停留在老员工脑中、微信、会议和临时 Excel；
4. 任务依靠管理层层传递，信息不断失真；
5. AI 节省了时间，却没有重新分配为收入、风险下降或组织增长；
6. 每次项目重新调研、重新写 Prompt、重新搭工具，公司不会因使用而变聪明。

### 给企业和老板的价值

- 老板不等三层汇报，也能看到真实业务状态、证据、卡点和 DRI；
- 员工拿到完成任务所需的 Context、Skill 和权限，不靠找熟人搬信息；
- 成功方法和失败边界沉淀为组织资产，换人、换模型、换工具后仍能留下；
- AI 从“提高个人效率”升级为“提高整个组织的信息流速和行动质量”；
- 企业拥有自己的 Context、行动边界和学习闭环，而不是被某个 Agent SaaS 锁住。

---

## 1. ANC 要解决的根本问题

## 1.1 企业已经有数据，但没有可行动的 Context

多数企业并不真正缺数据。它们已经有 ERP、CRM、财务、审批、工单、IM、网盘和无数 Excel。

缺的是：

```text
这是哪个客户/订单/项目？
当前生效的是哪个版本？
现在处于什么状态？
这个判断来自哪份原件？
谁有权看到？
谁对下一步负责？
哪些动作 Agent 可以直接做？
哪些必须经过人？
上一次为什么做错？
```

数据库能保存字段，却不会自动形成这套业务含义、责任与行动逻辑。

## 1.2 企业在重复购买“叶子”

常见 AI 项目包括：

- OCR；
- 问答知识库；
- 合同助手；
- 报价 Agent；
- 客服机器人；
- 财务匹配；
- 数据分析助手；
- 自动报告；
- 独立部门工作流。

这些功能可以有 ROI，但如果每个工具分别维护用户、知识、权限、Prompt、状态和反馈，公司会得到第二轮系统孤岛。

模型和功能会越来越便宜。真正难复制的是：这家公司在长期工作中形成的 Context、判断、异常处理、责任关系和反馈。

## 1.3 组织架构本质上承担信息路由

传统中层管理的很多工作是：

```text
收集进度 → 压缩信息 → 上报 → 接收决定 → 分解任务 → 催办 → 检查
```

组织变大后，人类带宽不足，只能增加层级；层级越多，信息越慢、越失真。

ANC 并不是“用一个 AI 经理替代所有中层”，而是把中层工作拆成可运行组件：

| 传统职能 | ANC 承接方式 |
|---|---|
| 汇总状态 | Company Context + 持续更新的组织状态 |
| 传递方法 | Skills + 标准工作流 |
| 分配任务 | Agent orchestration + 动态 DRI |
| 检查质量 | Evals + monitoring + audit |
| 权限控制 | identity + scope + approval policy |
| 人才成长和冲突 | 人类 player-coach |

该组织观点可对照 [HA7CH《0 中层公司》](https://www.ha7ch.com/writing/zero-middle-management/zh) 与 [Block《From Hierarchy to Intelligence》](https://block.xyz/inside/from-hierarchy-to-intelligence)。

## 1.4 AI 提效没有自动变成企业价值

员工把两小时报告压缩到十分钟，不代表公司获得了增长。企业还必须决定：

- 节省的时间被重新投入哪里；
- 哪些岗位应承担更完整的结果；
- 哪些审批和协调环节可以消失；
- 哪些风险必须保留人工责任；
- 怎样用经营指标验证改变。

ANC 的目标不是 Token 使用量，而是业务工作如何重新组织。

---

## 2. 树干、树枝与叶子

### 树干

所有业务能力共享的组织运行层：

1. Company Context；
2. identity、scope、权限、审批与审计；
3. Task、Decision、动态 DRI；
4. Agent 调 Skill、工具和人的运行时；
5. Artifact、Trace、Eval 和反馈闭环；
6. 企业自己的规则、经验和能力晋升机制。

### 树枝

围绕一个端到端业务域形成的共享能力，例如：

- Contract-to-Cash；
- 询盘到报价；
- 订单到履约；
- 招聘到入职；
- 工程项目到付款；
- 客诉到整改。

### 叶子

具体可替换工具和功能，例如 OCR、合同生成、路线优化、催收函、报表、某个 Agent 或 SaaS。

树干轻，意味着它不重新实现所有叶子；树干重要，意味着所有叶子必须共享 Context、权限、证据、任务和反馈。

---

## 3. ANC 的十个核心观点

## 3.1 不是数据库，而是企业 AI 运行方式

数据库、对象存储、图数据库都可能参与实现，但它们只是零件。

如果系统只有统一数据库和看板，它只能回答“有什么”；ANC 还必须回答：

```text
接下来谁做什么？
凭什么做？
可以自动做还是要审批？
做完留下什么？
失败怎样改变下一次？
```

## 3.2 使用即搭建

ANC 不是顾问一次性访谈完公司，再录入所有知识。它通过真实工作逐步搭建：

```text
工作产生 Artifact
→ Artifact 更新 Context
→ 经过验证的知识进入 Wiki/Decision
→ 稳定方法封装成 Skill
→ Skill 分发给人和 Agent
→ 新工作产生更好的 Artifact
```

一次人工修改首先解决当前任务；同类修改多次出现并通过 Eval 后，才升级为组织规则。

## 3.3 以真实业务对象和结果组织，而不是以软件模块组织

供应链围绕“一张订单、一件货、一个客户承诺”；园区运营可以围绕“一份租赁履约关系”；制造可以围绕“一件产品从询盘到客诉的生命周期”。

系统模块是实现边界，客户结果才是经营边界。

## 3.4 Context 不等于知识库

Context 至少包含：

- 事实；
- 当前状态；
- 版本；
- 来源；
- 责任；
- 权限；
- 任务；
- 历史决定；
- 相关异常；
- 可调用能力。

知识库通常只覆盖其中“稳定知识和文档解释”的一部分。

## 3.5 人和 Agent 可以互相调用

Agent 能调用 Skill，也能调用人。当任务需要谈判、现场判断、伦理责任、复杂关系或高风险决定时，人是运行时的一等能力节点。

调用人时，应提供目标、Context、证据、已完成步骤、候选方案、需要回答的问题和期限，而不是只发一句“请处理”。

## 3.6 权力绑定结果，而不只绑定职位

动态 DRI 表达的是：某个人在某段时间内，对一个具体结果负责，并临时获得所需 Context、工具和权限；任务结束后，授权结束或转移。

部门可以继续存在，但责任不能只写“归财务部处理”。

## 3.7 Eval 是企业标准的可执行表达

没有 Eval，Agent 只能展示能力，不能承担责任。

Eval 应同时检查：

- 最终业务状态；
- 完整执行轨迹；
- 原始证据；
- 权限是否正确；
- 正常、边界和异常案例；
- 人工接受、修改、拒绝的原因。

可参考 [Anthropic《Demystifying evals for AI agents》](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)。

## 3.8 拒绝 RAG-first，不等于禁止检索

推荐顺序：

1. 业务 ID、路径、时间、负责人等确定性定位；
2. Context Graph、索引、frontmatter、全文/BM25；
3. 读取少量 Wiki 页面和精确原始材料；
4. 超大语料、跨项目相似搜索、单对象持续溢出 Context 时，再使用 RAG fallback。

稳定知识应被编译为可维护的组织资产，而不是每次提问重新发现。可参考 [Karpathy《LLM Wiki》](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)及 [AWS 的企业实践](https://aws.amazon.com/cn/blogs/china/llm-wiki-enterprise-practice/)。

## 3.9 拒绝重型 ontology，也不要求先建设 Context Graph

| 重型 ontology | ANC 的轻量起点 |
|---|---|
| 先定义全企业对象、关系和动作 | 从真实工作产生的文件和目录开始 |
| 追求语义完整和长期统一 | 先支持事实、责任、权限和下一步行动 |
| 专家先建模，业务后使用 | 使用产生 Artifact，结构随工作生长 |
| 建模和映射成本高 | Markdown、frontmatter、链接、Git 历史即可起步 |

第一版不需要图数据库，也不必先抽象全公司的业务对象。文件目录、命名、frontmatter 和 Markdown 链接已经可以表达大量关系。只有当跨系统对象、实时状态和细粒度权限无法继续由文件可靠承载时，再增加 SQLite/PostgreSQL 或轻量 node/edge；Context Graph 是可能的演进形态，不是 ANC 的定义。

## 3.10 企业必须拥有树干，叶子可以采购

模型、OCR、SaaS、行业软件和外部 Agent 都可以更换。企业应长期拥有：

- 自己的 Context；
- Skill 和 Eval；
- 决策与证据；
- 权限与行动边界；
- 反馈历史；
- 业务对象和责任关系。

---

## 4. ANC：先看已经跑通的两种形态

Lawted 2026-08-10 分享给出了比“四层企业架构”更具体的实现线索。ANC 的最小技术内核不是新数据库，而是：

> 证据边界：本节目前依据用户提供的 8 月 10 日分享摘要整理，尚未逐字核对原始录音或全文；`anc-ha7ch`、`anc-climax`、`anc-sigma` 的内部实现细节不作超出摘要的推断。

```text
多个 ANP
→ 把会议、知识、语录和工作产物写进共享 files/markdown
→ Agent 在 workspace 中直接 search / read / grep / edit
→ 稳定 Context 蒸馏为 Wiki、Decision 和 Skill
→ Skill 再分发给全公司的 Agent
```

### Lite：Git 同步到每个人本地

类似 `anc-ha7ch`：

```text
GitHub repo
  ├── member A 本地 clone → Claude Code / Codex
  ├── member B 本地 clone → Claude Code / Codex
  └── member C 本地 clone → Claude Code / Codex
```

- files/markdown 通过 Git 同步；
- 每个人的 Agent 直接读取本地完整文件树；
- 去中心化、速度快、Agent search 能力完整；
- 当前卡点是目录级权限、clone 体积和速度、网络、包/Skill 分发、离职后的权限回收。

因此 Lite 是方向明确、工程治理仍在探索的形态，不能把这些问题写成已经解决。

### Hosted：中心机器托管 Context 和 Agent

类似 `anc-climax`、`anc-sigma`：

```text
飞书 / 钉钉 / Web
        ↓
中心机器（例如 Mac Mini）
  ├── company files/markdown
  ├── Claude Code / Agent runtime
  └── skills / logs / scheduled jobs
```

- 文件和 Agent runtime 放在中心机器；
- 员工通过飞书、钉钉等入口使用；
- 不要求每个人配置本地环境或 clone 全量仓库；
- 更容易统一升级、备份和控制入口；
- 代价是中心化、并发、身份映射、会话隔离和机器可靠性需要运营。

按这场分享，Hosted 已经跑通，Lite 仍在解决权限和分发问题。

## 4.1 为什么本地 Agent Search 是关键技术选择

本地 code agent 可以在一次任务里快速遍历几百个文件，使用 glob、grep、全文读取、目录关系和 Git diff。这里的优势不只是“检索准确”，而是 Agent 能自主决定下一步读什么，并保留完整文件上下文。

经过飞书、GitHub 或业务 API 后，通常会多出网络延迟、分页、切片、限流和字段裁剪。于是同一个 Agent 从“操作完整工作区”退化成“反复向远端请求若干片段”。这解释了 Lawted 为什么更偏向 files/markdown 和本地 workspace，而不是把 RAG/API 当作第一入口。

## 5. 面向更大企业的四层演进架构

下面的四层不是 ANC 起步时必须自研的平台，而是 Lite/Hosted 在人数、风险和业务系统增加后，逐步补齐的治理能力。

```text
┌──────────────────────────────────────────────────────────┐
│ Gateway：IM / Web / 工作台 / API / 文件 / Agent            │
└───────────────────────────┬──────────────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────┐
│ Identity / Scope / Policy / Approval                     │
│ 用户是谁、代表谁、当前任务、可读 Context、可执行动作          │
└───────────────────────────┬──────────────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────┐
│ Context                                                  │
│ Operational Context Graph：对象、关系、状态、责任、权限      │
│ Organizational Memory：Wiki、Decision、经验、失败边界       │
└───────────────────────────┬──────────────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────┐
│ Harness                                                  │
│ Agent loop / Work Graph / Task & DRI Router              │
│ Skills / Tools / Existing Systems / Humans               │
└───────────────────────────┬──────────────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────┐
│ Artifact / Trace / Eval / Feedback / Audit               │
└───────────────────────────┬──────────────────────────────┘
                            └──────回写 Context / Skill / Eval
```

## 5.1 Data：事实仍留在合适的系统

ERP、财务、CRM、MES、OA、工单和文件系统继续承担自己的事实责任。

ANC 不必复制全部数据，只需：

- 稳定 external ID；
- source URI；
- 必要状态快照；
- 文件 hash 和版本；
- 业务对象关联；
- 权限与时间范围。

原则：AI 结论必须能回到原始记录；LLM 生成结果不能反过来冒充交易事实。

## 5.2 Context：操作状态与组织记忆分开

### Operational Context

负责“现在”：

- 当前业务对象；
- 状态和版本；
- 责任人和任务；
- 相关原件；
- 谁能读写；
- 下一个允许动作。

### Organizational Memory

负责“公司学会了什么”：

- Wiki；
- 决策记录；
- 规则解释；
- 典型案例；
- 失败边界；
- Skill 使用说明；
- Eval 标准。

LLM Wiki 适合后者，不应作为实时库存、余额或交易状态的唯一真相。

## 5.3 Harness：从现成 Agent workspace 开始

在最小 ANC 中，Claude Code、Codex 或 Hermes 已经提供搜索、读取、编辑、工具调用和任务循环，不必重造 Agent runtime。企业规模扩大后，才按实际风险补：

1. identity 与 principal；
2. scope 和 permission resolver；
3. context assembler；
4. task/decision/DRI state；
5. agent loop；
6. Skill registry；
7. human routing；
8. approval gate；
9. trace、eval 和 audit；
10. reflection 与 Skill promotion。

[YC Software QM](https://github.com/yc-software/qm) 是目前较接近“多人企业 Agent Harness”的公开参照：个人/共享 scope、memory、files、permissions、keychain、sandbox、shared skills、审计和企业 deployment layer。

## 5.4 Gateway：进入真实工作的入口

Gateway 可以是：

- 钉钉/飞书/企业微信/Slack；
- 现有业务工作台；
- H5/Web 表单；
- API/Webhook；
- 文件上传；
- CLI；
- Agent 发起的后台任务。

IM 只解决入口，不等于 Company Brain。真正状态、权限、Skill 和反馈必须留在企业 Harness 中。

---

## 6. 三条核心运行闭环

## 6.1 Context 写入闭环

```text
捕获真实材料
→ 保存原始 Artifact
→ 判断身份、来源和 audience
→ 关联业务对象与 Context Graph
→ LLM 编译 Wiki/Decision/摘要
→ 人工处理冲突和敏感内容
→ 版本化提交并更新索引
```

## 6.2 任务执行闭环

```text
authenticate(principal)
→ resolve(task, scope, permission)
→ retrieve(context graph, wiki, raw evidence)
→ assemble minimal context
→ plan
→ route to Skill / Tool / Agent / Human
→ approve high-risk action
→ execute
→ record artifact and trace
→ eval final state
```

## 6.3 学习与能力晋升闭环

```text
任务结果
→ 接受 / 修改 / 拒绝 / 异常
→ 记录 before / after / reason / actor / evidence
→ 更新当前 Context
→ 同类模式多次出现
→ 生成候选 Skill + Eval
→ 业务 Owner 审批
→ 向指定 scope 发布
```

一次成功不能自动成为公司 SOP；一次错误也不能被系统静默吞掉。

---

## 7. 最小数据与 Artifact 模型

第一版不需要行业大 ontology，只需要一组通用元对象：

```text
Artifact
  id, kind, source_uri, hash, actor, audience, created_at

ContextNode
  id, type, external_ref, state_json, version, audience

ContextEdge
  from, relation, to, evidence_ref, valid_from, valid_to

Task
  id, objective, status, dri, due_at, required_scope

Decision
  id, question, decision, evidence, decider, effective_at

Skill
  id, version, input_schema, output_schema,
  required_permission, human_gate, evals, owner

Run
  id, task_id, principal, context_refs, actions,
  approvals, result, evidence, eval_result
```

每次关键工作至少留下：

```text
原始输入
Agent 方案
使用的 Context 和规则版本
人工修改
审批理由
执行动作
最终结果
Eval
```

---

## 8. 最小可部署技术方案

### 方案 A：Lite ANC

- 一个 Git 仓库保存允许共享的 Markdown、附件索引、Decision、Skill 和模板；
- 每个成员本地 clone，并使用 Claude Code、Codex 或 Hermes；
- Git commit/PR 负责版本、变更审查和回滚；
- `.gitignore`、加密文件或拆分仓库只能解决一部分权限问题；
- 适合信任度高、规模小、技术能力较强、资料敏感度可控的团队。

Lite 的首要工程问题不是模型，而是 repo 边界、密钥、成员权限、离职回收、Skill 包版本和同步冲突。

### 方案 B：Hosted ANC

- 一台受控机器或服务器保存 company files；
- 中心运行 Claude Code 或其他 agent runtime；
- 飞书/钉钉 Bot 把用户身份、问题和附件转给 runtime；
- 每次会话建立独立工作目录和可读 scope；
- 日志、备份、并发队列和密钥统一管理；
- 适合非技术员工较多、需要快速部署统一入口的组织。

Hosted 已经可以很轻，但不能把“所有员工共用一个高权限 Claude Code 进程”当成企业权限方案。涉及财务、人事、合同或客户机密时，仍要做身份、scope、沙箱和审计。

### 方案 C：规模化增强

只有当 files/markdown 无法承载实时交易状态、复杂授权或高并发时，再增加下面的组件。

### 存储

- 对象存储：原始文件、截图、附件和大 Artifact；
- PostgreSQL/SQLite：按需保存用户、scope、grant、task、run、audit，以及跨系统对象状态；
- Markdown + Git：Wiki、Decision、Skill 说明、Eval、规则与版本历史；
- 可选全文/BM25：Wiki 与 Artifact 索引；
- 向量检索：仅作为大规模语料的 fallback。

### 运行时

- Node.js/TypeScript 或 Python agent core；
- tool-calling 模型；
- MCP/HTTP/数据库只读连接器；
- 每个用户或 scope 的隔离沙箱；
- queue/scheduler 处理后台任务；
- policy engine 和人工审批；
- traces、metrics、audit log。

### Skill 规范

每个 Skill 必须定义：

```text
输入/输出 schema
所需 Context
所需权限
可产生的副作用
自动/人工审批边界
正常、边界、异常 Evals
失败和回滚方式
版本与 Owner
```

### 安全原则

1. 权限在模型调用和 Context 组装前执行；
2. Agent 不自行决定授权；
3. 高风险动作使用短期凭证、最小权限和人工批准；
4. 用户、Agent、Skill、审批人和副作用均可归因；
5. 外部文本和工具结果视为不可信输入；
6. 付款、签约、删除、公开发送等动作必须有确定性策略；
7. 审计只能帮助追责，不能替代事前控制。

可继续阅读 [QM Security Policy](https://github.com/yc-software/qm/blob/main/SECURITY.md) 和 [YC Agentic Fabriq](https://www.ycombinator.com/companies/agentic-fabriq)。

---

## 9. 如何落地：从一片叶子进入，但搭出树干

## 9.1 先选择一个“面”和一个“点”

- **面**：一条端到端业务结果，例如订单履约、合同回款、客户续约；
- **点**：四周内可以用真实材料完成并验收的狭窄链路。

点必须属于这个面，并留下未来可复用的业务对象、Context、Skill 和 Eval；否则只是孤立 Demo。

## 9.2 48 小时：建立第一份可信证据

目标不是交付整套系统，而是：

- 访谈多角色，画清输入、判断、例外、责任和输出；
- 选一个真实材料和可判对错的任务；
- 让结果回到原始证据；
- 记录成功、失败和人工修正；
- 决定是否值得进入真实使用。

参考 [HA7CH《The Enterprise AlphaGo Moment》](https://www.ha7ch.com/writing/enterprise-alphago-moment)。

## 9.3 48 天：在使用中蒸馏组织能力

- 接入真实员工和真实工作入口；
- 记录采用、修改、拒绝和异常；
- 建立权限与责任；
- 形成 Skill、Eval 和 Wiki；
- 让同一 Context 被多个角色或任务复用；
- 证明第二批工作比第一批更好。

## 9.4 48 周：重画人与 AI 的责任

只有经过长期验证，才讨论：

- 哪些任务 Agent 可以端到端承担；
- 哪些职位的信息搬运职能消失；
- 哪些结果改由动态 DRI 负责；
- 节省时间如何投入增长；
- 权限、风险和组织结构怎样变化。

ANC 不是先裁中层再搭系统，而是先建立组织智能，让部分信息路由工作自然失去必要性。

---

## 10. 四周最小试点模板

### 第 1 周：业务对象与证据

- 一名业务 Owner；
- 一个端到端业务面；
- 20–50 个真实案例；
- 一个核心业务对象；
- 原件、版本、主键和事实源；
- baseline 和停止条件。

### 第 2 周：第一个 Skill

- 输入、输出、异常和人工门；
- 业务证据链；
- 正常/边界/异常案例；
- 人工确认和失败显式化。

### 第 3 周：共享 Context

- 至少第二个角色或任务复用同一 Context；
- 加入 scope、权限、DRI 和任务状态；
- 证明不是另建一张孤立表。

### 第 4 周：反馈与 Go/No-Go

- 人工修改进入 Artifact；
- 建立 Skill v1 和 regression Eval；
- 比较时间、错误、采用和异常；
- 业务 Owner 决定继续、调整或停止。

最低 Go 条件：

1. 结果可追溯；
2. 真实用户愿意继续使用；
3. 同一 Context 被两个角色/任务复用；
4. 人工复核负担没有超过节省；
5. 越权和异常测试通过；
6. 业务结果可测量。

---

## 11. ANC 给企业带来的价值

## 11.1 从个人效率升级为组织效率

Copilot 让每个人更快；ANC 减少人与部门之间的信息等待、重复解释、重新录入和协调。

## 11.2 从数据资产升级为 Context 资产

企业不仅保留结果字段，还保留当时的原件、约束、方案、修改、审批理由和最终结果。

## 11.3 从关键人依赖升级为组织能力

老员工的经验不再只停留在个人脑中；经过验证后形成 Skill、Eval 和案例边界，新人和 Agent 都能调用。

## 11.4 从项目交付升级为能力复利

第一次解决问题可能仍然昂贵；后续同类工作应更快、更稳定、人工修改更少。公司使用得越多，组织能力越强。

## 11.5 从供应商锁定升级为企业所有权

外部模型、工具和 Agent 可以更换，而企业自己的 Context、Decision、Skill、Eval 和权限规则继续存在。

## 11.6 从管理黑箱升级为证据和责任

管理层能够看到：

- 当前业务状态；
- 为什么这样判断；
- 谁在负责；
- 哪个任务阻塞；
- 哪个动作等待批准；
- 最终结果是否达标。

---

## 12. ANC 给老板带来的价值

老板真正购买的不是 Agent 数量，而是经营控制力。

### 1. 更短的信息距离

老板能从结论下钻到真实业务对象、原始材料、判断过程和当前 DRI，而不只看到层层压缩后的汇报。

### 2. 更清楚的决策队列

系统把“必须由老板判断”的事项从日常噪音中分离出来，并携带证据、备选方案、成本和风险。

### 3. 更低的组织摩擦

员工不需要靠领导协调，才能获得另一个部门的数据、规则或资源；权限允许时，Harness 自动提供任务所需 Context。

### 4. 更可控的授权

老板不必在“全部自动化”和“全部人工”之间二选一。不同金额、风险和客户等级可配置不同的自动、确认和升级规则。

### 5. 把节省时间转成增量

老板可以重新安排 Agent 节省的工时：拓客、客户维护、产品改进、风险检查或新业务，而不是让效率收益消失在日常中。

### 6. 让公司每天比昨天更懂自己

真正的壁垒不是买到同一个模型，而是公司不断积累自己的异常、判断、客户信号、方法和结果。

### 不应向老板承诺的内容

- 48 小时重构整家公司；
- 一次接入全部系统；
- Agent 自动替代管理责任；
- 不需要人工确认；
- 所有数据进一个库就会自动产生智能；
- 上线后无需运营、Eval 和治理；
- 所有企业都值得建设 ANC。

---

## 13. 组织与角色

| 角色 | 核心责任 |
|---|---|
| Owner | 定义公司方向、重要结果、不可越过的边界和最终责任 |
| DRI | 在指定期限内对具体结果负责，获得临时资源与权限 |
| Builder | 直接创造结果，可以带领多个 Agent 完成更大任务 |
| Player-coach | 参与真实工作，同时负责专业标准、人才成长和复杂判断 |
| FDE | 进入现场，发现真实问题，建立首个可验证闭环并把经验沉淀进树干 |
| Platform/Security | 提供身份、运行时、权限、审计、部署和可靠性 |

FDE 不是一个人承担战略咨询、售前、生产工程、法务、安全、变革管理和客户成功。成熟交付通常是跨职能小队。

OpenAI、Anthropic、Accenture 和 McKinsey 的公开 FDE/转型角色对照见：[FDE 官方定义对比](./fde-definitions-accenture-mckinsey-openai-anthropic-2026-08-29.md)。

---

## 14. 交付物应该是什么

一次合格的 ANC 试点不以“上线一个 Agent”结束，应至少交付：

1. 业务面与真实工作流地图；
2. 核心业务对象和最小 Context Graph；
3. 事实源、主键、版本和 Artifact 规则；
4. 角色、scope、权限和审批矩阵；
5. 第一个可运行 Skill；
6. 人类异常队列和 DRI 机制；
7. 正常、边界、异常和越权 Eval；
8. Run trace、业务 baseline 和试点结果；
9. 反馈如何进入 Context/Skill/Eval 的机制；
10. 下一阶段 Go/No-Go 决策。

交付形态是：

```text
诊断
→ 树干最小运行时
→ 首个业务域/Skill
→ 真实使用和 Eval
→ 是否扩展的经营决定
```

---

## 15. 什么企业适合，什么企业暂时不适合

### 适合

- 已有数字化材料，但跨系统、跨部门割裂；
- 有明确业务 Owner 和一号位支持；
- 能开放真实材料与真实用户；
- 存在可量化的收入、成本、风险或响应问题；
- 有重复任务、人工修改和异常经验可以沉淀；
- 愿意先跑小闭环，而不是要求一开始全自动。

### 暂不适合

- 老板只想看 Demo，不愿开放真实流程；
- 没有事实源、负责人和验收标准；
- 问题频率极低，建设和维护成本高于收益；
- 企业连基本在线化和数据责任都没有，且不愿补齐；
- 只想通过 AI 裁员，没有新的经营目标；
- 要求 Agent 立即承担高风险动作但不接受审批和审计；
- 没有真实用户愿意连续使用。

会判断“不值得搭 ANC”也是 FDE 的能力。

---

## 16. 活水实业示例

活水已有金蝶、钉钉 OA、云物管、Excel、微信和合同材料；现有方案也设计了 CloudBase、钉钉 AI 表格、H5、云函数、WorkBuddy 和表格处理。

但这些零件要成为 ANC，必须围绕核心业务对象组织。当前建议：

```text
核心对象：TenancyCase（租赁履约单）
业务面：意向 → 签约 → 入驻 → 收费 → 催收/服务 → 退租
价值证明点：一个楼栋、一个月度批次的房租结算
首个 Skill：合同/金蝶/渠道规则匹配 + 异常人工确认
树干证明：同一 TenancyCase 被财务和招商/物业至少两个角色复用
```

详细映射、权限、Skill Registry、Evals 和四周试点见：[活水实业 ANC 应用蓝图](./huoshui-anc-application-blueprint-2026-08-29.md)。

---

## 17. 判断 ANC 是否真的成立的十个问题

1. 是否围绕真实业务对象，而不是部门功能？
2. 关键结论能否回到原始证据？
3. 同一 Context 是否被多个角色和任务复用？
4. Agent 是否知道当前代表谁、拥有什么权限？
5. Skill 与人的调用边界是否明确？
6. 是否存在具体 DRI、期限和完成状态？
7. 是否有正常、边界、异常和越权 Eval？
8. 人工修改是否改变下一次执行？
9. 换模型、换入口、换某片叶子后，企业能力是否仍留下？
10. 业务结果是否比原流程更快、更准、更低风险或更能增长？

如果答案仍然主要依靠会议、熟人和管理层手工协调，公司只是在使用 AI，还没有形成 ANC。

---

## 18. 进一步阅读与证据

### 明日阅读入口

[ANC / Enterprise Agent Harness 外部必读清单](./anc-external-reading-list-agent-notes-2026-08-29.md)

### 核心一手来源

1. HA7CH：[0 中层公司](https://www.ha7ch.com/writing/zero-middle-management/zh)
2. HA7CH：[The Enterprise AlphaGo Moment](https://www.ha7ch.com/writing/enterprise-alphago-moment)
3. Andrej Karpathy：[LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
4. YC Software：[QM](https://github.com/yc-software/qm)
5. Block：[From Hierarchy to Intelligence](https://block.xyz/inside/from-hierarchy-to-intelligence)
6. YC：[Within](https://www.ycombinator.com/companies/within)
7. YC：[Agentic Fabriq](https://www.ycombinator.com/companies/agentic-fabriq)
8. Anthropic：[Building effective agents](https://www.anthropic.com/engineering/building-effective-agents)
9. Anthropic：[Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)
10. SAP：[SAP and Anthropic Plan to Bring Claude to SAP Business AI Platform](https://news.sap.com/2026/05/sap-anthropic-to-bring-claude-sap-business-ai-platform/)
11. OpenAI：[OpenAI launches the OpenAI Deployment Company](https://openai.com/index/openai-launches-the-deployment-company/)
12. OpenAI：[Introducing OpenAI Frontier](https://openai.com/index/introducing-openai-frontier/)

### 支持性研究

- [Accenture 与 McKinsey 企业改革](./accenture-mckinsey-enterprise-transformation-2026-08-28.md)
- [Anthropic FDE 方法论](./anthropic-fde-methodology-2026-08-28.md)
- [OpenAI、Anthropic、Accenture、McKinsey FDE 对比](./fde-definitions-accenture-mckinsey-openai-anthropic-2026-08-29.md)
- [Block / YC 外部信号核验](./block-yc-external-signals-verification-2026-08-28.md)
- [腾讯 FDE 报告与 Anthropic 对比](./tencent-fde-report-vs-anthropic-verification-2026-08-29.md)

---

## 19. 最终定义

> **ANC 的最小树干，是企业共同拥有的一套 files/markdown Context，加上能够直接 search/read/grep/edit 的 Agent workspace，以及“工作产物 → 知识 → Skill → 全员复用”的飞轮。Lite 用 Git 把工作区分发到成员本地；Hosted 把工作区和 Agent 放在中心机器，通过飞书、钉钉等入口提供服务。组织扩大后，再按风险逐步补权限、任务、审批、Eval 和审计。**

它的技术价值是让 Agent 操作完整、可版本化的公司 Context；它的组织价值是把个人工作流沉淀为公司能力；它的经营价值是让重复劳动不断被 Skill 吸收，让企业在实际使用中形成能力复利。
