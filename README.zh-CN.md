<div align="center">

# 🛠️ my-skills

[English](./README.md) | **简体中文**

![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)
![Skills](https://img.shields.io/badge/skills-2-brightgreen.svg)
![Agents](https://img.shields.io/badge/agents-Claude%20Code%20%7C%20OpenCode%20%7C%20Codex%20%7C%20Hermes%20%7C%20OpenClaw%20%2B50-orange.svg)

📖 把软件工程经典书籍蒸馏成**可安装、可执行的操作型 Agent Skill** ——
不是读书笔记，而是编码 agent 真正能执行的工作流。

</div>

---

## 📚 Skill 目录

| Skill | 📕 出处 | ⚙️ 做什么 | 💬 触发场景 |
|-------|---------|-----------|-------------|
| 🔧 [**fowler-refactoring**](./fowler-refactoring/SKILL.md) | Fowler《重构》第 2 版 | 诊断坏味道（24 项映射表）→ 选择目录技法 → 两顶帽子规则下小步执行并逐步验证 | 「重构这段代码」「有什么坏味道」「不改行为地清理代码」 |
| 🧪 [**myers-art-of-software-testing**](./myers-art-of-software-testing/SKILL.md) | Myers《软件测试的艺术》第 3 版 | 破坏性用例设计：黑盒 EP+BVA+因果图，再白盒补分支；单元与多模块接缝配方；固定用例表格 | 「设计测试用例」「给这个函数写测试」「评审我的用例」 |

每个 skill 文件夹包含 `SKILL.md`（YAML frontmatter + 操作指令）和按需加载的
`references/` 目录——采用官方 skill 仓库
（[anthropics/skills](https://github.com/anthropics/skills)）的**渐进式披露**结构。

## 🚀 快速开始

```bash
# 安装全部 skill 到当前项目（所有已检测到的 agent）
npx skills add <YOUR_USER>/my-skills --all -y

# 或全局安装（本机所有项目可用）
npx skills add <YOUR_USER>/my-skills --all -g -y
```

基于 [skills CLI](https://github.com/vercel-labs/skills)（[skills.sh](https://skills.sh/)）。⚡

## 🤖 支持的 Agent

| Agent | 📦 安装命令 | 📂 Skill 目录（项目级 / 用户级） |
|-------|-------------|----------------------------------|
| **Claude Code** | `npx skills add . --all -a claude-code -y` | `.claude/skills/` · `~/.claude/skills/` |
| **OpenCode** | `npx skills add . --all -a opencode -y` | `.opencode/skill/` · `~/.config/opencode/skill/` |
| **Codex** | `npx skills add . --all -a codex -y` | `.agents/skills/` · `~/.codex/skills/` |
| **Hermes Agent** | `npx skills add . --all -a '*' -y` | 自动检测——用 `npx skills list` 查看 |
| **OpenClaw** | 手动复制（见下方折叠） | `<workspace>/skills/` · `~/.openclaw/skills/` |
| Cursor / Copilot / Gemini CLI 等 50+ | `npx skills add . --all -a '*' -y` | 见 `npx skills list` |

> 💡 安装后**重启 agent 会话**，新 skill 才会被发现。

<details>
<summary><b>🧰 完整安装矩阵（本地路径、GitHub、单个 skill、手动复制）</b></summary>

### 从本地克隆安装

```bash
cd /path/to/my-skills

npx skills add . -l                      # 列出可用 skill
npx skills add . --all -y                # 全部 → 当前项目
npx skills add . --all -g -y             # 全部 → 用户全局
npx skills add . --skill fowler-refactoring -y            # 单个
npx skills add . --skill myers-art-of-software-testing -y
npx skills add . --all -a '*' -y         # 一次装到所有已识别的 agent
```

或使用 npm 快捷脚本：

```bash
npm run list              # 列出 skill
npm run install:local     # 当前项目
npm run install:global    # 用户全局（-g）
npm run install:claude    # 仅 Claude Code
npm run install:opencode  # 仅 OpenCode
npm run install:codex     # 仅 Codex
```

### 从 GitHub 安装

```bash
npx skills add YOUR_USER/my-skills --all -y
npx skills add YOUR_USER/my-skills --all -g -y
npx skills add YOUR_USER/my-skills --skill fowler-refactoring -y
npx skills add https://github.com/YOUR_USER/my-skills --all -y
```

### 手动复制（不依赖 npx）

每个 skill 文件夹（含 `references/`）必须整体复制：

```bash
# Claude Code（用户级）
mkdir -p ~/.claude/skills
cp -R fowler-refactoring myers-art-of-software-testing ~/.claude/skills/

# OpenCode（项目级；真实拷贝，不要用 symlink）
mkdir -p /your/project/.opencode/skill
cp -R fowler-refactoring myers-art-of-software-testing /your/project/.opencode/skill/

# Codex（用户级）
mkdir -p ~/.codex/skills
cp -R fowler-refactoring myers-art-of-software-testing ~/.codex/skills/

# OpenClaw（用户级）
mkdir -p ~/.openclaw/skills
cp -R fowler-refactoring myers-art-of-software-testing ~/.openclaw/skills/
```

Hermes Agent：按其文档把 skill 文件夹整体复制到对应 skills 目录，保持目录结构不变。

### 更新 / 卸载

```bash
npx skills list
npx skills update -y
npx skills remove fowler-refactoring
npx skills remove myers-art-of-software-testing
```

</details>

## 💬 使用

skill 通过自然语言触发——直接描述任务，或显式点名 skill。
skill 会**用你的语言回答**（中文、English 均可）。🌏

**🔧 fowler-refactoring**

```text
重构这个函数——它有哪些坏味道？该用哪些技法？

对这个文件做 Extract Class，小步执行，保持行为不变。

这个模块现在该重构，还是等发版之后？
```

**🧪 myers-art-of-software-testing**

```text
为下面函数设计测试用例：def is_isosceles_triangle(a, b, c) -> bool

黑盒 EP+BVA，输出用例表（ID/Inputs/Expected/Tech/Pri）。

两个模块 A→B：给出接缝用例和失败传播的端到端用例。

评审我现有的测试用例，缺什么补什么。
```

> 📋 期望输出风格：**表格而非长文**——诊断表、用例表、Stub 映射表，
> 带明确优先级和一行覆盖说明。

## 📁 仓库结构

```
my-skills/
├── README.md                 # English
├── README.zh-CN.md           # 简体中文（本文件）
├── LICENSE
├── package.json
├── fowler-refactoring/
│   ├── SKILL.md              # 操作型工作流（始终加载）
│   └── references/
│       ├── smell-map.md      # 24 坏味道 → 技法映射（按需加载）
│       └── techniques.md     # 章节摘要 + 技法索引
└── myers-art-of-software-testing/
    ├── SKILL.md
    └── references/
        └── examples.md       # 示例 + 方法速查表
```

## ➕ 新增一个 Skill

1. 📝 创建 `<skill-name>/SKILL.md`，frontmatter 含 `name` + `description`。
   description 用第三人称写，带明确触发词（"MUST use when the user asks to …"）。
2. ✂️ `SKILL.md` 保持操作型且精简（工作流、输出格式、反模式、完成标准）；
   大块参考表格移到 `<skill-name>/references/*.md`。
3. ✅ 本地验证：`npx skills add . -l` → 安装 → 重启 agent → 触发测试。
4. 📣 把新 skill 加进两份 README 的目录表和 `package.json` 的 `files`。

## ❓ 常见问题

<details>
<summary><b><code>npx skills add . -l</code> 看不到 skill？</b></summary>

确认目录下有 `SKILL.md`，且含 `name`、`description` frontmatter。
</details>

<details>
<summary><b>装了但 agent 不加载？</b></summary>

重启会话；用 `-a claude-code|opencode|codex` 确认目标目录；
项目级 skill 必须在该项目内安装。
</details>

<details>
<summary><b>项目级 vs 全局？</b></summary>

项目级只影响当前仓库；`-g` 影响本机所有项目。
</details>

<details>
<summary><b>skill 正文是英文，中文对话能用吗？</b></summary>

可以。正文用英文是为了触发更稳定，每个 skill 都被要求用用户的语言回答。
</details>

## 📄 License

MIT。Skill 正文为方法蒸馏，供 agent 使用；请尊重原书版权，勿整书再分发。

---

<div align="center">

⭐ 如果这些 skill 对你有帮助，欢迎点个 star！

</div>
