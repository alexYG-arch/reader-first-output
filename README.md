# reader-first-output · 清楚表达

让 AI 从一开始就把 Chat、进度说明、报告和文档写清楚，保留原意，避免无必要的自造词。当前版本：**0.4.1**。

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

## 安装与使用

在 [Releases](https://github.com/alexYG-arch/reader-first-output/releases) 下载 `reader-first-output-v0.4.1.zip`。解压后保留完整的 `reader-first-output/` 目录，按所用工具支持的 Skill 机制安装。

Codex 项目内的目录示意：

```text
.agents/skills/reader-first-output/
  SKILL.md
  agents/openai.yaml
  references/
```

目录与发现范围以宿主版本为准，参见 [官方 Skill 说明](https://learn.chatgpt.com/docs/build-skills)。其他工具应采用各自实际支持的加载方式；本项目没有逐宿主验证兼容性。

显式调用示例：

```text
$reader-first-output 解释这个接口为何失败，保留已知原因与未知部分。

$reader-first-output 把以下内容讲给未参与项目的同事看，完整保留原意。

$reader-first-output 将这份记录做成可连续阅读、可查找、可核对来源的交互式HTML。
```

若希望项目日常回复默认采用这些规则，可参考 [项目指令短稿](references/project-instructions.md)，合并到宿主支持的项目指令入口，保留已有约束。加载一次 Skill 不代表它永久覆盖所有项目或新会话。

## 文件说明

- [SKILL.md](SKILL.md)：共同规则与按需入口。
- [通俗改写](references/rewriting.md)：完整改写与原意核对。
- [交互式 HTML](references/interactive-html.md)：解释、呈现、交互与验证。
- [项目指令短稿](references/project-instructions.md)：日常使用的可合并规则。
- [评测说明](references/evaluation.md)：生成、独立检查与修复方法。
- [测试摘要](docs/VALIDATION.md)：已完成范围、修复记录与证据限制。

Skill 是独立文本指令，不含业务执行器、模型、媒体或运行时依赖；安装包只有 6 个文本文件。仓库中的说明与测试摘要不属于这 6 个 Skill 文件。

## 测试范围

2026-10-15 使用 gpt-6.1-sol 显式加载测试：13组真实材料分别用于 Chat、完整改写及 HTML，最终各13/13通过AI审阅；后两种分别核对同一组120项原始信息。13个HTML完成了所列浏览器交互检查。另有11个构造情境及16段错误表达检查。[详细口径与限制](docs/VALIDATION.md)。

没有真人理解测量或无 Skill 对照试验，也未验证自动发现、所有宿主、所有模型或完全离线打开。测试通过不意味着以后不会遗漏，重要交付仍应对照来源检查。
