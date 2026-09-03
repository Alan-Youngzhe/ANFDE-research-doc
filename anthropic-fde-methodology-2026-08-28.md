# Anthropic 如何做 FDE：公开证据、可推导方法与未知边界

> 研究日期：2026-08-28  
> 范围：仅使用 Anthropic 官方职位、官方工程文章、官方客户案例和官方合作公告。  
> 证据口径：文中的“官方明确”来自单一官方材料；“综合判断”是多份材料之间的谨慎归纳，不代表 Anthropic 已公开命名这套方法论。

## 一句话结论

Anthropic 的 Forward Deployed Engineering（FDE）不是“给客户做演示”，也不是泛化的驻场顾问，而是：**把工程师直接放进战略客户的真实系统中，用 Claude 构建可投入生产的应用，再把一次性交付沉淀为评测、模板、集成和产品反馈。**

但需要特别强调：Anthropic 公开的 FDE Manager 职位明确说，这支团队仍在从 0 到 1 建设，尚无现成 playbook。因此，外部目前看到的是一套已经成形的“交付闭环”，而不是 Anthropic 正式公开、固定命名的完整 FDE 方法论。[Manager, Forward Deployed Engineering](https://job-boards.greenhouse.io/anthropic/jobs/5385634008)

## 1. Anthropic 对 FDE 的官方定义

美国 FDE 职位把该角色放在 Applied AI 团队，要求工程师直接嵌入最重要的客户团队，在客户系统中构建使用 Claude 的生产应用，交付 MCP servers、sub-agents、agent skills 等技术资产，并把可重复的部署模式反馈给 Product 和 Engineering。岗位还包括白手套式部署支持、长期客户关系和在同一客户生命周期内继续发现 AI 部署机会。[Forward Deployed Engineer](https://job-boards.greenhouse.io/anthropic/jobs/5302966008)

这意味着其核心交付单位不是“建议”，而是三类可验收结果：

1. **生产系统**：Claude 驱动的真实业务应用，而非只停留在 PoC 或演示。
2. **客户专属技术资产**：MCP、子 agent、skills、评测与集成代码。
3. **Anthropic 可复用资产**：部署模式、starter repository、integration template、知识库和产品反馈。

FDE 仍然需要做客户 discovery 和机会识别，但岗位对工程能力的要求很硬：Python、TypeScript/Java 等编程能力，LLM agent、prompt、eval、规模化部署经验，以及实际交付生产应用的记录。[Forward Deployed Engineer](https://job-boards.greenhouse.io/anthropic/jobs/5302966008)

## 2. 从官方岗位拼出的完整 operating loop

以下九步是对 FDE、Technical Deployment Lead、FDE Manager 和 Pre-Sales Program Lead 四类官方岗位的综合，不是 Anthropic 自己发布的一张方法论图。

### 1）选择值得投入的机会

FDE 资源是稀缺的，因此不是有需求就接。Pre-Sales Program Lead 要在商业、研究和使命型项目之间建立优先级，并决定哪些机会由 Anthropic 自己做、交给合作伙伴，或采用共同交付。[Pre-Sales Program Lead, Forward Deployed Engineering](https://job-boards.greenhouse.io/anthropic/jobs/5391012008)

**隐含原则：FDE 的第一步不是写代码，而是判断这是不是值得占用高成本现场工程能力的问题。**

### 2）从机会推进到可交付合同

售前侧负责 qualification、scope、proposal、审批、法务、财务和合作伙伴安排，并把已签约项目以足够完整的上下文交给交付团队。FDE Manager 和 Engagement/Deployment 角色会参与机会资格判断、工作范围与 SOW。[Pre-Sales Program Lead](https://job-boards.greenhouse.io/anthropic/jobs/5391012008)；[FDE Manager](https://job-boards.greenhouse.io/anthropic/jobs/5385634008)

### 3）进入现场做技术与业务 discovery

Technical Deployment Lead 要映射客户的真实 workflow、约束、依赖和组织复杂性，定义 MVP，形成解决方案架构；FDE 则用自身的工程判断验证这些假设能否在客户系统中成立。[Technical Deployment Lead](https://job-boards.greenhouse.io/anthropic/jobs/5017903008)

### 4）在构建前定义“成功”

交付计划必须包含 milestone、dependency、success criteria 和 value hypothesis。官方还要求建立 baseline 与 KPI，做部署前后测量，并向高层赞助者汇报 ROI。[Technical Deployment Lead](https://job-boards.greenhouse.io/anthropic/jobs/5017903008)

Anthropic 面向企业的官方实施指南也给出相同方向：先选用例与模型，设定可测量且与业务目标一致的成功指标，并证明量化回报；从小处开始，经过评测再逐步扩展。[Planning to production: Best practices for implementing AI](https://www-cdn.anthropic.com/2db91550aa050eae0f205b04c908cd32ec1dab4b.pdf)

### 5）在客户真实系统中构建

FDE 负责生产代码，直接在客户系统里构建 Claude 应用以及 MCP、sub-agent、agent skill 等组件。Technical Deployment Lead 则明确“不写生产代码”，但要负责技术方向、产品范围、backlog 和跨团队推进。[Forward Deployed Engineer](https://job-boards.greenhouse.io/anthropic/jobs/5302966008)；[Technical Deployment Lead](https://job-boards.greenhouse.io/anthropic/jobs/5017903008)

这里体现出 Anthropic FDE 的关键边界：**业务价值有人负责，生产工程有人负责，两者紧密配对，但不能把所有职责混成一个“超级顾问”。**

### 6）用评测驱动迭代，而不是靠 demo 感觉

Anthropic 的官方实施循环是 Planning → Prompting → Evaluation → Optimization → Deployment，并要求部署后继续把生产数据反馈到离线评测中。评测应组合代码规则、模型评分和关键环节的人类判断。[Planning to production](https://www-cdn.anthropic.com/2db91550aa050eae0f205b04c908cd32ec1dab4b.pdf)

其最新 agent eval 指南进一步强调：既要看完整轨迹，也要验证环境里的最终状态；要同时维护 capability eval 和 regression eval，并把真实失败转为测试任务。自动评测、生产监控、A/B 测试、用户反馈和人工校准是互补层，而非互相替代。[Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)

### 7）穿过企业落地的非代码阻力

交付不止是模型表现。Technical Deployment Lead 需要处理安全审查、法务、采购、合规、范围变更、跨组织协调和高层沟通，FDE 则需要解决集成、生产故障和质量问题。[Technical Deployment Lead](https://job-boards.greenhouse.io/anthropic/jobs/5017903008)

**综合判断：Anthropic 把“企业采用失败”视为一个社会技术系统问题，而不仅是 prompt 或模型问题。**

### 8）上线、测量并扩大采用

先小规模试点，准备 A/B 测试和人类反馈界面，再持续更新评测；同时比较部署前后的 KPI 与业务结果。FDE 还要建立长期关系，在客户内部继续寻找值得扩展的 Claude 场景。[Planning to production](https://www-cdn.anthropic.com/2db91550aa050eae0f205b04c908cd32ec1dab4b.pdf)；[Forward Deployed Engineer](https://job-boards.greenhouse.io/anthropic/jobs/5302966008)

### 9）把现场经验压缩成产品和组织资产

团队要把成功做法沉淀为 playbooks、starter repositories、integration templates、evaluation frameworks 和内部知识库；同时把现场信号送回 Product、Engineering 和 Research，影响平台、模型和产品方向。[FDE Manager](https://job-boards.greenhouse.io/anthropic/jobs/5385634008)；[Technical Deployment Lead](https://job-boards.greenhouse.io/anthropic/jobs/5017903008)

因此，完整飞轮可以概括为：

> **筛选高价值问题 → 发现真实 workflow → 定义可测结果 → 现场构建 → 评测与上线 → 验证 ROI → 沉淀复用资产 → 反馈产品 → 扩大采用**

## 3. 角色边界

| 角色 | 主要责任 | 明确不是什么 |
|---|---|---|
| Pre-Sales Program Lead | 机会生成、资格判断、项目组合优先级、proposal、签约、partner 路由、售前到交付 handoff | 不是生产方案的构建者 |
| Technical Deployment Lead | SOW 到上线的交付计划、discovery、MVP、backlog、干系人、ROI、范围与组织复杂性 | 官方明确写明不写生产代码 |
| FDE | 在客户系统中写生产代码，构建 Claude 应用、MCP、sub-agent、skills，解决生产问题 | 不是纯售前、纯 PM 或只做 demo 的 Solutions Engineer |
| FDE Manager | 资源配置、技术标准、审架构/代码、debug、人才发展、playbook 和机制 | 不是每个项目的日常项目经理 |
| Engagement Manager | 交付物流和干系人管理，与 FDE/FDE Manager 配对 | 不是主写生产代码的人 |
| Applied AI Engineer | 面向一组客户做技术顾问、原型、评测、workshop 与架构指导 | 相比 FDE，公开描述更偏组合式顾问与平台采用，不强调长期嵌入单一战略客户系统 |
| Product / Engineering / Research | 接收现场信号，改进产品、平台和模型 | FDE 没有被描述为核心产品路线图的最终 owner |

边界依据：[FDE](https://job-boards.greenhouse.io/anthropic/jobs/5302966008)、[Technical Deployment Lead](https://job-boards.greenhouse.io/anthropic/jobs/5017903008)、[FDE Manager](https://job-boards.greenhouse.io/anthropic/jobs/5385634008)、[Applied AI Engineer](https://job-boards.greenhouse.io/anthropic/jobs/5057647008)、[Pre-Sales Program Lead](https://job-boards.greenhouse.io/anthropic/jobs/5391012008)。

## 4. Claude 在 FDE 工作中扮演什么角色

### 客户解决方案的“能力核心”

官方明确写出的交付形态包括 Claude models、MCP servers、sub-agents、agent skills、agentic solutions 和 evaluation frameworks。Claude 并非独立聊天机器人，而是被放进客户数据、工具、权限、业务流程和人类审批组成的系统中。[Forward Deployed Engineer](https://job-boards.greenhouse.io/anthropic/jobs/5302966008)

Anthropic 的 agent 架构原则是从最简单方案开始：能用单次模型调用、检索和上下文示例解决，就不先造 agent；只有当灵活决策的收益足以覆盖延迟和成本时，才提高复杂度。工作流适合可预测任务，agent 适合需要动态决策的任务。[Building effective agents](https://www.anthropic.com/engineering/building-effective-agents)

### FDE 业务运营的“内部工具”

Anthropic 的售前岗位明确要求把 Claude 用于 proposal、deal qualification 和 pipeline tracking。这是目前公开材料中少数直接说明“FDE 团队自己怎样用 Claude 工作”的证据。[Pre-Sales Program Lead](https://job-boards.greenhouse.io/anthropic/jobs/5391012008)

### 不能过度推断的地方

当前 FDE 职位没有明确说 FDE 必须使用 Claude Code，也没有公开其个人开发环境、agent workflow、prompt、代码审查流程或产能数据。因此，只能确认他们交付 Claude 应用以及以 Claude 支持部分售前运营；**不能确认所有 FDE 都以 Claude Code 作为主要编程工具。**

## 5. 公开案例透露出的真实做法

这些案例能说明 Anthropic 如何支持客户落地，但官方没有确认它们都由名为“FDE”的团队负责，因此不能把它们直接称为 FDE 项目。

### Graphite：先建立评测基准，再设计实现与扩容

Graphite 用 500 个真实和合成 PR 建立评测，经过 A/B 测试选出 Claude；Anthropic 团队通过专门 Slack 频道协助设计 eval 与实现方式，并在发布需求激增时帮助扩大 rate limit。Graphite 又把复杂任务拆成步骤，并加入多层验证，只让高质量评论到达用户。[Graphite customer story](https://www.anthropic.com/customers/graphite)

这展示了典型落地链条：**真实数据集 → 模型/架构评测 → 实现指导 → 生产扩容 → 指标反馈。**

### Harvey：领域评测、真实环境、人类 checkpoint 与合规上线

Harvey 使用 BigLaw Bench、真实产品环境和 AI/法律研究人员的开放评估三条线选择模型。其 workflow 会主动追问、展示中间成果，并允许律师在每个 checkpoint 编辑和批准；上线还需要满足安全、隐私、合规和多区域数据处理要求。Anthropic 从领导层到实施团队全程支持，但案例未给出人员角色名称。[Harvey customer story](https://www.anthropic.com/customers/harvey)

这说明高风险行业的 Claude 落地不是“模型一次回答正确”，而是：**领域基准 + 真实环境 + 人类审批 + 合规与区域部署。**

### Ramp：从个人试用到组织采用，并把 Claude 接进真实工具链

Ramp 从工程师个人试验开始扩展，通过直接沟通渠道向 Anthropic 反馈问题；内部把 Claude Code 接入测试框架、Datadog/Sentry、项目管理系统与数据仓库，形成测试修复、incident triage 和 ticket-to-code 等真实工作流。案例报告 50% 工程师周活，内部 incident 工具把初始调查时间最多缩短约 80%。[Ramp customer story](https://www.anthropic.com/customers/ramp)

这说明扩大采用依赖的不是一次培训，而是：**贴进原有工具链、让反馈能直接到产品方、用可见指标证明重复价值。**

## 6. Anthropic 如何扩张 FDE 能力

Anthropic 并不只依靠自己的 FDE。官方与 UST 的合作公告说，UST 的 forward-deployed engineers 会与客户团队并肩工作，Anthropic 则通过 Claude Partner Network 提供 enablement、技术指导和认证；UST 同时先在自己的工程组织内验证 Claude，再带进客户系统。[UST partnership](https://www.anthropic.com/news/ust-claude)

Accenture 合作也采用类似模式：由大规模、具行业知识的 forward-deployed/reinvention deployed engineers 把 Claude 嵌入客户环境，而 Anthropic 提供模型、Claude Code 和受监管行业 playbooks。[Accenture partnership](https://www.anthropic.com/news/anthropic-accenture-partnership)

**综合判断：Anthropic 的规模化路径是“少量高杠杆自有 FDE + 系统集成商/咨询伙伴的现场交付网络 + 可复用技术与行业 playbook”。**

## 7. 驻场强度

官方使用 “embed directly” 和 “build in person”，但并未说 FDE 长期全职常驻单一客户。美国 FDE 预计约 25% 客户现场差旅；巴黎、慕尼黑 FDE 以及管理/交付岗位多为 25%–50%。这更接近在 kickoff、高优先级阶段和关键阻塞点深入现场，而不能直接等同于传统外包式常驻。[US FDE](https://job-boards.greenhouse.io/anthropic/jobs/5302966008)、[Paris FDE](https://job-boards.greenhouse.io/anthropic/jobs/5391021008)、[Munich FDE](https://job-boards.greenhouse.io/anthropic/jobs/5391016008)。

## 8. 置信度与未知项

### 高置信度

- FDE 属于 Applied AI，直接嵌入战略客户并写生产代码。
- 核心技术交付包含 Claude 应用、MCP、sub-agent、agent skills。
- Technical Deployment Lead/Engagement Manager 与 FDE 存在清楚的产品、项目和代码责任边界。
- 成功标准不仅是模型质量，还包括业务 KPI、ROI、安全、合规和采用。
- 每次交付都应产生可复用资产并把现场信号回流到产品/工程/研究。

### 中等置信度（跨材料综合）

- 九步 operating loop 是 Anthropic 当前实际形成中的 FDE motion。
- 自有 FDE 更可能服务少量高战略性客户，更多规模通过伙伴网络完成。
- kickoff、复杂 discovery、生产阻塞和高层对齐是现场参与最密集的时刻。

### 官方材料仍未回答

- 单次 engagement 的典型时长，以及一位 FDE 同时服务多少客户。
- 每个项目的标准人员配置和 FDE/TDL 比例。
- 代码与技术资产最终归客户、Anthropic 还是共建仓库。
- 上线后的维护期、退出条件、SLA 和 clean handoff 方式。
- FDE 绩效中收入、产品采用、ROI、代码质量各占多少。
- FDE 是否普遍用 Claude Code，以及其具体内部 agentic engineering 流程。
- 是否存在未公开、正式命名的内部 FDE playbook。

## 9. 最准确的“Anthropic FDE 方法论”表述

如果必须把公开材料压缩成一句方法论，最稳妥的版本是：

> **选择值得用前沿模型解决的高价值 workflow，和客户一起把成功标准与 ROI 定清楚；让能写生产代码的工程师进入真实系统，用最简单可行的 Claude 架构做出可评测、可监管的业务结果；上线后把生产反馈变成回归评测、复用资产和产品改进。**

这句话是证据驱动的综合，不是 Anthropic 的官方 slogan。

## 主要一手来源

1. [Forward Deployed Engineer](https://job-boards.greenhouse.io/anthropic/jobs/5302966008)
2. [Manager, Forward Deployed Engineering](https://job-boards.greenhouse.io/anthropic/jobs/5385634008)
3. [Technical Deployment Lead](https://job-boards.greenhouse.io/anthropic/jobs/5017903008)
4. [Pre-Sales Program Lead, Forward Deployed Engineering](https://job-boards.greenhouse.io/anthropic/jobs/5391012008)
5. [Applied AI Engineer, Enterprise Tech](https://job-boards.greenhouse.io/anthropic/jobs/5057647008)
6. [Planning to production: Best practices for implementing AI](https://www-cdn.anthropic.com/2db91550aa050eae0f205b04c908cd32ec1dab4b.pdf)
7. [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents)
8. [Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)
9. [Graphite customer story](https://www.anthropic.com/customers/graphite)
10. [Harvey customer story](https://www.anthropic.com/customers/harvey)
11. [Ramp customer story](https://www.anthropic.com/customers/ramp)
12. [UST partnership](https://www.anthropic.com/news/ust-claude)
13. [Accenture partnership](https://www.anthropic.com/news/anthropic-accenture-partnership)
