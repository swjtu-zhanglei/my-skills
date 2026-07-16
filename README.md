# my-skills

个人 Agent Skill 仓库：把书里的方法做成可安装、可复用的 agent 能力。

| Skill | 做什么 | 触发场景 |
|-------|--------|----------|
| **fowler-refactoring** | Fowler《重构》第 2 版：坏味道识别、重构技法、两顶帽子 | 重构、去坏味道、改善设计且不改行为 |
| **myers-art-of-software-testing** | Myers《软件测试的艺术》第 3 版：操作型用例设计 | 为函数/模块/API 设计测试用例 |

通过统一 CLI 安装（[skills.sh](https://skills.sh/) / [vercel-labs/skills](https://github.com/vercel-labs/skills)）：

```bash
npx skills add <来源>
```

支持 **OpenCode**、**Claude Code**、**Codex**，以及 Cursor / Copilot 等 50+ agent。

---

## 目录结构

```
my-skills/
├── README.md
├── LICENSE
├── package.json
├── fowler-refactoring/
│   └── SKILL.md
└── myers-art-of-software-testing/
    └── SKILL.md
```

每个 skill 是独立文件夹，内含带 YAML frontmatter 的 `SKILL.md`（`name` + `description`）。  
CLI 会扫描仓库里所有这类文件夹。

本仓库与 `.opencode/skill/` 使用**各自独立副本**（无 symlink）。改 skill 后两边需分别更新，或以本仓库为准再 `cp` / `npx skills add .`。

---

## 安装

### 前置

- 已安装 Node.js（建议 18+）
- 用 `npx` 即可，不必全局安装 `skills`

### 方式一：从本地路径安装（本仓库）

```bash
# 进入本仓库
cd /path/to/my-skills

# 查看有哪些 skill
npx skills add . -l

# 安装全部 → 当前项目（project-local）
npx skills add . --all -y

# 安装全部 → 用户全局（所有项目可用）
npx skills add . --all -g -y

# 只装某一个
npx skills add . --skill fowler-refactoring -y
npx skills add . --skill myers-art-of-software-testing -y
```

**指定 agent：**

```bash
# OpenCode
npx skills add . --all -a opencode -y

# Claude Code
npx skills add . --all -a claude-code -y

# Codex
npx skills add . --all -a codex -y

# 一次装到所有已识别的 agent
npx skills add . --all -a '*' -y
```

**package.json 快捷脚本**（在 `my-skills/` 下）：

```bash
npm run list              # 列出 skill
npm run install:local     # 当前项目
npm run install:global    # 全局 -g
npm run install:opencode  # 仅 OpenCode
npm run install:claude    # 仅 Claude Code
npm run install:codex     # 仅 Codex
```

### 方式二：从 GitHub 安装（推远程后）

把 `YOUR_USER` 换成你的 GitHub 用户名：

```bash
# 全部 skill，当前项目
npx skills add YOUR_USER/my-skills --all -y

# 全部 skill，全局
npx skills add YOUR_USER/my-skills --all -g -y

# 单个 skill
npx skills add YOUR_USER/my-skills --skill fowler-refactoring -y
npx skills add YOUR_USER/my-skills --skill myers-art-of-software-testing -y

# 指定 agent
npx skills add YOUR_USER/my-skills --all -a opencode -y
npx skills add YOUR_USER/my-skills --all -a claude-code -y
npx skills add YOUR_USER/my-skills --all -a codex -y

# 完整 URL
npx skills add https://github.com/YOUR_USER/my-skills --all -y
```

### 方式三：手动复制（不依赖 npx）

| Agent | 项目级目录 | 用户级目录 |
|-------|------------|------------|
| OpenCode | `.opencode/skill/<name>/` | `~/.config/opencode/skill/<name>/` |
| Claude Code | `.claude/skills/<name>/` | `~/.claude/skills/<name>/` |
| Codex | `.agents/skills/<name>/` | `~/.codex/skills/<name>/` |

```bash
# OpenCode 项目级（真实目录拷贝，不要用 symlink）
mkdir -p /your/project/.opencode/skill
cp -R fowler-refactoring myers-art-of-software-testing /your/project/.opencode/skill/

# Claude Code 用户级
mkdir -p ~/.claude/skills
cp -R fowler-refactoring myers-art-of-software-testing ~/.claude/skills/

# Codex 用户级
mkdir -p ~/.codex/skills
cp -R fowler-refactoring myers-art-of-software-testing ~/.codex/skills/
```

每个目录里必须有 `SKILL.md`。复制后**重启 agent 会话**以便发现新 skill。

### 更新与卸载

```bash
npx skills update -y
npx skills update fowler-refactoring myers-art-of-software-testing -g -y
npx skills list
npx skills remove fowler-refactoring
npx skills remove myers-art-of-software-testing
```

### 安装后文件落在哪

| Agent | 常见路径 |
|-------|----------|
| OpenCode 项目 | `.opencode/skill/<name>/` |
| OpenCode 全局 | `~/.config/opencode/skill/<name>/` |
| Claude Code | `~/.claude/skills/<name>/` 或 `.claude/skills/` |
| Codex | `~/.codex/skills/<name>/` |

`-g` = 全局；不加 = 当前项目。`-a <agent>` = 只装到指定 agent。

---

## 使用

安装成功后，在对应 agent 里用**自然语言触发**，或显式点名 skill。

### fowler-refactoring（重构）

**适用：** 坏味道识别、重构手法选择、两顶帽子（加功能 vs 重构）。

**示例提示词：**

```text
用 fowler-refactoring：这段函数有哪些坏味道？优先用哪些重构手法？

对下面类做 Extract Function / Move Method，保持行为不变。

按两顶帽子：先只重构，不要加功能。
```

### myers-art-of-software-testing（测试用例设计）

**适用：** 单函数/多模块用例设计；评审已有用例是否缺边界。

**示例提示词：**

```text
用 myers-art-of-software-testing 为下面函数设计测试用例：
def is_isosceles_triangle(a, b, c) -> bool: ...

按 Myers：黑盒 EP+BVA，输出用例表（ID/Inputs/Expected/Tech/Pri）。

这是两个模块 A→B，按多模块 PATH 出 seam 用例和失败传播用例。

评审我这组用例，缺什么补什么（表格）。
```

**期望输出：** 用例**表格**（非长文）；含无效类/边界；有依赖写 Stub；多模块写接缝表。

### 在各产品里怎么叫到 skill

| 产品 | 怎么用 |
|------|--------|
| **OpenCode** | 安装后可被发现；对话描述任务，或 `/myers-art-of-software-testing` / 点名 skill |
| **Claude Code** | 装到 skills 目录后自动可发现；说「用 myers 设计用例」 |
| **Codex** | 装到 Codex skills 后，在任务里写 skill 意图；或 `npx skills use .@myers-art-of-software-testing` |

不确定是否装上：

```bash
npx skills list
```

---

## 推到 GitHub（可选）

```bash
cd my-skills
git remote add origin git@github.com:YOUR_USER/my-skills.git
git branch -M main
git push -u origin main
git tag v1.0.0 && git push origin v1.0.0
```

之后：

```bash
npx skills add YOUR_USER/my-skills --all -g -y
```

---

## 开发与维护

1. 以 `my-skills/<skill>/SKILL.md` 为源。  
2. 父项目 `.opencode/skill/` 是**独立副本**：改完后执行  
   `cp -R fowler-refactoring myers-art-of-software-testing ../.opencode/skill/`  
   或在目标项目 `npx skills add /path/to/my-skills --all -y`。  
3. 改 frontmatter 的 `name`/`description` 会影响触发，改后重装。  
4. 发版：`commit` → `tag vX.Y.Z` → `push`。

---

## 常见问题

**Q: `npx skills add . -l` 看不到 skill？**  
A: 确认目录下有 `SKILL.md`，且含 `name`、`description` frontmatter。

**Q: 装了但 agent 不加载？**  
A: 重启会话；用 `-a opencode|claude-code|codex` 确认目录；项目级 skill 须在该项目下安装。

**Q: 项目级 vs 全局？**  
A: 项目级只影响当前仓库；`-g` 影响本机所有项目。

**Q: 能否只给 Claude、不给 Codex？**  
A: `npx skills add . --all -a claude-code -y`。

---

## License

MIT。Skill 正文为方法蒸馏，供 agent 使用；请尊重原书版权，勿整书再分发。
