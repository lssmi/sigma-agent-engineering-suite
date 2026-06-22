# 方法论账本（Methodology Ledger）

> 本文件是 `sigma-agent-engineering` 主 skill 的「方法论账本」，对应专著《企业 AI Agent：从聊天框到数字员工》（范玉辉，Sigma Engineering 范式）附录 E（方法论出处对照表）与附录 A（全局参考文献）。
>
> 它回答两个问题：① 主 skill 每一个动作背后踩的是哪一门几十年的工业质量传统、对应哪件具体工具、在书里哪一节展开（E 表）；② 这些主张可被核验地引用到哪一条原始来源（A 表 23 条已核验核心来源）。
>
> **诚实声明（来自附录 E 原文）**：这张表也是本书的「账本」。一个常见风险是「将既有方法论重新包装为 AI 概念」。这张表要求对每条做法诚实交代它到底是不是新的——大多数不是。真正新的，是它们被搬到一条会自己续写错误前提、会被人故意投毒的概率执行链之后，组合方式、应用对象与执行顺序发生了改变。**方法论基础未变，变的是组合方式与应用场景。**
>
> **主线锚定**：全书唯一主线是 `R ≈ pⁿ`。精确写法是条件概率连乘 `R = ∏ p_i`（`p_i = P(step_i 正确 | 上游状态、上下文长度、噪声密度、既有错误)`）；`pⁿ` 只是便于建立直觉的乐观基线，真实衰减往往更陡（self-conditioning 式误差传播）。本账本所有做法都按它们「在 `R ≈ pⁿ` 上做什么」（抬 p / 缩 N / 封顶 / 不属于 pⁿ 的正交轴）归位。CDOC 末阶段是 **Capability（能力验证）**，不是 Control。
>
> **来源**：附录 E、附录 A 全文已对八审清样真文件 `28_附录　参考资料与速查表.txt` 逐字核对。23 条来源的卷期号 / 页码字段一律只抄真文件已有的部分；**当前稿不杜撰任何卷期号与页码**（原文体例）。

---

## 第一部分（E）　五组做法 → 方法论传统 → 工具 → 正文位置

附录 E 体例：正文使用业务语言（以 8D Agent 与电商只读决策平台两个案例推导），没有显性引入方法论名称；每一条做法都有出处。下表按全书骨架分四组（主线 / 家族 A / 家族 B / 家族 C）再加正交轴，逐条列出。

读法两用：① 给六西格玛 / 可靠性 / 精益背景的读者一个「对得上号」的确认——你熟悉的每件工具，本书都已映射到 Agent 治理框架，只是应用对象从物理产线转向了大模型这条概率执行链；② 给想往深里走的读者一张索书地图，照「方法论传统」列去翻附录 A 给出的经典即可。

### E.0　主线：复合主导（Compounding Dominance）

| 核心做法 | 方法论出处 | 该传统里对应的工具 / 内核 | 正文位置 |
|---|---|---|---|
| `R ≈ pⁿ`：可靠性是乘出来的，不是加出来的；盯水位不只盯水滴 | 复合误差 / 六西格玛 / 长程可靠性研究 / 控制论积分环节 | 连乘可靠度模型；过程能力把容差翻译成 `pⁿ` 落点；蓄水池 = 对每步小误差的积分 | 总论 0.1–0.2 · 第 1 / 3 章 |

> 主线注解（与铁律一致）：连乘律是双向的——单步 p 每跌一点长链成倍崩塌，每抬一点可胜任任务长度成倍变长，越接近满分放大越猛。`pⁿ` 把每步近似为独立同分布是乐观基线；真实是 `R = ∏ p_i`，且 `p = p(上下文长度)`，随上下文增长而退化（context rot），N 涨 + p 跌双重恶化。

### E.1　家族 A —— 挑对的事 + 挑对的控制档位

> 路由：→ `enterprise-agent-assessment`

| 核心做法 | 方法论出处 | 对应工具 / 内核 | 正文位置 |
|---|---|---|---|
| A1 人要的是结果，不是工具；先确认你在解对的问题 | JTBD（Jobs to be Done）/ 设计思维 | 「用户雇佣产品来完成任务」；以结果而非功能定义需求 | 第 2 章 |
| A2 先分清问题类型，再决定上多重的控制——查表的事别派 LLM，真复杂的事别轻管控裸跑 | Cynefin 框架 + 艾什比必要多样性定律 | 清晰 / 繁杂 / 复杂 / 混乱域分类；控制的多样性须 ≥ 被控对象的多样性 | 第 4 / 5 章 |
| A3 能撤的放手做，不能撤的先问——人工门放在「撤不回」处，不是「没把握」处 | 单向门 / 双向门决策（可逆性） | 不可逆决策须慎、可逆决策可快；人工门按后果可逆性而非置信度放置 | 第 4 / 14 章 |
| A4 先把 must-be 的硬基本功做到位，再谈惊喜功能 | Kano 模型 | must-be / 期望 / 兴奋三类需求；基本型缺失 = 不满意的地板 | 第 5 章 |

### E.2　家族 B —— 把链条做可靠（三手段主战场）

> 路由：→ `agent-control-plane-design`（B2/B3/B4/B5/B6/B11 的状态机·契约·分离·瓶颈）+ `agent-guardrails-config`（B8/B9 的稳健 / FMEA·Safety-I/II）+ `agent-human-loop-design`（B9 接不住时的可逆门）+ `agent-production-gate`（B10 误差预算上线门）

| 核心做法 | 方法论出处 | 对应工具 / 内核 | 正文位置 |
|---|---|---|---|
| B1 差错是乘的，不是加的——所以压的是每步方差，不只是抬均值 | 复合误差 / 六西格玛 | 降方差优先于抬均值；最差档步骤在连乘里最致命 | 第 3 / 8 章 |
| B2 不增值的步骤就是风险，能删就删；做减法优先于再加一道控制 | 精益 Lean + via negativa | 消除浪费 / 流动；每个不增值步 = 一次额外的随机抽签 | 第 10 / 20 章 |
| B3 把质量设计进去：让错的动作根本做不出来，而非事后加护栏 | DFSS + 防错（Poka-Yoke） | 面向六西格玛的设计；工具白名单里根本没有危险动作 | 第 6 / 11 / 13 章 |
| B4 矛盾能化解，不必折中：把确定性和随机性分开，自治与可验证兼得 | TRIZ | 40 发明原理 / 矛盾矩阵 / 分离原理；算力归代码、解释归模型 | 第 8 / 12 章 |
| B5 每步进度写到模型脑子外面——能外置状态，才谈得上续跑 / 回滚 / 被独立检查 | 状态外置 / 检查点（分布式系统） | 外部状态文件 + checkpoint-resume；不能封顶一个只活在上下文里的产物 | 第 4 / 10 / 15 章 |
| B6 按两次等于按一次（幂等），否则每次重试都是一次新的双下单 | 分布式系统幂等性 | idempotency / exactly-once；重试安全的前提 | 第 8 / 10 章 |
| B7 冗余只在「会犯不同的错」时才算数——同模型的三个裁判会一起错 | 共因失效（可靠性工程） | common-cause failure；多样性冗余 vs 同构冗余 | 第 9 章 |
| B8 让它对噪声不敏感，而不是只在干净输入上漂亮 | 田口稳健设计 | 信噪比 / 参数设计；对不可控噪声因子稳健 | 第 7 / 11 章 |
| B9 出错前先想清楚它会怎么错（列得出的）；列不出的意外，就让它只代价一次安全停机 | FMEA（Safety-I）+ Safety-II / 韧性工程 | 失效模式与影响分析；优雅降级 / 安全停机转人 | 第 13 / 17 章 |
| B10 给可靠性记账，像花预算一样花——预算之下别再镀金，把劲使到真瓶颈上 | 可靠性工程 / SRE 误差预算 | error budget；derating 留余量；satisficing 够好就停 | 第 17 / 20 章 |
| B11 系统下限由唯一的瓶颈决定：硬化最脆那一步，别打磨不卡脖子的步 | 约束理论 TOC | 《目标》五步聚焦法；定位并集中硬化最低 p 的步 | 第 16 章 |

### E.3　家族 C —— 越用越好（慢外环 DMAIC）

> 路由：→ `agent-observability-setup`（C1/C2 测量与独立量具 + C3/C4/C5/C6 DMAIC 外环·判例飞轮·验证经济学，对应 P7–P10）。
> 注：**C 家族（C1-C6）全程归 `agent-observability-setup`**（与 `reference/phase-pipeline.md` P7–P10 测量/分析/改进/判例飞轮路由一致）；`agent-production-gate` 承接的是慢外环**终点的 P11 规模化生产准入 gate**（DMAIC 判例飞轮之后的规模化落地，仍属慢外环、但不是 C3-C6 的 DMAIC 改进循环）——勿把 C 家族改进工作错交给生产准入 skill。

| 核心做法 | 方法论出处 | 对应工具 / 内核 | 正文位置 |
|---|---|---|---|
| C1 先量，再改；而且去现场看真产物，不信仪表盘、不信子代理的转述 | DMAIC-Measure + 现地现物（genchi genbutsu） | 测量驱动改进；ground-truth 铁律——看真交付物而非二手转述 | 第 18 / 19 / 21 章 |
| C2 量的人不能是被量的人；而且裁判要去找反例，不是收集对勾 | MSA + 波普尔证伪 | 测量系统分析（量具独立性）；证伪——好检验去找反例 | 第 18 章 |
| C3 别因一次坏结果就改：先分信号与噪声，对每个坏 trace 打补丁会越改越糟 | 休哈特 / 戴明（不 tampering） | 控制图区分共因 / 特殊原因；过度调整的危害（漏斗实验） | 第 21 章 |
| C4 闭环：把每次教训（预想的 + 真发生的）焊成同一道永久的门 | PDCA / 判例飞轮 | 计划-执行-检查-处理循环；C→D 把修复固化成新红线 | 第 14 / 21 章 |
| C5 修系统，别骂模型——「你必须准确!!」不抬地板；改动作空间 / 工具 / 契约 | 戴明 94/6 原则 | 94% 的问题来自系统而非个人；改流程而非训话 | 第 21 章 |
| C6 验证通常比生成便宜——把可靠性预算花在独立检查者上 | 验证者定律 / 生成-检查不对称 | verifier's law；检查成本 < 生成成本时优先投独立验证 | 第 20 / 21 章 |

### E.4　正交轴 II —— 信任与遏制（对抗性，非随机；第三篇）

> 路由：→ `agent-guardrails-config`
>
> **铁律提示**：这条轴对付的是「对抗性」（注入 / 越权），**不是 `pⁿ` 随机误差**。把对抗性硬塞进 `pⁿ`（当成 CpK / 过程能力问题）是错的——这正是它必须从 `pⁿ` 主线里单列出来的原因。

| 核心做法 | 方法论出处 | 对应工具 / 内核 | 正文位置 |
|---|---|---|---|
| II1 把一切外部内容当数据，不当指令 | 提示词注入防御 | untrusted 定界包裹；块内指令不执行、只当数据分析 | 第 13 章 |
| II2 别让一个 agent 同时握有 {私有数据 + 读不可信内容 + 对外发送} | 「致命三联」风险模型 | lethal trifecta；三能力齐聚 = 可被诱导外泄，拆开任一即断链 | 第 13 章 |
| II3 只给这一次任务、这一段时间所需的最小权限，在网关上机械执行，而非好言相劝 | 最小权限 / policy-as-code | least privilege；工具白名单 + 网关强制，零写回靠「没有这个动作」 | 第 13 / 22 章 |

### 附加细化做法登记（不单列、但同样有出处）

正文还随处嵌了一批不单列、但同样有出处的细化做法，一并登记，方便溯源：

| 细化做法 | 方法论出处 / 内核 |
|---|---|
| 前馈：可预见的扰动在入口消除 | 控制论前馈 |
| 留余量别贴红线 | derating（降额） |
| 反馈太慢会震荡、校验离起因越近越好 | 回路延迟 |
| 恢复速度是可靠性的一半 | MTTR |
| 学成功也学差点出事 | near-miss / Safety-II |
| 痛觉报警专线 | algedonic（比尔 VSM） |
| 没测的量一定会漂 | 控制 = 所感的集合 |
| 每个节点自成闭环 | VSM 递归 |
| 杠杆点不在最吵处 | 梅多斯系统杠杆点 |
| 够好就停 | satisficing |
| 暴露前先锁计划 | 防注入（归正交轴 II） |
| 看见的是被设计的输入 | context engineering |
| p 不是常数，长上下文运行后会发生上下文退化 | context rot |

> 对应关系（附录 E 说明）：本表「方法论传统」与「对应工具」两列，与附录 A 的文献维度、总论表 0-1（六门方法论 = 三手段 @ 三高度）**严格对应**；想往源头深钻，照「方法论传统」列去翻附录 A 给出的经典即可。

---

## 第二部分（A）　23 条已核验核心来源清单

附录 A 体例：本书论证有两个不同来源的支柱，**必须分开看**。

- **成熟支（地基直接引）**：几十年沉淀的方法论传统（六西格玛、精益、约束理论、可靠性工程、DFSS、TRIZ、控制论）——经过工业验证的成熟资产，内核几十年未变，挑一本公认经典读透便足够受用很久，引用时可以放心。
- **新近支（标注日期版本，谨慎引）**：近几年关于大模型与 Agent 可靠性的文献——仍在快速演进的活材料。关于长程可靠性、上下文退化、注入防御的研究每隔几个月翻新一轮，今天的最佳实践明年可能被推翻。**每次引用都尽量标注日期与版本，并回最新来源核对**；一本想用十年的书，不应把新近研究当作长期不变的定论写入论证。

> **下面 23 条逐字抄自真文件 `28` 附录 A「已核验核心来源（第三轮）」(file28 第 27–49 行)。仅含真文件已给出的字段（arXiv 号 / DOI / 年份 / 出版社 / 会议）。原文明确：当前稿不杜撰任何卷期号与页码——故下表凡真文件未给页码者一律留空，不补造。**

### 新近支（大模型 / Agent 可靠性；引用须标注日期版本，谨慎引）

| # | 来源（真文件字段） | 标识 | 对应本书 |
|---|---|---|---|
| 1 | Sinha, A., Arun, A., Goel, S., Staab, S., & Geiping, J. (2026). *The Illusion of Diminishing Returns: Measuring Long Horizon Execution in LLMs.* ICLR. | arXiv:2509.09677 | 总论 · 第 1 / 3 章（`R≈pⁿ` 长程执行） |
| 2 | Liu, N. F., Lin, K., Hewitt, J., Paranjape, A., Bevilacqua, M., Petroni, F., & Liang, P. (2024). *Lost in the Middle: How Language Models Use Long Contexts.* Transactions of the Association for Computational Linguistics, 12, 157–173. | DOI: 10.1162/tacl_a_00638 | 第 1 / 3 章（中间迷失 / context rot） |
| 3 | Xu, Z., Jain, S., & Kankanhalli, M. (2024). *Hallucination is Inevitable: An Innate Limitation of Large Language Models.* | arXiv:2401.11817 | 第 1 / 3 章（幻觉不可消除） |
| 4 | Gao, L., Madaan, A., Zhou, S., Alon, U., Liu, P., Yang, Y., Callan, J., & Neubig, G. (2023). *PAL: Program-aided Language Models.* ICML. | arXiv:2211.10435 | 第 6 / 8 章（算力归代码） |
| 5 | Huang, J., Chen, X., Mishra, S., Zheng, H. S., Yu, A. W., Song, X., & Zhou, D. (2024). *Large Language Models Cannot Self-Correct Reasoning Yet.* ICLR. | arXiv:2310.01798 | 第 3 / 9 章（自评偏差 / 生成者≠审查者） |
| 6 | Greshake, K., Abdelnabi, S., Mishra, S., Endres, C., Holz, T., & Fritz, M. (2023). *Not what you've signed up for: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection.* ACM CCS. | arXiv:2302.12173 | 第 13 章（间接注入 / II1） |
| 7 | Tsai, L., & Bagdasarian, E. (2025). *Contextual Agent Security: A Policy for Every Purpose.* | arXiv:2501.17070 | 第 4 / 13 章（指令/工具/数据/上下文四类边界） |
| 9 | Anthropic. (2024). *Introducing the Model Context Protocol.* 官方公告与 Model Context Protocol 文档。 | — | 第 6 章（MCP） |
| 10 | Anthropic. (2025). *Effective harnesses for long-running agents.* Engineering blog, published Nov. 26, 2025. | — | 总论 · 第 10 章（长程驾驭工程 / 状态外置） |
| 11 | Sharma, M., et al. (2023). *Towards Understanding Sycophancy in Language Models.* | arXiv:2310.13548 | 第 1 / 3 章（谄媚性源于 RLHF） |
| 12 | Chen, L., Zaharia, M., & Zou, J. (2023). *FrugalGPT: How to Use Large Language Models While Reducing Cost and Improving Performance.* | arXiv:2305.05176 | 第 20 章（成本工程） |
| 19 | Willison, S. (2025). *The Lethal Trifecta for AI Agents: private data, untrusted content, and external communication*；及 Willison, S. (2023). *The Dual LLM Pattern for Building AI Assistants that Can Resist Prompt Injection.* | — | 第 13 章（致命三联 II2 / 双 LLM 隔离） |
| 20 | Ben Natan, S., & Tsur, O. (2026). *Not Your Typical Sycophant: The Elusive Nature of Sycophancy in Large Language Models.* | arXiv:2601.15436 | 第 3 章（近因偏差 recency） |
| 21 | Yao, S., Zhao, J., Yu, D., Du, N., Shafran, I., Narasimhan, K., & Cao, Y. (2022). *ReAct: Synergizing Reasoning and Acting in Language Models.* | arXiv:2210.03629 | 第 6 章（函数调用 / 推理-行动） |
| 18 | Soder, L., Smakman, J., Dunlop, C., Pan, W., Swaroop, S., & Kolt, N. (2024). *Levels of Autonomy: Liability in the Age of AI Agents.* NeurIPS 2024 Workshop (SoLaR). | — | 第 5 / 14 章（自治层级与责任锚定） |

> **新近支版本提示（真文件原文）**：Agent Harness Engineering、Code as Agent Harness、AI Agent liability/autonomy 等方向已有 OpenReview、arXiv 或行业预印本，但版本和会议状态仍在变动；正式出版时宜以「近年预印本 / 行业实践」谨慎引用，**不应写成已稳定的经典定论**。

### 成熟支（工业质量传统；地基直接引，内核几十年未变）

| # | 来源（真文件字段） | 对应本书 |
|---|---|---|
| 8 | Hollnagel, E. (2014). *Safety-I and Safety-II: The Past and Future of Safety Management.* Ashgate. | 第 13 / 17 章（Safety-I / Safety-II） |
| 13 | Deming, W. E. (1986). *Out of the Crisis.* MIT Press.（94/6 法则、漏斗实验与过度调整 / tampering） | 第 21 章（C3 不 tampering / C5 戴明 94/6） |
| 14 | Beer, S. (1972). *Brain of the Firm.* Allen Lane / John Wiley.（可生机系统模型 VSM 与 algedonic 越级报警回路） | 第 16 章 · 附加细化（VSM / algedonic） |
| 15 | Altshuller, G. S. 发明问题解决理论（TRIZ）：矛盾分析与分离原理（空间 / 时间 / 条件分离）。 | 第 12 章（B4 TRIZ） |
| 16 | Ford Motor Company. (1987). *Team Oriented Problem Solving (TOPS)* —— 8D（Eight Disciplines）问题解决法。 | 总论 · 第 1 章（8D 主案例链条） |
| 17 | Kepner, C. H., & Tregoe, B. B. (1965/1981). *The Rational Manager / The New Rational Manager.* 结构化问题分析与决策。 | 第 4 章（结构化问题分析） |
| 22 | Ficalora, J., & Cohen, L. *Quality Function Deployment and Six Sigma: A QFD Handbook* (2nd ed.). Prentice Hall.（DFSS 的 CDOC 四阶段 Concept–Design–Optimize–Capability 的权威阐述，源自 SBTI） | 第 11 章（CDOC 归属权威） |
| 23 | Creveling, C. M., Slutsky, J., & Antis, D. (2003). *Design for Six Sigma in Technology and Product Development.* Prentice Hall.（CDOV / CDOC 深受 SBTI / S. Zinkgraf 体系影响） | 第 11 章（CDOV / CDOC 体系血缘） |

> **CDOC 归属铁律（来自 file28 #22/#23 + ch11）**：CDOC = **Concept–Design–Optimize–Capability**，是面向六西格玛设计 DFSS 的经典四阶段，源自 **SBTI**（Sigma Breakthrough Technologies, Inc.），权威阐述见 Ficalora & Cohen《QFD and Six Sigma》（QFD Handbook, 2nd ed.），姊妹路线 CDOV 见 Creveling 2003（受 SBTI / S. Zinkgraf 体系影响）。**末阶段是 Capability（能力验证），不是 Control**——Control 是 DMAIC（慢外环）的末阶段，二者不可混。

> **编号说明**：上表编号严格沿用真文件 `28` 附录 A「已核验核心来源（第三轮）」的原始序号 1–23（真文件按主题混排，未按成熟/新近分组；本账本仅做分支重排，编号不变、字段不改）。真文件未提供卷期号与页码的条目一律留空，**遵循原文「当前稿不杜撰任何卷期号与页码」体例**。

---

## 账本使用纪律（给主 skill 各阶段调用）

1. **每个动作背书**：主 skill 流水线任一阶段产出工程判断时，回此账本 E 表指出该判断踩在哪条做法（A1–A4 / B1–B11 / C1–C6 / II1–II3）、出自哪门传统。删掉那个方法论专名，那段道理仍应站得住（附录 E 自检）。
2. **引用分支判据**：引「成熟支」（六西格玛 / 可靠性 / TRIZ / 精益 / DFSS / 控制论）可直接当地基；引「新近支」（长程可靠性 / 上下文退化 / 注入防御 / MCP / Agent 自治）必须标注日期与版本，并提醒回最新来源核对，不得写成稳定经典定论。
3. **诚实交代真新**：任何对外材料用到本范式时，按附录 E 账本诚实区分——绝大多数做法不是新发明，而是既有质量方法论被迁移到「会自己续写错误前提、会被人故意投毒的概率执行链」之后，组合方式 / 应用对象 / 执行顺序发生了改变。
4. **不补造字段**：23 条来源的卷期号 / 页码若真文件未给，一律留空，不据印象或检索结果补造（ground-truth-first 铁律 + 原文体例双重约束）。
