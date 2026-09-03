# 活水实业 ANC 应用蓝图

日期：2026-08-29  
性质：基于现有访谈、结构化调研、方案草案和真实样表的架构重构建议；不是已经部署或验证完成的系统。

## 0. 结论先行

活水现有方案已经有不少正确零件，但整体仍然是：

```text
统一数据库
+ 钉钉/H5 入口
+ 若干自动化与 AI 工具
= 轻量数字化中台 + AI 叶子
```

要变成 ANC，组织方式要换成：

```text
Data：旧系统和原始凭证继续保留事实
Context：围绕一份租赁履约关系关联对象、状态、责任、权限和证据
Harness：Agent 按 Work Graph 调用 Skill 或人，并经过 Eval/审批
Gateway：员工继续从钉钉、表单、Web、文件上传进入工作
+ 每次真实工作产生 Artifact，反哺 Context、Decision、Skill 和 Eval
```

最关键的变化不是换技术栈，而是换“系统的基本单位”：

> **不要以部门表格或 AI 功能为基本单位；要以一个客户/企业从意向、签约、入驻、履约、催收直到退租的完整租赁生命周期为基本单位。**

本文把这个核心对象暂命名为 `TenancyCase`（租赁履约单）。名称可以与活水管理层再确认。

---

## 1. 活水现有 Context 的事实基础

依据：[活水实业 AI 落地调研资料结构化整理](/Users/alanyu/Desktop/活水实业ai落地企业/活水实业ai落地企业_合并_结构化整理.md)、[现有 AI 落地方案](/Users/alanyu/Desktop/活水实业ai落地企业/方案/活水实业AI落地方案.md)及其[技术附录](/Users/alanyu/Desktop/活水实业ai落地企业/方案/活水实业AI落地方案_技术附录.md)。

当前已经确认的公司级断点：

1. 客户、合同、房间、收付款和审批分散在钉钉 OA、钉盘、云平台/云物管、金蝶、微信、Excel、Word/WPS、本地档案；
2. 表之间缺少稳定主键，同一客户、房号、合同和付款无法可靠关联；
3. 招商是财务、物业、运营、公寓服务的数据上游，但没有一次性、可追踪的数据交付；
4. 金蝶房租收入与合同条款、面积、租期、递增、渠道和签约率需要人工匹配；
5. 物业催收、付款截图、财务开票核销之间经过微信群和人工二次转发；
6. 工程合同、钉钉付款审批和历史付款记录没有形成连续项目 Context；
7. 管理层得到的是汇总结果，不容易回到原始材料、当时判断和责任人。

这些问题与 ANC 的切入点高度吻合：活水不是缺更多 AI 工具，而是缺一条跨系统、跨部门、跨人员的经营主线。

---

## 2. 现有方案与 ANC 四层的差距

| ANC 层 | 活水现有零件 | 当前状态 | 还缺什么 |
|---|---|---|---|
| Data | 金蝶、钉钉 OA、云物管、Excel、合同/截图、CloudBase 文件存储 | 来源较清楚 | source URI、版本、hash、业务对象引用和事实权威规则 |
| Context | CloudBase 数据字典：房间、客户、合同、付款、工程、催收等 | 有关系型表草案 | 以 `TenancyCase` 为中心的跨系统 Context Graph、责任、权限、决策和证据链 |
| Harness | 云函数、WorkBuddy、Codex/Kimi、定时同步 | 有工具和自动任务 | Work Graph、Agent 调 Skill/人的路由、动态 DRI、审批门、Eval、反思和 Skill 晋升 |
| Gateway | 钉钉 AI 表格、OA、H5、Web、文件上传 | 已设计较完整 | 统一任务入口、用户身份继承、按任务展示 Context，而不只是岗位视图 |

因此，CloudBase PostgreSQL 不是树干本身。它可以承载部分 Data、Context Graph 和运行状态；钉钉/H5 是 Gateway；云函数、合同生成、对账脚本是 Skills。真正的树干是把这些零件按同一个 Context 和行动闭环组织起来的 Harness。

---

## 3. 活水的核心业务对象应该是什么

### 3.1 不应该以“部门”作为主结构

如果按部门建设：

```text
招商表 + 财务表 + 物业表 + 运营表 + 管家表
```

最终只是把原来的 Excel 孤岛搬到云端。每个部门仍然维护自己的局部事实，跨部门时继续复制和解释。

### 3.2 以租赁履约单 TenancyCase 为主线

一个 `TenancyCase` 表示：

> 某个客户/企业，对某个房间或空间，在某个合同版本和期限内，活水承诺完成入驻、交付、收费、服务和退租的一条完整经营关系。

它至少关联：

```text
企业 Company
空间 Room/Space
租赁合同 LeaseContract + Version
招商线索 Lead
钉钉审批 Approval
交房/退房 Handover
应收 Receivable
收款 Payment
发票/核销 Invoice/Reconciliation
催收 Collection
报修/服务 WorkOrder
Artifact 原始材料
Task / Decision / DRI
```

最小 Context Graph：

```text
Company ──occupies──> Space
   │                    │
   └──signs──> LeaseContract ──governs──> TenancyCase
                         │                    │
                         ├──creates──> Receivable
                         ├──requires──> Handover
                         ├──evidenced_by──> Artifact
                         └──approved_by──> Approval

Payment ──settles──> Receivable
Collection ──follows_up──> Receivable
Task ──about──> TenancyCase ──owned_by──> DRI
Decision ──changes──> TenancyCase / Skill / Eval
```

这个 Graph 不要求重建云物管或金蝶。节点可以只保存 `external_ref` 和必要快照，点击时回到原系统或原始文件。

### 3.3 第二条独立主线：工程项目履约单

活水还有一条不应强行塞进租赁主线的业务对象：`ProjectCase`（工程项目履约单）。

```text
工程项目 → 合同 → 付款节点 → 钉钉审批 → 实付 → 质保金 → 验收/质保期
```

它与 `TenancyCase` 共用同一套权限、Artifact、Task、Decision、Skill、Eval 和 Gateway，但业务 Context 独立。

---

## 4. 活水 ANC 四层的具体实现

## 4.1 Data：原系统继续保留事实

不要一开始把所有系统的数据完整复制到 CloudBase。先为每类数据确定事实源：

| 数据 | 第一事实源 | ANC 中保存什么 |
|---|---|---|
| 合同和合同版本 | 已签合同原件/钉盘/云物管 | 文件 URI、hash、抽取字段、版本、人工确认状态 |
| 钉钉审批 | 钉钉 OA | approval_id、状态、申请人、审批人、原链接、关键字段 |
| 会计记录/房租收入 | 金蝶导出或金蝶 | 导入批次、原行引用、标准化结果、匹配状态 |
| 房源状态 | 与活水确认云物管还是指定主表 | external_ref、必要状态快照、更新时间 |
| 付款截图/客户沟通 | 微信转发到固定收件口 | 原图、发送人、关联对象、待确认状态；不自动监听个人微信 |
| 员工修改和审批理由 | ANC Harness | 完整 Artifact、actor、time、before/after、reason |

Data 层的第一条纪律：任何 AI 结论必须能回到原始记录，不能只保存模型生成的结果。

## 4.2 Context：两个不同的存储面

### A. Operational Context Graph

负责当前正在发生的业务：

- 这个客户是谁、别名是什么；
- 当前租的是哪个房间；
- 生效的是哪个合同版本；
- 应收、实收、核销差在哪里；
- 当前卡在哪个审批或交接任务；
- DRI 是谁；
- 哪些人有权查看金额、合同、联系方式和付款信息；
- 每个判断对应哪个原始 Artifact。

第一版可用 PostgreSQL 通用表实现：

```text
artifacts(id, kind, source_uri, hash, actor, audience, created_at)
context_nodes(id, type, external_ref, state_json, version, audience)
context_edges(from_id, relation, to_id, evidence_id, valid_from, valid_to)
tasks(id, objective, state, dri_id, due_at, required_scope)
decisions(id, question, decision, evidence_ids, decider, effective_at)
runs(id, task_id, actor, trace, result, eval_result)
```

### B. Organizational Memory / LLM Wiki

负责稳定经验和解释：

- 活水房号、客户名称和渠道的规范；
- 自如换房为什么按 50% 而不是 100%；
- 提前解约、非自如渠道、递增 6%、非整月租期如何处理；
- 哪类付款截图不足以直接核销；
- 哪类退租必须经过哪些确认；
- 某次异常为什么由人工否决；
- 已验证的工程合同模板和付款节点规则。

Wiki 不保存实时应收金额作为唯一真相，只保存规则、解释、案例边界和 source refs。

## 4.3 Harness：让 Agent 调 Skill，也调人

### 最小 Skill Registry

| Skill | 输入 | 输出 | 必须调用人的情况 |
|---|---|---|---|
| `normalize-company-room` | 客户名、房号、历史别名 | 标准 company/space ID、候选匹配 | 多个候选或无证据匹配 |
| `extract-lease-contract` | 合同 PDF/扫描件 | 条款草稿、版本、来源定位 | 金额/期限/递增条款低置信 |
| `create-handoff-package` | 已确认 TenancyCase | 财务、物业、运营、管家各自任务包 | 缺必填字段或审批未完成 |
| `match-rent-settlement` | 金蝶批次、合同、渠道规则 | 匹配结果、异常清单、计算证据 | 换房、提前解约、重复、别名冲突 |
| `generate-collection-draft` | 应收、逾期、客户 Context | 催收任务、函件草稿、证据包 | 金额争议、重大客户、承诺变更 |
| `reconcile-payment` | 付款截图、银行/金蝶记录、应收 | 待核销建议 | 付款人/金额/账期不一致 |
| `generate-handover-doc` | 合同、空间、日期、要求 | 交房/退房文件草稿 | 特殊工程或押金争议 |
| `generate-project-contract` | 工程字段、模板、付款比例 | 合同草稿、节点金额、台账 | 非标准条款、比例异常、高金额 |

每个 Skill 都必须有：

```text
input schema
output schema
required permission
source requirements
confidence / exception rules
human approval rule
eval suite
version
owner
```

### 人也是可调用能力

| 人/角色 | Agent 何时调用 |
|---|---|
| 招商 | 客户身份不完整、合同意图或渠道来源不清 |
| 财务 | 金额、账期、核销、服务费和异常条款判断 |
| 物业 | 交退房现场、工程状态、客户关系和催收沟通 |
| 运营 | 入驻/迁出状态、OPC 材料、统计口径 |
| 总经理/老板 | 高金额、重大客户、跨部门冲突、策略例外 |

Agent 调用人时，不能只发一句“请处理”。任务包要包含目标、当前状态、已完成步骤、原始证据、候选方案、需要人回答的问题和截止时间。

## 4.4 Gateway：保留员工现有入口

| Gateway | 用途 | 边界 |
|---|---|---|
| 钉钉 OA | 审批事实和组织身份 | 不重做审批系统 |
| 钉钉 AI 表格 | 人工确认、岗位/任务视图、异常队列 | 不作为全部事实的唯一数据库 |
| H5/Web 表单 | OPC、工程登记、付款截图、外部资料上传 | 写入前校验身份和 scope |
| 文件上传/Codex | 月度金蝶导出和复杂批量处理 | 必须保留原文件和导入批次 |
| 微信人工转发 | 客户付款、报修和临时材料 | 第一阶段不监听个人微信，不自动群发 |
| 管理层视图 | 风险、卡点、待决策任务 | 每个结论可下钻到证据和 DRI |

Gateway 的核心不是“让所有人都能问 AI”，而是让每一次真实工作进入同一条可追踪的 Context 和任务链。

---

## 5. 一条真实业务怎样在 ANC 中运行

以“一个新租客户完成签约并进入履约”为例。

### 5.1 进入

1. 招商从钉钉/H5 提交客户、房间、合同及渠道材料；
2. 系统保存原件，生成 Artifact；
3. `normalize-company-room` 识别客户和空间；
4. `extract-lease-contract` 生成条款草稿；
5. 招商确认关键字段后，创建/更新 `TenancyCase`。

### 5.2 Context 组装

Harness 关联：

```text
客户 + 房间 + 合同版本 + OA 审批 + 渠道 + 应收规则 + 相关原件
```

并检查是否缺少：联系人、租期、面积、租金、押金、账期、递增、开票信息、交房时间、特殊要求。

### 5.3 任务和责任

`create-handoff-package` 不复制四张表，而是从同一个 `TenancyCase` 产生四个权限不同的任务视图：

- 财务：建立应收、开票和核销 Context；
- 物业：准备交房、现场要求和联系人；
- 运营：更新入驻/迁出和园区统计；
- 管家/客服：建立服务、报修和沟通 Context。

每个任务都有 DRI、截止时间、状态、证据和完成 Eval。

### 5.4 执行和确认

- 标准字段可由 Skill 自动生成；
- 金额、合同版本、高风险动作必须人工确认；
- 某部门修改客户名或房号时，系统不能直接覆盖其他部门，而要形成变更 Artifact 和冲突任务；
- 审批通过后，受影响任务自动继续。

### 5.5 反馈生长

例如财务发现：

> “该客户是换房，不应按新签 100% 服务费，应按 50%。”

系统应留下：

```text
原始合同和历史房间
Agent 原方案
财务修改
修改理由
确认人
最终结算结果
```

一次修改先更新本次 `TenancyCase`；同类情况多次出现并通过 Eval 后，才更新 `match-rent-settlement` Skill 和相应测试用例。这就是“使用即搭建”。

---

## 6. 权限与动态 DRI

### 6.1 权限不能只做到“按表授权”

最小权限矩阵：

| 角色 | 可以看到 | 可以行动 | 不应默认看到/修改 |
|---|---|---|---|
| 招商 | 客户、房间、合同、跟进、交付状态 | 创建客户/合同草稿、补材料 | 银行流水、完整财务核销、其他客户敏感信息 |
| 财务 | 合同金额、应收、付款、核销、渠道规则 | 确认匹配、核销、提出异常 | 物业现场记录、非必要客户沟通全文 |
| 物业 | 房间、联系人、交退房、催收任务 | 更新交接和沟通结果 | 完整银行信息、其他合同财务细节 |
| 运营 | 企业状态、入驻迁出、OPC、统计字段 | 确认统计状态 | 银行流水、非必要合同金额 |
| 管理层 | 汇总、风险、卡点和重大事项证据 | 任命 DRI、批准例外、改变优先级 | 不应绕过业务 Owner 无痕改事实 |
| Agent/Skill | 完成当前任务必需的最小 Context | 在授权范围内建议/执行 | 继承不到的其他 scope |

权限判断必须发生在 Context 组装和工具调用之前。

### 6.2 DRI 按结果动态产生

传统方式可能是“这件事属于财务部”。ANC 要表达为：

```text
目标：在 8 月 5 日前确认 7 号楼 207 本期应收并完成核销
DRI：某财务人员
可调资源：合同 Context、金蝶批次、付款截图、核销 Skill、物业联系人
升级条件：金额不符/付款人不符/重大客户争议
完成 Eval：金额一致 + 原始凭证存在 + 核销状态写回
```

部门仍然存在，但责任围绕具体结果和期限产生。

---

## 7. Evals：怎样证明它不是“会聊天的数据库”

### 7.1 技术 Evals

| Eval | 合格标准 |
|---|---|
| 来源追溯 | 每个关键字段和结论能回到原合同、审批、Excel 行或截图 |
| 对象关联 | 客户/房号/合同/付款正确挂到同一 TenancyCase |
| 版本判断 | 能识别当前生效合同和历史版本，不混用 |
| 权限隔离 | 越权用户在模型调用前拿不到敏感 Context |
| 幂等 | 同一 OA 审批或导入批次不会重复建记录 |
| 失败显式 | 无法匹配、证据冲突、工具失败时进入异常队列，不伪装成功 |
| 人工修改留痕 | before/after/reason/actor/time 完整 |

### 7.2 业务 Evals

第一阶段不要以“问答准确率”作为主指标，而应测：

- 新租数据从招商交付到财务/物业/运营的缺字段率；
- 房租结算自动匹配率和人工修改率；
- 每个异常从发现到找到 DRI 的时间；
- 一笔结算回到原合同/金蝶行的时间；
- 工程付款审批查历史所需时间；
- 重复录入次数；
- 第二个月相同任务是否比第一个月更快、异常是否减少。

---

## 8. 第一阶段该从哪里切

### 8.1 不能只做“房租结算 Agent”

房租结算价值高、已有样表，但单独交付只会成为财务叶子。正确方式是：

> **以房租结算作为价值证明点，以 TenancyCase 作为树干切口。**

也就是“一个面 + 一个点”：

- 面：客户从签约、入驻、收费、催收到退租的租赁履约；
- 点：选一个楼栋/一个月度批次，跑通合同—金蝶收入—渠道规则—结算—人工确认—最终结果的证据链。

### 8.2 四周试点

#### 第 1 周：建立最小 TenancyCase

- 选一个楼栋、20–30 个真实客户案例；
- 确认 Company、Space、LeaseContract、Receivable、Payment、Artifact 的主键和事实源；
- 建通用 Context node/edge 和 source refs；
- 不迁移全公司历史数据。

#### 第 2 周：跑通房租结算 Skill

- 导入一个月金蝶批次和对应合同；
- 实现客户/房号标准化、合同匹配、服务费计算；
- 把换房、提前解约、非自如、递增、非整月纳入异常队列；
- 财务确认每个结果和理由。

#### 第 3 周：让 Context 被第二个角色复用

- 选择催收或招商数据交付中的一个；
- 证明同一个 TenancyCase 能同时支持财务和物业/招商，而不是再建一张孤立表；
- 加入 DRI、权限和任务状态。

#### 第 4 周：Eval 与反馈晋升

- 用正常、边界、异常、权限违规案例跑 Eval；
- 比较第一批和第二批处理时间、修改率、漏项；
- 把稳定规则做成 Skill v1；
- 输出是否进入 48 天真实使用期的 Go/No-Go 决策。

### 8.3 Go/No-Go 条件

进入下一阶段至少需要：

1. 关键字段均有原始证据引用；
2. 财务确认匹配结果足以减少真实工作，而不是增加复核负担；
3. 同一个 TenancyCase 被至少两个角色复用；
4. 越权测试通过；
5. 异常和失败不会被系统吞掉；
6. 业务 Owner 愿意指定真实 DRI 并连续使用四周。

---

## 9. 技术部署建议

第一版可以继续沿用现有 CloudBase 方向，但重新划分职责：

```text
CloudBase/PostgreSQL
  ├── context_nodes / context_edges
  ├── tasks / decisions / runs / grants
  └── 选定业务字段的 materialized views

对象存储/钉盘
  └── 合同、截图、Excel 批次、审批附件等 Artifact

Markdown + Git
  ├── company wiki
  ├── decisions
  ├── skills
  └── evals

Agent Harness
  ├── identity / scope / policy
  ├── context assembler
  ├── task & DRI router
  ├── skill registry
  ├── human approval
  └── trace / eval / reflection

Gateway
  ├── 钉钉 OA / AI 表格
  ├── H5 / Web
  ├── Codex 文件处理入口
  └── 管理层任务与风险视图
```

第一版不需要：

- Neo4j 或完整企业知识图谱；
- 全量历史档案扫描；
- 向量数据库作为默认中心；
- 重做金蝶、云物管和钉钉 OA；
- 自动监听个人微信；
- 一次上线所有部门 Agent；
- 让 Agent 自动付款、自动核销或自动对外承诺。

---

## 10. 这项交付与普通 SaaS 项目的区别

| 普通叶子交付 | 活水 ANC 树干交付 |
|---|---|
| 做一个房租结算工具 | 建立 TenancyCase，房租结算是第一个 Skill |
| 做一个合同生成器 | 合同、版本、付款节点和审批成为 ProjectCase Context |
| 做一个 OPC 表单 | OPC 材料进入统一 Artifact/Context/Task 链 |
| 做一个管理大屏 | 管理层看的是实时任务、DRI、证据和待决策事项 |
| 交付功能后继续维护功能 | 交付 Context/Skill/Eval 的生长机制，企业使用时继续搭建 |

最终判断：

> 活水不是先做“企业大脑”，也不是先做一个全公司数据库。它应该从一份真实租赁履约关系开始，把分散事实组织成可追溯 Context，让 Agent、Skill 和人围绕同一结果工作，再把人工修改和最终结果沉淀回组织能力。这样，房租结算、催收、交退房、OPC 和工程付款才不是五片孤立叶子，而是从同一套树干上长出来的能力。

---

## 11. 开工前必须向活水确认的问题

1. 租赁生命周期的业务 Owner 是谁？谁有权确定字段和事实源？
2. 房源状态究竟以云物管、钉钉表格还是其他系统为准？
3. 合同编号是否稳定唯一？历史换房、续签、退租如何表示版本关系？
4. 哪些财务数据允许进入 ANC，哪些只能留在金蝶？
5. 招商、财务、物业、运营分别能看到哪些字段和原件？
6. 一次租赁信息变更由谁确认，怎样通知下游？
7. 选哪个楼栋和哪一个月作为 20–30 个真实试点案例？
8. 财务愿意提供哪些人工修改理由，用于建立第一版 Eval？
9. 哪些动作只能建议，哪些可以自动写状态，哪些必须审批？
10. 四周后用什么业务指标决定继续或停止？
