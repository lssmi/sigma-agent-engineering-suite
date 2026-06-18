# Contributing

欢迎对 Sigma Agent Engineering skill 套件提 issue 与 PR。

## 原则（与方法论一致）

1. **Ground-Truth First**：任何涉及章节锚定、术语 canon、数值的改动，先核对仓内 `sigma-agent-engineering/reference/` 的已核验台账与正式出版书，再动手。不凭印象改。
2. **诚实边界**：示例/案例的数值若为说明性示例，必须显式标注「模拟/示例值」，不得冒充真实生产数据。
3. **单一真理源**：跨 skill 的章节号、术语写法以 `reference/phase-pipeline.md` 与 `reference/terminology.md` 为准；改动需同步，避免漂移。
4. **可调用项诚实**：正文引用的工具/skill，若非本套件内置，须标注「占位示例·非本套件 skill·按环境替换」。

## 提 PR 前

- 确认 7 个 `SKILL.md` 的 frontmatter（`name` 与目录名一致、`description` 含触发条件）未被破坏。
- 确认没有引入指向本机绝对路径（如 `/tmp/...`、`/Users/...`）的死指令。
- 确认没有密钥、凭据、个人隐私信息。

## 结构

```
sigma-agent-engineering/     # 主编排器（SKILL.md + reference/ + examples/）
<6 个子 skill>/              # 各一个 SKILL.md
```

安装方式见 [README](./README.md)。
