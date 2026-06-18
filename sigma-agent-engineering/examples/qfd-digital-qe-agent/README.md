# QFD 演练：用 QFD 设计"数字质量工程师 Agent"（AB 角互换 · 跨模型对抗评审）

模拟运行（数据为示例值）。客户=工厂质量经理/质量工程师；用 QFD 四阶段把 VOC 翻成 8D 诊断 Agent 的设计。
这是 `sigma-agent-engineering` 的 CDOC Concept→Design 前端的一次具体化，且演练了"运动员≠裁判"的跨模型互审。

## 协议（AB 角互换，铁律：实现者≠评审者）
| 步 | 提法 | 实现 | 评审 |
|---|---|---|---|
| 一 产品规划 HOQ | Codex | Claude | Codex |
| 1.5 CTQ→8D 过渡矩阵 | Claude | Codex | Claude |
| 二 子系统部署矩阵 | Claude | Claude | Codex |
| 三 过程/控制计划矩阵 | Claude | Claude | Codex |
> 步二/三因 codex CLI"实现"调用当日不稳(连接挂/进程泄漏)，降为 codex 只做评审；核心规则(生成者≠评审者)始终守住。

## 跨模型对抗评审"逮到的真错"（这就是效果）
- **步一**（Codex 审 Claude）：HOW-05 单位 ppm 与目标 ≤3‰ 不自洽；屋顶混入非-How 项(报告自动化/专家可编辑性) → 已修。
- **1.5**（Claude 审 Codex）：漏"承接 How"列、Phase二交接缺 How 量化锚 → 记录。
- **步二**（Codex 审 Claude）：技术重要度算术错(S3 写 243、实为 285；S1 写 126、实 >156) → 全表重算修正。
- **步三**（Codex 审 Claude）：S4 知识检索整行漏掉；S2/S8/S9 验收门未量化 → 补回。
> 每一步都被对方逮到自己看不见的盲区——同源自审发现不了，跨模型互审才行（书 ch3/ch18 测量独立性的活例）。

## 文件
- phase1-HOQ.md / phase1.5-transition.md / phase2-subsystems.md / phase3-control-plan.md
- review-phase{1,2,3}_codex.md（Codex 对抗评审原文；1.5 的 Claude 评审见会话记录）
