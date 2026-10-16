# Codex 全局安装与默认生效

希望日常 Chat、进度、报告和文档都默认采用清楚表达规则，需要完成两步：安装 Skill；再设置用户级默认指令。设置只做一次，以后直接用自然语言提问。

## 1. 安装 Skill

从 [v0.4.3 Release](https://github.com/alexYG-arch/reader-first-output/releases/tag/v0.4.3) 下载 ZIP，解压后将整个 `reader-first-output/` 放入用户目录：

```text
~/.agents/skills/reader-first-output/
  SKILL.md
  agents/openai.yaml
  references/
    codex-global-instructions.md
    project-instructions.md
    rewriting.md
    interactive-html.md
    evaluation.md
```

`~` 表示当前用户的主目录，不是项目目录。macOS/Linux 通常位于用户主目录下，Windows 对应用户配置目录。本文的真实运行验证范围是 macOS arm64；Windows 路径和实际运行未单独验收。

也可让 Codex 的 `skill-installer` 从仓库的 `v0.4.3` 标签安装。仓库根目录就是 Skill 目录，目标名称为 `reader-first-output`。安装前查看是否已有同名目录，不在多个发现目录重复安装；已有版本先移到发现目录以外备份，再安装新版本，不丢弃用户自行修改的内容。

仅安装后，Codex 可以按描述选用它；简单问答可能不读取正文。这不是默认使用已设置成功的证据。`allow_implicit_invocation` 默认为 `true`，只代表允许隐式选用，不是每次调用的开关。[OpenAI 官方 Skill 说明](https://learn.chatgpt.com/docs/build-skills)

## 2. 合并用户级默认指令

打开 Codex 用户目录中的 `AGENTS.md`。默认是 `~/.codex/AGENTS.md`；如果设置了 `CODEX_HOME`，使用该目录下的文件。

将 [codex-global-instructions.md](../references/codex-global-instructions.md) 的完整标记块合并进去。保留已有内容；如果已存在 `<!-- reader-first-output:begin -->` 到 `<!-- reader-first-output:end -->`，更新这个块，不重复追加，也不要把整份原文件覆盖。首次修改前保留备份。

如果同目录已有非空 `AGENTS.override.md`，Codex 会优先读取它，普通 `AGENTS.md` 可能不会生效。先核对该文件的用途；如确需全局默认表达规则，将块合并到实际使用的用户指令中，保留既有要求。不要为本 Skill 新建 override 来压过已有设置。[OpenAI 官方 AGENTS 说明](https://learn.chatgpt.com/docs/agent-configuration/agents-md)

短稿中的表达规则会作为用户指令直接进入新聊天。普通问答直接遵循这些规则，不为应用表达偏好额外读取或启动 Skill，也不加加载说明。完整改写、HTML 或正式可读性审阅等专门任务仍可按任务隐式选用 Skill，并遵循宿主的必要告知要求。简单问题直接短答，不自动生成报告。它不会扩大业务权限，也不能忽略项目要求或用户指定的精确格式。

Skill 文件不会自行安装、修改用户配置或要求所有用户必须启用全局设置。全局默认由用户通过这一步选择；只想某个项目采用时，改用项目 `AGENTS.md` 和 [项目短稿](../references/project-instructions.md)。

## 3. 用自然问题验证

新建一个聊天，不输入 Skill 名。例如：

```text
这个产品把草稿保存在浏览器里，把导出文件保存在电脑的下载目录。
我只清理浏览器数据，不删除下载目录。哪一个会丢？一句话告诉我。
```

同时核对三件事：

1. Skill 目录或选择器中能找到 `reader-first-output`：这证明可发现。
2. 会话实际收到用户级默认规则；普通问题没有额外 Skill 调用或加载说明。可查看工具活动或本机会话中的实际指令记录，不需要公开整份日志。v0.4.3 普通 Chat 不要求读取 Skill 正文；这与旧版机制不同。
3. 检查整轮用户可见文字，包含进度消息；保留“浏览器草稿会丢、下载目录中的导出文件保留”，且只用一句话。不能只检查 final 而漏掉前面的说明，也不能仅凭回答简洁代替第 2 项。

再自然追问一轮，并另开新聊天检查规则持续可用。完整改写或 HTML 等专门任务可再检查实际 Skill 正文及相应参考文件读取；它们的正常使用告知不算普通 Chat 的多余提示。不要因为回答看起来简洁，就断定默认规则已注入。

Codex 会自动发现 Skill 更新；未出现时重启。用户指令通常在一次运行或会话启动时组成；修改后用新聊天或新 CLI 命令检查。已打开的 Desktop 聊天是否即时更新未在本项目中证明。[官方 Skill 说明](https://learn.chatgpt.com/docs/build-skills)与 [AGENTS 验证说明](https://learn.chatgpt.com/docs/agent-configuration/agents-md)

## 更新、停用与排查

- **更新**：保留旧 Skill 和原指令备份，更新 Skill 文件及同名标记块，再用新聊天验证。由 v0.4.2 更新时必须替换旧的“每个新聊天读取 Skill”要求，不能只换 Skill 文件；块外内容保持不变。升级不会自动替每个用户改写全局文件。
- **只保留按需调用**：删除用户指令中的本 Skill 标记块，保留 Skill 目录。日常默认表达规则不再由该块提供；专门任务仍可显式调用，或由 Codex 按任务选择。
- **完全停用**：移除默认规则块，并将 Skill 移到发现目录以外，或按 Codex 支持的 Skill 禁用设置处理。两处分别控制日常默认规则和按需方法。
- **找不到或没生效**：检查目录是否多套了一层、是否有同名副本、Skill 是否被禁用、自定义 `CODEX_HOME` 是否一致、override 是否替换了普通 AGENTS，以及项目指令是否有冲突。不要覆盖其他规则来解决。

已验证与未验证的边界见 [全局默认使用验证](GLOBAL-VALIDATION.md)。其他宿主的全局配置机制不同，不能直接承诺同样路径有效。

从旧版迁移及首次说明问题的证据边界见 [修复与复测](QUIET-DEFAULTS.md)。
