<div align="center">

# 🛠️ my-skills

**English** | [简体中文](./README.zh-CN.md)

![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)
![Skills](https://img.shields.io/badge/skills-4-brightgreen.svg)
![Agents](https://img.shields.io/badge/agents-Claude%20Code%20%7C%20OpenCode%20%7C%20Codex%20%7C%20Hermes%20%7C%20OpenClaw%20%2B50-orange.svg)

📖 Classic software engineering books and field practice, distilled into **operational, installable agent skills** —
not summaries, but workflows your coding agent actually executes.

</div>

---

## 📚 Skills

| Skill | 📕 Source | ⚙️ What it does | 💬 Triggers on |
|-------|-----------|-----------------|----------------|
| 🔧 [**fowler-refactoring**](./skills/fowler-refactoring/SKILL.md) | Fowler, *Refactoring* 2nd ed. | Diagnoses code smells (24-smell map), picks catalog techniques, executes in verified baby steps under the Two-Hats rule | "refactor this", "code smells?", "clean up without changing behavior" |
| 🧪 [**myers-art-of-software-testing**](./skills/myers-art-of-software-testing/SKILL.md) | Myers, *The Art of Software Testing* 3rd ed. | Designs destructive test cases: black-box EP + BVA + cause-effect, then white-box branch fill-in; unit and multi-module seam recipes; fixed case tables | "design test cases", "write tests for", "review my test suite" |
| 🔍 [**git-diff-review**](./skills/git-diff-review/SKILL.md) | Field practice (agent handoff & code review) | Shows every change as real line-by-line `git diff` — stat summary, per-file hunks, risk-ordered review points, verification evidence; no full-file before/after dumps | "show me the diff", "what changed?", "review this branch/commit/PR" |
| 🎨 [**animated-diagram-style**](./skills/animated-diagram-style/SKILL.md) | Field practice (visual style) | Applies pastel panels, clear typography, and restrained vector motion to architecture, flow, class, sequence, and state diagrams; delivers only HTML + SVG | "use this drawing style", "beautify a class diagram", "animated sequence diagram" |

Each skill under `skills/` contains a `SKILL.md` (YAML frontmatter + operational
instructions); bulky tables move into an optional `references/` directory loaded
on demand — following the
[progressive-disclosure structure](https://github.com/anthropics/skills) used
by official skill repositories.

## 🚀 Quick Start

```bash
# interactive: choose skills and agents
npx skills add swjtu-zhanglei/my-skills

# all four skills, then choose agents
npx skills add swjtu-zhanglei/my-skills --skill '*'

# non-interactive global install for one agent
npx -y skills add swjtu-zhanglei/my-skills --skill '*' -g -a claude-code
```

> `--skill '*'` selects all skills. `--all` selects all skills **and all agents**;
> `-y` skips every prompt. Universal agents (`.agents/skills`) are always included —
> the agent picker only adds per-agent links.

Powered by the [skills CLI](https://github.com/vercel-labs/skills) ([skills.sh](https://skills.sh/)). ⚡

## 🤖 Supported Agents

| Agent | 📦 Install command | 📂 Skills directory (project / user) |
|-------|--------------------|--------------------------------------|
| **Claude Code** | `npx skills add swjtu-zhanglei/my-skills --skill '*' -a claude-code -y` | `.claude/skills/` · `~/.claude/skills/` |
| **OpenCode** | `npx skills add swjtu-zhanglei/my-skills --skill '*' -a opencode -y` | `.opencode/skill/` · `~/.config/opencode/skill/` |
| **Codex** | `npx skills add swjtu-zhanglei/my-skills --skill '*' -a codex -y` | `.agents/skills/` · `~/.codex/skills/` |
| **Hermes Agent** | `npx skills add swjtu-zhanglei/my-skills --skill '*' -a hermes-agent -y` | auto-detected — check with `npx skills list` |
| **OpenClaw** | manual copy (below) | `<workspace>/skills/` · `~/.openclaw/skills/` |
| **DSH** (DeepSeek Harness) | `npx -y skills add swjtu-zhanglei/my-skills --skill '*' -g -a codex` | `~/.agents/skills/` (shared root; `-a codex` target) |
| Cursor / Copilot / Gemini CLI / 50+ more | `npx skills add swjtu-zhanglei/my-skills --skill '*'` (then choose agents) | see `npx skills list` |

> 💡 After installing, **restart the agent session** so new skills are discovered.

<details>
<summary><b>🧰 Full install matrix (local path, GitHub, single skill, manual copy)</b></summary>

### From a local clone

```bash
cd /path/to/my-skills

npx skills add . -l                      # list available skills
npx skills add . --skill '*'             # all skills, then choose agents
npx skills add . --skill '*' -g          # all skills, global, choose agents
npx skills add . --skill fowler-refactoring -y            # one skill
npx skills add . --skill myers-art-of-software-testing -y
npx skills add . --skill animated-diagram-style -y
npx -y skills add . --skill '*' -a codex # all skills → Codex + universal
```

Or use the npm shortcuts:

```bash
npm run list              # list skills
npm run install:local     # current project
npm run install:global    # user global (-g)
npm run install:claude    # Claude Code only
npm run install:opencode  # OpenCode only
npm run install:codex     # Codex only
npm run install:dsh       # ~/.agents/skills/ (read by DSH, Codex, Cursor, ...)
```

### From GitHub

```bash
npx skills add swjtu-zhanglei/my-skills
npx skills add swjtu-zhanglei/my-skills --skill '*'
npx -y skills add swjtu-zhanglei/my-skills --skill '*' -g -a claude-code
npx skills add swjtu-zhanglei/my-skills --skill fowler-refactoring -y
npx skills add https://github.com/swjtu-zhanglei/my-skills --skill '*'
```

### Manual copy (no npx)

Each skill folder under `skills/` (including `references/` and `assets/`) must be copied whole:

```bash
# Claude Code (user-level)
mkdir -p ~/.claude/skills
cp -R skills/* ~/.claude/skills/

# OpenCode (project-level; real copies, not symlinks)
mkdir -p /your/project/.opencode/skill
cp -R skills/* /your/project/.opencode/skill/

# Codex (user-level)
mkdir -p ~/.codex/skills
cp -R skills/* ~/.codex/skills/

# OpenClaw (user-level)
mkdir -p ~/.openclaw/skills
cp -R skills/* ~/.openclaw/skills/
```

For Hermes Agent, copy the folders from `skills/` into its skills directory per
its documentation, keeping the same folder layout.

### Update / uninstall

```bash
npx skills list
npx skills check
npx skills update -y
npx skills remove fowler-refactoring
npx skills remove myers-art-of-software-testing
npx skills remove git-diff-review
npx skills remove animated-diagram-style
```

</details>

## 💬 Usage

Skills trigger on natural language — describe the task, or name the skill explicitly.
They answer **in your language** (English, 中文, …). 🌏

**🔧 fowler-refactoring**

```text
Refactor this function — what smells does it have and which techniques apply?

Do an Extract Class on this file, baby steps, behavior unchanged.

Should I refactor this module now or after the release?
```

**🧪 myers-art-of-software-testing**

```text
Design test cases for: def is_isosceles_triangle(a, b, c) -> bool

Black-box EP+BVA, output the case table (ID/Inputs/Expected/Tech/Pri).

Two modules A→B: give me seam cases and failure-propagation threads.

Review my existing test suite — what's missing?
```

**🔍 git-diff-review**

```text
I just changed the auth middleware — show me the diff before I review it.

Review this branch against main: findings at file:line, fixes as git diff blocks.
```

**🎨 animated-diagram-style**

```text
Use $animated-diagram-style to draw the approval flow in this pastel technical style.

Apply the same style to a class diagram and an RPC sequence diagram; keep each diagram’s notation.

Deliver HTML and animated SVG only, with no JSON or raster previews.
```

[HTML viewer template](./skills/animated-diagram-style/assets/diagram-template.html) ·
[Animated SVG template](./skills/animated-diagram-style/assets/diagram-template.svg)
(download either file and open in a browser).

> 📋 Refactoring and testing produce concise tables; review produces real diffs;
> the diagram skill delivers an HTML viewer and an animated SVG; verification files stay outside the repository.

## 📁 Repository Structure

```
my-skills/
├── README.md                 # English (this file)
├── README.zh-CN.md           # 简体中文
├── LICENSE
├── package.json
└── skills/
    ├── fowler-refactoring/
    │   ├── SKILL.md          # operational workflow (always loaded)
    │   └── references/
    │       ├── smell-map.md  # 24 smells → techniques (loaded on demand)
    │       └── techniques.md # chapter digests + technique index
    ├── myers-art-of-software-testing/
    │   ├── SKILL.md
    │   └── references/
    │       └── examples.md   # worked examples + method cheatsheet
    ├── git-diff-review/
    │   └── SKILL.md          # diff-based handoff & review protocol
    └── animated-diagram-style/
        ├── SKILL.md          # diagram style and two-file delivery
        ├── agents/openai.yaml # skill discovery metadata
        ├── assets/
        │   ├── diagram-template.html
        │   └── diagram-template.svg
        └── references/
            ├── style-guide.md # palette, hierarchy, logos, motion
            └── verification.md # playback, embedding, zoom checks
```

## ➕ Adding a New Skill

1. 📝 Create `skills/<skill-name>/SKILL.md` with `name` matching the folder,
   plus `description` and `license: MIT` frontmatter. Write the description in
   third person with explicit trigger phrases ("MUST use when the user asks to …").
2. ✂️ Keep `SKILL.md` operational and lean (workflow, output format, anti-patterns,
   done criteria); move bulky reference tables into `skills/<skill-name>/references/*.md`.
3. ✅ Test locally: `npx skills add . -l`, install, restart the agent, trigger it.
4. 📣 Add the skill to the table in both READMEs; `package.json` already ships
   everything under `skills/**`.

## ❓ FAQ

<details>
<summary><b><code>npx skills add . -l</code> doesn't show my skill</b></summary>

The skill must live at `skills/<skill-name>/SKILL.md`, with `name` and
`description` frontmatter, and `name` must match `<skill-name>`.
</details>

<details>
<summary><b><code>--all</code> vs <code>--skill '*'</code> vs <code>-y</code></b></summary>

`--all` installs all skills to all agents; `--skill '*'` installs all skills but
still lets you choose agents; `-y` skips every prompt and auto-selects detected
agents. For a targeted install, use `--skill '*'` plus `-a <agent>`.
</details>

<details>
<summary><b>Installed but the agent doesn't load it</b></summary>

Restart the session. Verify the target directory with `-a claude-code|opencode|codex`.
Project-level skills must be installed inside that project.
</details>

<details>
<summary><b>Project vs global?</b></summary>

Project-level affects only the current repo; `-g` affects every project on the machine.
</details>

<details>
<summary><b>Skills are in English — will they work in Chinese conversations?</b></summary>

Yes. Skill bodies are English for reliable triggering, but every skill is
instructed to respond in the user's language.
</details>

## 📄 License

MIT. Skill bodies are method distillations for agent use; please respect the
original books' copyright — no full-book redistribution.

---

<div align="center">

⭐ If these skills help you, consider giving the repo a star!

</div>
