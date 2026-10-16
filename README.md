# reader-first-output · 清楚表达

让 AI 从一开始就把 Chat、进度说明、报告和文档写清楚，保留原意，避免无必要的自造词。当前版本：**0.4.2**。

日常使用直接完成原任务，不需要先提供难懂的文章；通俗改写与交互式 HTML 是按需使用的两种方式。

## 能做什么

| 使用方式 | 输出要求 |
| --- | --- |
| 日常 Chat、进度与报告 | 先回应问题，再补必要理由和条件；简单问题保持简短，不自动套报告或网页模板 |
| 完整通俗改写 | 调整措辞、顺序和解释，保留范围内全部独立信息，不自行变成摘要 |
| 交互式 HTML | 用连续可读的说明、适当图表和有用交互解释材料；支持定位与核对来源，不默认做网页幻灯片 |

共同规则：

- 优先使用已有名称、通行术语和普通语言；不为普通工作创造缩写或专用名词。
- 必要专业词准确保留，按读者需要解释。确实必须新命名时，标明暂称并给出通俗定义。
- 保留事实、原因、条件、否定、例外、数字、统计对象、时间、版本、状态、归属和不确定性。未知不能顺手补成已知。
- 不把局部测试当完整验收，不把历史记录当当前事实，不靠附原文掩盖解释中的遗漏。
- 不把条件或假设当成当前事实；建议同样不能预设未确认的原因。

## 安装与使用

### 推荐：Codex 全局安装，并默认使用

**只安装 Skill，表示它能被发现和自动选用；要让普通 Chat 也默认采用，还需合并一次用户级 `AGENTS.md` 规则。** 配好后直接提问，不必每次输入 `$reader-first-output`。

可把下面这段话一次性发给 Codex：

```text
请使用 skill-installer 从 https://github.com/alexYG-arch/reader-first-output/tree/v0.4.2
安装 reader-first-output 到 ~/.agents/skills/reader-first-output；如已有安装，先备份再更新。
再将 references/codex-global-instructions.md 合并到 Codex 用户级 AGENTS.md
（默认 ~/.codex/AGENTS.md；自定义 CODEX_HOME 时使用对应目录）。
保留已有内容，已有相同标记块时更新该块，不重复追加。
若存在 AGENTS.override.md，先检查实际生效文件，不覆盖或新建 override。
设置完成后，用不提 Skill 名的自然问题验证新聊天确实读取了 Skill 正文。
```

默认规则让每个新聊天首次回复前读取 Skill，同一聊天已有规则时不反复读取。只有改写、HTML、正式评测才读取相应参考文件。项目中的具体要求、当前任务的精确格式和权限边界继续适用。

也可在 [Releases](https://github.com/alexYG-arch/reader-first-output/releases/latest) 下载 `reader-first-output-v0.4.2.zip`，将完整目录放入 `~/.agents/skills/`，再按上述方式合并 [全局指令短稿](references/codex-global-instructions.md)。**ZIP 解压本身不会修改全局设置。**

新建聊天验证即可；如果 Skill 没出现或指令仍旧，重启 Codex 后再检查。已打开的旧聊天是否即时更新不作保证。完整的手动步骤、更新、停用及排查方法见 [Codex 安装与默认生效](docs/INSTALL-CODEX.md)。目录与加载规则依据 [OpenAI 官方 Skill 说明](https://learn.chatgpt.com/docs/build-skills)和 [AGENTS 说明](https://learn.chatgpt.com/docs/agent-configuration/agents-md)。

### 只在某个项目使用

将 Skill 放入项目目录：

```text
.agents/skills/reader-first-output/
  SKILL.md
  agents/openai.yaml
  references/
```

将 [项目指令短稿](references/project-instructions.md) 合并到该项目的 `AGENTS.md` 等受支持入口，保留已有约束。其他工具应采用各自实际支持的加载方式；本项目没有逐宿主验证兼容性。

### 按需显式调用

显式调用示例：

```text
$reader-first-output 解释这个接口为何失败，保留已知原因与未知部分。

$reader-first-output 把以下内容讲给未参与项目的同事看，完整保留原意。

$reader-first-output 将这份记录做成可连续阅读、可查找、可核对来源的交互式HTML。
```

这些示例用于按需选用，已完成全局默认设置的用户不必在日常问题前反复加上 Skill 名。加载一次 Skill 本身不代表永久覆盖所有项目或新会话。

## 文件说明

- [SKILL.md](SKILL.md)：共同规则与按需入口。
- [通俗改写](references/rewriting.md)：完整改写与原意核对。
- [交互式 HTML](references/interactive-html.md)：解释、呈现、交互与验证。
- [项目指令短稿](references/project-instructions.md)：日常使用的可合并规则。
- [Codex 全局指令短稿](references/codex-global-instructions.md)：新聊天默认使用的可复制规则。
- [Codex 安装说明](docs/INSTALL-CODEX.md)：全局安装、合并、验证、更新与停用。
- [评测说明](references/evaluation.md)：生成、独立检查与修复方法。
- [测试摘要](docs/VALIDATION.md)：已完成范围、修复记录与证据限制。
- [全局默认使用验证](docs/GLOBAL-VALIDATION.md)：安装与实际读取证据、两次失败及修复范围。

Skill 是独立文本指令，不含业务执行器、模型、媒体或运行时依赖；v0.4.2 Skill ZIP 有 7 个文本文件，比 v0.4.1 多一份全局指令短稿。克隆仓库还会得到 README、安装说明和测试摘要，不要把仓库文件数与 Skill ZIP 文件数混为一谈。

## 测试范围

2026-10-15 使用 gpt-6.1-sol 显式加载测试：13组真实材料分别用于 Chat、完整改写及 HTML，最终各13/13通过AI审阅；后两种分别核对同一组120项原始信息。13个HTML完成了所列浏览器交互检查。另有11个构造情境及16段错误表达检查。[详细口径与限制](docs/VALIDATION.md)。

2026-10-16 另行验证了用户级发现、默认规则注入、Skill 正文实际读取、新 Desktop 聊天及 CLI 续接；发现条件推断问题后做定点修复。历史结果与本次发布检查分别记录，见 [全局验证摘要](docs/GLOBAL-VALIDATION.md)。

没有真人理解测量或无 Skill 对照试验，也未验证所有宿主、所有模型或完全离线打开。自动读取不保证以后每条回答都正确，重要交付仍应对照来源检查。
