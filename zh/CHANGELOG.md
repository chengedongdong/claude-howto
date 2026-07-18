# 更新日志

## [v2.1.212] — 2026-07-18

### 同步至 Claude Code v2.1.212

将教程覆盖范围从 v2.1.206 基线（2026-07-11 同步）提升至 v2.1.212，并附带一次仓库内部准确性审查，发现了若干与版本增量无关的缺陷。

### 修复

- **移除失效的 `#` 记忆快捷方式** — `02-memory/README.md` 在两处（"Method 3" 和一个 "Example 4" 演练）将 `#` 前缀快速添加记忆的模式记载为可用功能，与该文件自身开头三行处的命令表直接矛盾 — 该表早已将 `#` 标记为**已停用**。两处内容均已移除/改写，改为指向 `/memory` 和会话式记忆请求。
- **Bedrock/Vertex/Foundry 上的 Auto 模式由选择启用改为选择退出（v2.1.207）** — auto 模式现在在这些提供商上（以及已登录的 Claude 应用网关会话）对 Sonnet 5、Opus 4.7/4.8 和 Fable 5 默认可用；自 v2.1.207 起，`CLAUDE_CODE_ENABLE_AUTO_MODE` 这个选择启用标志不再生效。已在 `09-advanced-features/README.md` 和 `10-cli/README.md` 中修复。
- **`auto` 权限模式被误标为 "Research Preview"** — 当前官方文档将 `auto` 呈现为 GA（在所有计划上可用，仅受模型和提供商资格限制），而非预览功能。已在 `09-advanced-features/README.md`、`CATALOG.md` 和 `README.md` 中更正，`README.md` 还补上了其权限模式摘要表中缺失的 `auto` 行。
- **更正 `/fork` / `/branch` 历史** — 仓库曾声称 "`/fork` 在 v2.1.77 重命名为 `/branch`（保留别名）"。当前文档显示这是两个各自独立的现行命令：`/fork` 生成一个继承对话的后台子代理，`/branch` 则让你就地切换到对话的一个副本中。它们仅在 v2.1.77 至 v2.1.161 期间是同一个带别名的命令。已在 `01-slash-commands/README.md` 和 `09-advanced-features/README.md` 中修复。
- **补全 `effort` frontmatter 枚举** — `04-subagents/README.md` 只列出了 4 个级别（`low`/`medium`/`high`/`max`），缺少 `xhigh`，而记录该枚举的其他所有文件均包含它。
- **核对 bundled skills 数量** — `CATALOG.md` 的摘要表写着 "9 bundled"，与其自身 10 行的明细表不符；已更正为 10（总计 16）。
- **完整重新计算 `INDEX.md` 的 Feature Coverage Matrix** — 表头声称 "16 files"，Skills 行对同一类别声称 "28"；两者都与六个已记录 skills 实际的 21 个文件不符。Plugins 行类似地各项相加为 36 却显示 40。每一行都经过手工重新清点；Skills 行现为 `5 | 9 | 7`（**21**），Plugins 行现为 `11 | 9 | 3 | 3 | 3 | 3 | 7`（**39**）— 现在每一行的 Total 都等于其各单元格之和。
- **更新滞留在 `2.1.160` 的页脚集群** — 五个文件（`08-checkpoints/checkpoint-examples.md`、`09-advanced-features/planning-mode-examples.md`，以及三个 `07-plugins/*/README.md` 示例包文件）仍带着 2026 年 6 月的页脚，且未发现与版本相关的内容漂移；已提升至 2.1.212。
- **完整重新同步 `QUICK_REFERENCE.md`** — 此前落后 52 个版本，停留在 `2.1.160`/6 月 2 日。权限模式区块更新为 `manual`（原为 `default`），Compatible Models 增加了 Sonnet 5，"New Features (May 2026)" 一节已重新命名并刷新。
- **重写 `02-memory/README.md` 的 Memory Hierarchy 一节** — 用经过验证的结构替换了一个虚构的 8 层严格优先级模型：CLAUDE.md 文件和规则是被拼接进上下文的（而不是按覆盖关系选择），且 `managed-settings.d/` 是 `settings.json` 的机制，不属于 CLAUDE.md。Memory Architecture 图也已更正 — 它此前将 claude.ai 的 24 小时综合周期与 Claude Code 的持续自动记忆混为一谈。

### 新增

- **Subagent 输出扫描（v2.1.210）** — Claude Code 会扫描子代理报告中的提示注入模式（伪造的 system 标签、捏造的对话轮次、提及权限绕过），并将其无害化。已在 `04-subagents/README.md` 的新小节中记录。
- **会话级生成上限（v2.1.212）** — WebSearch 调用（`CLAUDE_CODE_MAX_WEB_SEARCHES_PER_SESSION`）和子代理生成（`CLAUDE_CODE_MAX_SUBAGENTS_PER_SESSION`，由 `/clear` 重置）默认每会话 200 次的上限。已在 `09-advanced-features/README.md`、`04-subagents/README.md` 和 `10-cli/README.md` 中记录。
- **MCP 长时间运行工具自动转入后台（v2.1.212）** — 超过 2 分钟的 MCP 工具调用现在会自动转入后台，而不是阻塞会话；可通过 `CLAUDE_CODE_MCP_AUTO_BACKGROUND_MS` 配置。已在 `05-mcp/README.md` 中记录。
- **`claude auto-mode reset` 与 `/resume` 选择器（v2.1.212）** — 新的 CLI 子命令可恢复默认的 auto-mode 配置；不带参数的 `/resume` 现在会打开一个历史会话选择器（包括已移除的会话），并以后台会话方式恢复。已在 `09-advanced-features/README.md`、`01-slash-commands/README.md` 和 `10-cli/README.md` 中记录。
- **Screen reader 模式（v2.1.208）** — 通过 `--ax-screen-reader`、`CLAUDE_AX_SCREEN_READER=1` 或 `"axScreenReader": true` 选择启用纯文本渲染。已在 `09-advanced-features/README.md` 和 `10-cli/README.md` 中记录。
- **Task 工具 `mode` 参数弃用（v2.1.212）** — 已在 `04-subagents/README.md` 中注明：子代理现在默认继承父会话的权限模式；Task 工具按调用传入的 `mode` 参数会被忽略。

### 已知缺口（本次同步推迟未修复）

- `03-skills/.claude/skills/blog-draft/` 是被 gitignore 的本地测试草稿（`03-skills/.gitignore` 中的 `# Local skill testing`），不是 `03-skills/blog-draft/` 的已跟踪副本 — 已确认无需处理。

## [v2.1.160] — 2026-06-02

### 同步至 Claude Code v2.1.160

将教程覆盖范围提升至 Claude Code v2.1.160 发布版。中间的 v2.1.156 同步（Claude Opus 4.8，#129）已应用到文档但未单独记录变更日志；本条目从那里继续，覆盖 v2.1.157–v2.1.160 的增量。此区间没有破坏性变更 — 新增内容是少量新的 CLI/功能面，外加例行的页脚版本更新。第三方提供商上的 Auto 模式是**选择启用（opt-in）**，不是新的默认值。

### 新增

- **`claude plugin init <name>`（v2.1.157）** — 直接在 `.claude/skills` 中脚手架生成一个新插件；放在那里的插件现在无需 marketplace 即可自动加载。已在 `10-cli/README.md`、`07-plugins/README.md` 和 `CATALOG.md` 中记录。
- **Bedrock / Vertex / Foundry 上的 Auto 模式（v2.1.158）** — auto 模式现在在这三个第三方提供商上对 Opus 4.7/4.8 可用，通过 `CLAUDE_CODE_ENABLE_AUTO_MODE=1` 环境变量**选择启用（opt-in）**。已在 `09-advanced-features/README.md`、`10-cli/README.md` 和 `CATALOG.md` 中记录。
- **`EnterWorktree` 会话中切换（v2.1.157）** — `EnterWorktree` 工具现在可以在会话内于 Claude 管理的各 worktree 之间切换，并且已完成的 worktree 会保持未锁定状态，以便 `git worktree remove`/`prune` 进行清理。已在 `09-advanced-features/README.md` 中记录。

### 行为变更

- **`acceptEdits` 写入安全提示（v2.1.160）** — 即使在 `acceptEdits` 模式下，Claude Code 现在也会在写入 shell 启动文件（`.zshenv`、`.zlogin`、`.bash_login`、`~/.config/git/`）和会执行代码的构建配置（`.npmrc`、`.yarnrc*`、`bunfig.toml`、`.bazelrc`、`.pre-commit-config.yaml`、`.devcontainer/`）之前先行提示，否则这些写入可能导致意外的命令执行。已在 `09-advanced-features/README.md` 中记录。
- **动态工作流触发关键词 `workflow` → `ultracode`（v2.1.160）** — 单独的 "workflow" 一词不再触发动态工作流运行；触发关键词现在是 `ultracode`。已在 `09-advanced-features/README.md` 中注明。

### 移除

- **`CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE` 现为 no-op（v2.1.160）** — 该环境变量已被移除，不再有任何效果。`10-cli/README.md` 中环境变量表的措辞已从 "removed 2026-06-01" 更新为 "Removed (no-op as of v2.1.160)"。

### 文档

- 修复了 `README.md` 中三处内部不一致的版本字符串（徽章和 FAQ 正文停留在 `2.1.145` / `v2.1.150`），并规范化了一个过期的 Sources 链接。
- 将每个英文文档的元数据页脚提升至 **v2.1.160 / 2026 年 6 月 2 日**，保持同步一致。

## [v2.1.150] — 2026-05-25

### 同步至 Claude Code v2.1.150

将教程覆盖范围从 Claude Code v2.1.145 提升至 v2.1.150（2026 年 5 月 23 日发布）。自上次同步以来，Anthropic 发布了五个补丁（v2.1.146 至 v2.1.150）。头条变化是 **bundled `/simplify` skill 重命名为 `/code-review`**（v2.1.146）— 这是纯粹的重命名，**没有别名**，旧名称不再可用。由于本仓库也自带一个本地 code-review skill，该目录已重命名为 `code-review-specialist`，以避免遮蔽新的内置命令。其他亮点：`/usage` 现在按类别细分成本，后台会话可用 `Ctrl+T` 固定，markdown 渲染器支持 GFM 任务列表复选框，并新增了 `allowAllClaudeAiMcps` 托管设置。本次同步还补齐了四个仍停留在 v2.1.143 的模块 README（`04-subagents`、`05-mcp`、`07-plugins`、`09-advanced-features`）。

### 行为变更

- **`/simplify` 重命名为 `/code-review`（v2.1.146）**：bundled 审查 skill 现在通过 `/code-review` 调用，并接受可选的 effort 级别（例如 `/code-review high`）；传入 `--comment` 可将发现作为内联 GitHub PR 评论发布（v2.1.147）。旧的 `/simplify` 名称不再可用 — 没有别名。已在 `01-slash-commands/README.md`、`03-skills/README.md`、`CATALOG.md`、`QUICK_REFERENCE.md` 和 `claude_concepts_guide.md` 中更新。

### 变更

- **将仓库本地的 `code-review` skill 重命名为 `code-review-specialist`**，以避免与新的内置 `/code-review` 冲突。目录 `03-skills/code-review/` → `03-skills/code-review-specialist/`，并在 `README.md`、`QUICK_REFERENCE.md`、`INDEX.md`、`CATALOG.md`、`LEARNING-ROADMAP.md`、`claude_concepts_guide.md` 和 `03-skills/README.md` 中更新了所有安装命令、目录树和交叉引用。新增一条说明，解释该命名冲突以及如何避免遮蔽内置命令。

### 新增

- **`/usage` 按类别细分成本（v2.1.149）** — 成本视图现在按类别（skills、subagents、plugins 以及每个 MCP 服务器的成本）细分支出。已在 `CATALOG.md` 和 `claude_concepts_guide.md` 中记录。
- **固定后台会话 — `Ctrl+T`（v2.1.147）** — 在 `claude agents` 中固定一个会话可使其在空闲时保持存活、就地重启以应用 Claude Code 更新，并且只有在非固定会话之后才会因内存压力被回收。已在 `10-cli/README.md` 中记录。
- **GFM 任务列表复选框渲染（v2.1.149）** — markdown 渲染器现在将 `- [ ]` / `- [x]` 渲染为复选框。已在 `09-advanced-features/README.md` 中记录。
- **`allowAllClaudeAiMcps` 托管设置（v2.1.149）** — 允许在组织范围内加载 claude.ai 云端 MCP 连接器。已在 `05-mcp/README.md` 中记录。

### 移除

- **Stop/SubagentStop hook 输入字段 `background_tasks` 和 `session_crons`** — 已从 `06-hooks/README.md` 和 `resources.md` 中移除。这两个字段源自 v2.1.145 发布说明，但并未在官方 hooks 参考页中列举；为使文档与已发布的参考保持一致而移除。

### 文档

- 将四个模块 README 从 v2.1.143 补齐至 v2.1.150：`04-subagents/README.md`、`05-mcp/README.md`、`07-plugins/README.md`、`09-advanced-features/README.md`。
- 将每个英文文档的元数据页脚提升至 **v2.1.150 / 2026 年 5 月 25 日**，保持同步一致。

## [v2.1.145] — 2026-05-20

### 同步至 Claude Code v2.1.145

将教程覆盖范围从 Claude Code v2.1.143 提升至 v2.1.145（2026 年 5 月 19 日发布）。自上次同步以来，Anthropic 发布了两个补丁（v2.1.144 和 v2.1.145）。亮点：`/extra-usage` 重命名为 `/usage-credits`、`/model` 默认变为仅当前会话生效、三个新的 bundled skills（`/run`、`/verify`、`/run-skill-generator`）、Stop/SubagentStop hook 输入字段 `background_tasks` 和 `session_crons`、用于脚本化的 `claude agents --json`，以及一个关闭 Bash 裸环境变量自动批准漏洞的安全修复。本次同步还补齐了六个在 v2.1.143 同步中被遗漏、仍停留在 v2.1.138 的根级参考文档（`LEARNING-ROADMAP.md`、`QUICK_REFERENCE.md`、`INDEX.md`、`resources.md`、`claude_concepts_guide.md`、`STYLE_GUIDE.md`）。

### 新增

- `/usage-credits` slash 命令（v2.1.144）— 取代 `/extra-usage` 成为主名称；`/extra-usage` 仍可作为别名使用。已在 `01-slash-commands/README.md` 和 `CATALOG.md` 中记录。
- 三个新的 bundled skills（v2.1.145）— `/run`（启动项目应用以查看变更运行效果）、`/verify`（构建、运行并观察应用以确认修复生效）、`/run-skill-generator`（通过生成按项目定制的 skill，教会 `/run`/`/verify` 处理特定项目）。已在 `03-skills/README.md`、`CATALOG.md` 和 `QUICK_REFERENCE.md` 中记录。使规范的 bundled skill 数量达到 **9**。
- Stop/SubagentStop hook 输入字段 `background_tasks` 和 `session_crons`（v2.1.145）— hook 作者可以读取它们，以决定在后台工作或计划任务仍未完成时是否阻止停止。已在 `06-hooks/README.md` 中记录。
- `claude agents --json`（v2.1.145）— 将代理列表打印为机器可读的 JSON，便于脚本化（状态栏、会话选择器、tmux-resurrect）。已在 `10-cli/README.md` 中记录。
- 摘要表中缺失的五个 hook 事件行 — `Setup`、`UserPromptExpansion`、`PermissionDenied`、`PostToolBatch`（正文早已声称 "29 events"；`CATALOG.md`、`claude_concepts_guide.md` 和 `INDEX.md` 中的摘要表只列出了 25 个）。

### 行为变更

- **`/model` 默认仅当前会话生效（v2.1.144）**：选择模型现在只应用于当前会话；选择后按 `d` 可将该选择设为未来会话的新默认值。已在 `01-slash-commands/README.md` 中记录。
- **关闭 Bash 裸环境变量自动批准漏洞（v2.1.145 安全修复）**：形如 `FOO=bar somecommand` 的命令在只有 `FOO=bar` 位于允许列表时不再被自动批准。需通过覆盖完整命令的 `Bash(...)` 权限规则显式重新允许此类命令。已在 `06-hooks/README.md` 中记录。
- **`context: fork` 无限循环修复（v2.1.145）**：使用 `context: fork` 的 skill 此前在罕见情况下可能触发无限重复调用循环。已在 `03-skills/README.md` 中以注释形式记录。

### 文档

- 将六个根级参考文档从 v2.1.138 补齐至 v2.1.145：`LEARNING-ROADMAP.md`、`QUICK_REFERENCE.md`、`INDEX.md`、`resources.md`、`claude_concepts_guide.md`、`STYLE_GUIDE.md`。
- 修复了 bundled skills 列表不一致 — `CATALOG.md`、`QUICK_REFERENCE.md` 和 `03-skills/README.md` 此前列出了三个各不相同的 5 项列表；已统一为规范的 9 个（`/batch`、`/claude-api`、`/debug`、`/fewer-permission-prompts`、`/loop`、`/run`、`/run-skill-generator`、`/simplify`、`/verify`）。`QUICK_REFERENCE.md` 的单元格此前还错误地把 `/voice` 和 `/browse` 列为 bundled skills — 两者都不是 bundled。
- 将 `QUICK_REFERENCE.md` 和 `resources.md` 中的 "New Features (March 2026)" 重命名为 "New Features (May 2026)"，与仓库其余部分保持一致。
- 将 `README.md` 中的版本徽章从 `2.1.138` 提升至 `2.1.145`，并更新了正文中两处 "latest: v2.1.138" 的表述。
- 将 STYLE_GUIDE 的示例元数据页脚从 `2.1.97` 更新至 `2.1.145`，使贡献者复制到的是当前版本。

## [v2.1.143] — 2026-05-19

### 同步至 Claude Code v2.1.143

将教程覆盖范围从 Claude Code v2.1.138 提升至 v2.1.143（2026 年 5 月 15 日发布）。自上次同步以来，Anthropic 发布了五个补丁（v2.1.139–v2.1.143）。亮点：`/goal` 和 `/scroll-speed` slash 命令、带完整分发标志集的 `claude agents` Agent View（Research Preview）、Stop hook 安全上限、hook exec 形式（`args`）、PostToolUse 的 `continueOnBlock`、hook `terminalSequence` 输出、Fast Mode 默认改为 Opus 4.7、Bedrock/Vertex/Foundry 在 Windows 上默认使用 PowerShell，以及 `worktree.bgIsolation` 设置。

### 新增

- `/goal <statement>` slash 命令（v2.1.139）— 注册一个会话级完成条件，并带有显示已用时间、轮次数和 token 用量的实时悬浮面板。已在 `01-slash-commands/README.md` 中记录，并从 `10-cli/README.md` 交叉链接。
- `/scroll-speed <±N>` slash 命令（v2.1.139）— 调节 TUI 实时预览滚动速度；按机器持久化。已在 `01-slash-commands/README.md` 中记录。
- `claude agents` Agent View（Research Preview，v2.1.139），带分发标志 `--cwd`（v2.1.141）、`--add-dir`、`--settings`、`--mcp-config`、`--plugin-dir`、`--permission-mode`、`--model`、`--effort`、`--dangerously-skip-permissions`（v2.1.142）。已在 `10-cli/README.md` 中记录。
- `claude plugin details <name>`（v2.1.139）— 完整的插件清单，外加预计的每轮/每次调用 token 成本估算。LSP 服务器在 v2.1.142 加入详情面板。已在 `07-plugins/README.md` 中记录。
- `/plugin` 浏览面板中的 marketplace 上下文成本预估（v2.1.143）。已在 `07-plugins/README.md` 中记录。
- Hook **exec 形式**（`args: string[]`，v2.1.139）— 直接以 `execve()` 生成进程，不经过 shell 解析；与 shell 形式的 `command` 字段互斥。已在 `06-hooks/README.md` 中记录。
- PostToolUse 上的 hook `continueOnBlock: true` 字段（v2.1.139）— 将被阻止的工具结果以 `tool_result` 形式返回给 Claude，而不是中止本轮。已在 `06-hooks/README.md` 中记录。
- Hook `terminalSequence` JSON 输出字段（v2.1.141）— 发出原始 OSC 转义序列，用于桌面通知、窗口标题和响铃。已在 `06-hooks/README.md` 中记录。
- `worktree.bgIsolation: "none"` 设置（v2.1.143）— 后台会话直接编辑当前工作副本，而不是隔离的 worktree。已在 `09-advanced-features/README.md` 中记录。
- `CLAUDE_PROJECT_DIR` 现在会传入每个 MCP stdio 服务器的环境（v2.1.139），并且插件和项目 `.mcp.json` 的 `command`/`args`/`env` 字段支持 `${CLAUDE_PROJECT_DIR}` 替换。已在 `05-mcp/README.md` 中记录。
- Subagent OTEL 头 `x-claude-code-agent-id` 和 `x-claude-code-parent-agent-id`（v2.1.139），在 `claude_code.llm_request` OTEL span 上作为 `agent_id` / `parent_agent_id` 属性暴露。已在 `04-subagents/README.md` 中记录。
- `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE=1`（v2.1.142）— 在 v2.1.142 默认值切换为 Opus 4.7 之后，将 Fast Mode 钉回 Opus 4.6。已在 `10-cli/README.md` 中记录。
- `CLAUDE_CODE_USE_POWERSHELL_TOOL=0` 和 `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1`（v2.1.143）— 退出默认启用的 PowerShell 工具，或让它遵循系统执行策略而不是 `-ExecutionPolicy Bypass`。已在 `09-advanced-features/README.md` 中记录。
- `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP`（v2.1.143）— 覆盖 Stop hook 连续 8 次阻止的安全上限（设为 `0` 可禁用）。已在 `06-hooks/README.md` 和 `09-advanced-features/README.md` 中记录。
- `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1`（v2.1.141）— 强制插件安装通过 HTTPS 克隆 GitHub 插件源，供没有 SSH 密钥的 CI runner 使用。已在 `07-plugins/README.md` 中记录。
- `ANTHROPIC_WORKSPACE_ID`（v2.1.141）— 将联邦工作负载身份令牌限定到特定工作区。已在 `09-advanced-features/README.md` 中记录。
- 根级 `SKILL.md` 插件模式（v2.1.142）— 只有顶层 `SKILL.md`（没有 `skills/` 子目录）的插件会作为单个 skill 呈现。已在 `07-plugins/README.md` 中记录。
- `/schedule` 的插件市场推广名称 **Routines**（Anthropic 博客，2026-05-14）— 在 `09-advanced-features/README.md` 中以一行说明呈现；CLI 界面仍为 `/schedule`。

### 行为变更

- **Fast Mode 默认切换为 Opus 4.7（v2.1.142）**：`/fast` 现在默认运行 Opus 4.7（原为 Opus 4.6）。设置 `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE=1` 可选择退回。
- **Windows 上 Bedrock/Vertex/Foundry 默认启用 PowerShell 工具（v2.1.143）**：Claude Code 以 `-ExecutionPolicy Bypass` 调用 PowerShell。可通过 `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1`（遵循系统策略）或 `CLAUDE_CODE_USE_POWERSHELL_TOOL=0`（禁用该工具）选择退出。
- **设置 API-key 认证时自动禁用 Remote Control、`/schedule`、claude.ai MCP 连接器和通知偏好（v2.1.139）**：设置 `ANTHROPIC_API_KEY`、`ANTHROPIC_AUTH_TOKEN` 或 `apiKeyHelper` 会禁用全部四个桥接 claude.ai 的功能面，即使同时存在 claude.ai 登录。
- **Stop hook 阻止循环上限为连续 8 次（v2.1.143）**：连续 8 次后会话以警告结束，防止有缺陷的 Stop hook 让会话永远循环。可用 `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP` 覆盖。
- **`subagent_type` 匹配现在不区分大小写和分隔符（v2.1.140）**：`code-reviewer`、`Code Reviewer` 和 `code_reviewer` 都解析到同一个代理。已在 `04-subagents/README.md` 中记录。

### 变更

- 根参考文档（`README.md`、`CATALOG.md`）从 `28 hook events` 更新为 `29 hook events` — 与 `Setup` hook 在 v2.1.138 落地后的 `06-hooks/README.md` 和 `LEARNING-ROADMAP.md` 一致。

### 译者注意事项

- 教程翻译（`vi/`、`ja/`、`uk/`、`zh/`）跟随英文；请在模块 README 和上方的 CHANGELOG 中同步本轮增量。页脚必须反映 `Last Updated: May 19, 2026` 和 `Claude Code Version: 2.1.143`。

## [v2.1.138] — 2026-05-09

### 同步至 Claude Code v2.1.138

将教程覆盖范围从 Claude Code v2.1.131 提升至 v2.1.138（2026 年 5 月 9 日发布）。自上次同步以来，Anthropic 在 v2.1.132 与 v2.1.138 之间发布了七个补丁。

### 新增（英文文档）

- `worktree.baseRef` 设置（v2.1.133）— 控制 `claude --worktree` 是从 `origin/<default>`（`"fresh"`，默认）还是本地 `HEAD`（`"head"`）创建分支。**行为变更**：`"fresh"` 默认值回退了 v2.1.128 的行为，因此在 v2.1.128 之后依赖本地 `HEAD` 分支方式的用户必须重新选择启用。已在 `09-advanced-features/README.md` 中记录。
- `autoMode.hard_deny` 管理键（v2.1.136）— 一组分类器规则，无论推断出的用户意图如何都阻止某类操作。用于在 auto 模式下绝不允许运行的操作（例如 `rm -rf /`、强制推送到受保护分支）。与 `soft_deny` 不同，hard-deny 规则不可被分类器协商。已在 `09-advanced-features/README.md` 中记录。
- `parentSettingsBehavior` 管理键（v2.1.133+，管理层级）— 控制 SDK 的 `managedSettings` 如何与父进程设置合并。`"first-wins"` 保持现有优先级；`"merge"` 对值进行深度合并。已在 `09-advanced-features/README.md` 中记录。
- `Setup` hook 事件 — 初始环境设置（每会话一次）；可用于配置工具链或安装依赖。使记录的 hook 事件总数从 28 增至 29。已在 `06-hooks/README.md` 中记录。
- Hook 输入 JSON 中的 `effort.level` 字段（v2.1.133）— 向 hook 暴露当前 effort 级别（`low`/`medium`/`high`/`xhigh`/`max`）。已在 `06-hooks/README.md` 中记录。
- Bash 子进程中的 `CLAUDE_CODE_SESSION_ID` 环境变量（v2.1.132）— 与 hook 输入 JSON 中 `session_id` 字段一致的会话 UUID，用于将 bash 日志与 hook 遥测关联。已在 `06-hooks/README.md` 中记录。
- Bash 子进程中的 `CLAUDE_EFFORT` 环境变量（v2.1.133）— 当前 effort 级别，与 hook 输入 JSON 中的 `effort.level` 一致。已在 `06-hooks/README.md` 中记录。
- `sandbox.bwrapPath` 和 `sandbox.socatPath` 设置（v2.1.133+，Linux/WSL）— 将 Claude Code 指向 `bubblewrap` 和 `socat` 的非标准安装位置。默认使用 `$PATH` 查找。已在 `09-advanced-features/README.md` 中记录。
- `CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN` 环境变量（v2.1.132）。已在 `09-advanced-features/README.md` 中记录。
- `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL` 环境变量（v2.1.136）— 为采集 OpenTelemetry 数据的组织重新启用会话质量调查；在 OTEL 部署中默认关闭。已在 `09-advanced-features/README.md` 中记录。

### 变更

- **行为变更**：Plan 模式现在无条件阻止所有文件写入（v2.1.136），包括 `permissions.allow` 中存在匹配的 `Edit(...)` 规则的情况。此前宽松的 `Edit(...)` 规则可能让写入在 plan 模式下通过；该绕过已被关闭。依赖旧行为的工作流必须先退出 plan 模式（`Shift+Tab`）再编辑。已在 `09-advanced-features/README.md` 中记录。
- 插件的空格分隔 slash 命令（例如 `/myplugin review`）现在解析为 `/myplugin:review`。插件的 `skills` 配置项不再隐藏默认的 `skills/` 目录 — 两者会被合并。已在 `07-plugins/README.md` 中记录。
- MCP 服务器现在跨 `/clear` 持久保留（v2.1.132+）。已在 `05-mcp/README.md` 中记录。
- 子代理通过 Skill 工具发现项目、用户和插件 skills（v2.1.133）。已在 `04-subagents/README.md` 中记录。
- 恢复 plan 模式会话时现在会遵循 `--permission-mode`（v2.1.132）。已在 `09-advanced-features/README.md` 中记录。
- `CronList` 输出现在包含限定符和计划的提示正文（v2.1.136），无需打开即可审计每个 cron 将运行的内容。已在 `09-advanced-features/README.md` 中记录。

### 修复

- OAuth 刷新令牌并发刷新竞态条件。
- INDEX.md 计数漂移：Skills 28 → 16、Plugins 40 → 27、Hooks 脚本 8 → 9（根据 markdown 内容树重新清点）。新的总数反映了仅统计 `.md` 的方法论，将计数范围限定为教程内容而非构建产物和配置。
- `CATALOG.md`（v2.1.118 → v2.1.138）和 `claude_concepts_guide.md`（v2.1.117 → v2.1.138）中过期的来源 URL。移除了概念指南中一个重复的旧页脚。

### 翻译维护者注意事项

`vi/`、`zh/`、`uk/` 和 `ja/` 本地化目录树由社区维护，可能落后于英文源。同步翻译的贡献者应与本次发布中更新的英文文件进行差异比对。

## [v2.1.131] — 2026-05-06

### 同步至 Claude Code v2.1.131

将教程覆盖范围从 Claude Code v2.1.126 提升至 v2.1.131（2026 年 5 月 6 日发布）。自上次同步以来，Anthropic 发布了 v2.1.128、v2.1.129 和 v2.1.131；v2.1.127 和 v2.1.130 被跳过，从未公开发布。

### 新增（英文文档）

- `--plugin-url <url>` 标志（v2.1.129）— 从 URL 获取插件 `.zip` 归档用于当前会话。可重复使用。已在 `07-plugins/README.md` 中记录。
- `CLAUDE_CODE_FORCE_SYNC_OUTPUT` 环境变量（v2.1.129）— 为自动检测失效的终端（例如 Emacs `eat`）强制使用同步输出。已在 `10-cli/README.md` 和 `09-advanced-features/README.md` 中记录。
- `CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE` 环境变量（v2.1.129）— 为 Homebrew/WinGet 安装方式（通常不自动更新）启用后台升级。已在 `10-cli/README.md` 和 `09-advanced-features/README.md` 中记录。
- `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY` 环境变量（v2.1.129）— 选择启用 `/v1/models` 网关发现时必需（见"变更"）。已在 `10-cli/README.md` 中记录。
- `disableRemoteControl` 设置（v2.1.128）— 管理员可以通过 managed/policy 作用域禁止 `claude remote-control` 和 `/remote-control`。已在 `09-advanced-features/README.md` 中记录。
- `--plugin-dir` 接受 `.zip` 归档（v2.1.128）— 与目录输入并存。已在 `07-plugins/README.md` 中记录。
- `skillOverrides` 接受 `"name-only"` 和 `"user-invocable-only"`（v2.1.129）— 在此前的 `"on"`/`"off"` 之外新增。已在 `03-skills/README.md` 中记录。

### 变更

- **行为变更**：网关 `/v1/models` 发现现在是**选择启用（opt-in）**（v2.1.129）。此前（v2.1.126），设置 `ANTHROPIC_BASE_URL` 会自动从网关的 `/v1/models` 端点填充 `/model`。从 v2.1.129 起，用户必须另外设置 `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1`；没有该环境变量时，`/model` 回退到内置静态列表。已在 `10-cli/README.md` 中记录。
- `/mcp` 显示每个服务器的工具数量，并以可视方式标记报告 0 个工具的服务器（v2.1.128）。已在 `05-mcp/README.md` 中记录。
- 不带参数的 `/color` 会随机选择一个会话颜色（v2.1.128）；显式的 `/color <name|hex>` 仍设置指定颜色。已在 `01-slash-commands/README.md` 中记录。
- `--channels` 标志现在可与 API-key（console）认证一起使用（v2.1.128）。更早的版本需要 Pro/Max OAuth。已在 `09-advanced-features/README.md` 中记录。
- Ctrl+R 历史选择器默认显示**所有项目的所有提示**（v2.1.129）。在选择器内按 Ctrl+S 可将范围缩小到当前项目。已在 `09-advanced-features/README.md` 中记录。
- `/context` 不再将其 ASCII 可视化输出进对话（v2.1.129）。该可视化仅在 UI 中显示；每次调用不再产生约 1.6k token 的成本。已在 `09-advanced-features/README.md` 中记录。
- 拖放中的超大图像会自动缩小（v2.1.128）— 更早的版本会直接拒绝这些图像。

### 修复

- Windows 上的 VS Code 扩展激活（v2.1.131）。
- Mantle 端点认证（v2.1.131）。
- 1 小时提示缓存 TTL 不再被截断为 5 分钟（v2.1.129）。
- 大于 10 MB 的 stdin 载荷导致的崩溃（v2.1.128）。

### 翻译维护者注意事项

`vi/`、`zh/`、`uk/` 和 `ja/` 本地化目录树由社区维护，可能落后于英文源。同步翻译的贡献者应与本次发布中更新的英文文件进行差异比对。

## [v2.1.126] — 2026-05-02

### 同步至 Claude Code v2.1.126

将教程覆盖范围从 Claude Code v2.1.119 提升至 v2.1.126（2026 年 5 月 1 日发布）。v2.1.120 在其首个发布日（2026-04-24）被回滚，但于 2026-04-28 成功重新发布，最初报告的回归问题已修复。v2.1.124 和 v2.1.125 被 Anthropic 跳过，从未发布。

### 新增（英文文档）

- `claude project purge [path]` 子命令（v2.1.126）— 删除某个项目的全部 Claude Code 状态（转录、任务、调试日志、文件编辑历史、提示历史、`~/.claude.json` 条目）。支持 `--dry-run`、`-y/--yes`、`-i/--interactive`、`--all`。已在 `10-cli/README.md` 中记录。
- `claude plugin prune` 子命令（v2.1.121）— 移除孤立的自动安装插件依赖；`plugin uninstall --prune` 会级联处理。已在 `07-plugins/README.md` 中记录。
- `claude ultrareview [target]` 子命令（v2.1.120）— 从 CI/脚本非交互式地运行 `/ultrareview`，将发现打印到 stdout，成功/失败时以 0/1 退出；支持 `--json` 和 `--timeout <minutes>`。已在 `10-cli/README.md` 中记录。
- skill 内容中可用的 `${CLAUDE_EFFORT}` 占位符（v2.1.120）— 解析为当前 effort 级别。已在 `03-skills/README.md` 中记录。
- `alwaysLoad` MCP 服务器配置选项（v2.1.121）— 为 `true` 时，该服务器的所有工具跳过 tool-search 延迟加载。已在 `05-mcp/README.md` 中记录。
- `PostToolUse.hookSpecificOutput.updatedToolOutput` 现在对所有工具生效（v2.1.121），此前仅限 MCP。已在 `06-hooks/README.md` 中记录。
- `ANTHROPIC_BEDROCK_SERVICE_TIER` 环境变量（v2.1.122）— 选择 Bedrock 服务层级（`default`、`flex`、`priority`）。已在 `10-cli/README.md` 环境变量表中记录。
- `--dangerously-skip-permissions` 扩展路径覆盖（v2.1.121、v2.1.126）— 现在会跳过对 `.claude/skills/`、`.claude/agents/`、`.claude/commands/`、`.claude/`、`.git/`、`.vscode/` 及 shell 配置文件写入的提示。灾难性删除命令（`rm -rf /` 等）仍会提示。已在 `09-advanced-features/README.md` 权限模式一节中记录。
- OAuth 代码粘贴回退（v2.1.126）— 当浏览器回调无法访问 localhost（WSL2、SSH、容器）时，`claude auth login` 接受把 OAuth 代码粘贴到终端。已在 `10-cli/README.md` 中记录。
- 键入即筛选的 `/skills` 菜单（v2.1.121）。已在 `03-skills/README.md` 中记录。
- `AI_AGENT` 环境变量（v2.1.120）— 在子进程上设置，使 `gh` 能将流量归因于 Claude Code。已在 `10-cli/README.md` 环境变量表中记录。

### 变更

- `--from-pr`（v2.1.119）和 `/resume` 的 PR-URL 搜索（v2.1.122）现在都支持 GitHub、GitHub Enterprise、GitLab 和 Bitbucket URL。
- Windows：不再需要 Git for Windows / Git Bash（v2.1.120）— 当 Git Bash 缺失时，Claude Code 使用 PowerShell 作为 shell 工具。从 v2.1.126 起，启用 PowerShell 工具时 PowerShell 是首选 shell。检测范围扩展到通过 Microsoft Store 安装的 PowerShell 7、未加入 PATH 的 MSI 安装或 `.NET global tool`。已在 `09-advanced-features/README.md` 平台说明中记录。
- 当 `ANTHROPIC_BASE_URL` 指向兼容 Anthropic 的网关时，`/model` 选择器现在会列出该网关 `/v1/models` 端点中的模型（v2.1.126）。已在 `10-cli/README.md` 中记录。
- `--dangerously-skip-permissions` 不再对一个大幅扩展的允许列表中的写入进行提示（见"新增"）。灾难性删除仍会提示。
- 图像粘贴自动缩小（v2.1.126）— 大于 2000px 的图像在粘贴时会被缩小；历史记录中的超大图像会被自动移除并重试请求。（仅作为安全/UX 说明与教程相关。）

### 安全

- 修复了当更高优先级的 managed-settings 源缺少 `sandbox` 块时，`allowManagedDomainsOnly` / `allowManagedReadPathsOnly` 被忽略的问题（v2.1.126）。

### 翻译维护者注意事项

`vi/`、`zh/`、`uk/` 和 `ja/` 本地化目录树由社区维护，可能落后于英文源。同步翻译的贡献者应与本次发布中更新的英文文件进行差异比对。

## [v2.4.0] — 2026-04-27

### 同步至 Claude Code v2.1.119

将教程覆盖范围从 Claude Code v2.1.112 提升至 v2.1.119（2026 年 4 月 23 日发布）。v2.1.120 于 4 月 24 日发布，因回归问题当天被短暂回滚，并于 4 月 28 日携修复重新发布 — 现已是正常发布线的一部分。随后的 v2.1.126（2026 年 5 月 1 日）是下一个稳定目标，在上方的 v2.1.126 条目中覆盖。

### 新增（英文文档）

- 原生二进制打包说明（v2.1.113）— CLI 现在按平台分发原生二进制文件
- 原生 macOS/Linux 构建上的 `bfs`/`ugrep` Glob/Grep 替换脚注（v2.1.117）
- 带示例的 `mcp_tool` hook 类型（v2.1.118）
- PostToolUse / PostToolUseFailure 输入上的 `duration_ms` 字段（v2.1.119）
- `prUrlTemplate` 设置（v2.1.119）以及扩展的 `--from-pr` 提供商列表（GitLab、Bitbucket）
- `cleanupPeriodDays` 扩展作用范围（检查点 + 任务 + shell 快照 + 备份，v2.1.117）
- 每个生命周期事件上的插件 marketplace 强制校验（v2.1.117）以及 `hostPattern`/`pathPattern` 正则（v2.1.119）
- 新环境变量：`DISABLE_UPDATES`、`CLAUDE_CODE_HIDE_CWD`、`CLAUDE_CODE_FORK_SUBAGENT`、`OTEL_LOG_TOOL_DETAILS`、`ENABLE_TOOL_SEARCH` Vertex 选择启用
- 新 slash 命令：`/btw`、支持自定义主题的 `/theme`
- `/usage` 规范命令（合并 `/cost` + `/stats`，v2.1.118）
- Fork 式子代理（`CLAUDE_CODE_FORK_SUBAGENT=1`，v2.1.117）
- Auto 模式 `"$defaults"` 令牌（v2.1.118）
- `wslInheritsWindowsSettings` 托管策略（v2.1.118）
- Vim visual / visual-line 模式（v2.1.118）
- `claude install [version]` 和 `claude plugin tag` 子命令

### 变更

- 文档主机迁移：`docs.anthropic.com/en/docs/claude-code/*` → `code.claude.com/docs/en/*`
- Opus 4.7 effort 级别：自 2026-04-16 发布起 `xhigh` 现为 Claude Code 默认值；Opus 4.7 原生上下文窗口确认为 1M（v2.1.117 修复了 `/context` 将其误计为 200K 的问题）
- Opus 4.6 / Sonnet 4.6 上 Pro/Max 订阅者的默认 effort 从 `medium` 提升至 `high`（v2.1.117）
- `STYLE_GUIDE.md` 的 Source URL 从 Claude Apps 文章更新为 `code.claude.com/docs/en/changelog`

### 已弃用（跟踪中，未移除）

- `includeCoAuthoredBy` 设置 → 使用 `attribution.commit` / `attribution.pr`
- `voiceEnabled` 设置 → 使用 `voice.enabled`

### 翻译维护者注意事项

`vi/`、`zh/` 和 `uk/` 本地化目录树由社区维护，可能落后于英文源。同步翻译的贡献者应与本次发布中更新的英文文件进行差异比对。

## v2.1.112 — 2026-04-16

### 亮点

- 将所有英文教程与 Claude Code v2.1.112 和新的 Opus 4.7 模型（`claude-opus-4-7`）同步，包括新的 `xhigh` effort 级别（Opus 4.7 的默认值，介于 `high` 和 `max` 之间）、两个新的内置 slash 命令（`/ultrareview`、`/less-permission-prompts`）、Max 订阅者在 Opus 4.7 上使用 auto 模式不再需要 `--enable-auto-mode`、Windows 上的 PowerShell 工具、"Auto (match terminal)" 主题，以及按提示命名的计划文件。全部 18 个英文文档页脚提升至 Claude Code v2.1.112。@Luong NGUYEN

### 功能

- 添加覆盖所有模块、根文档、示例和参考的完整乌克兰语（uk）本地化（039dde2）@Evgenij I

### Bug 修复

- 修正 pre-tool-check.sh hook 协议缺陷（bce7cf8）@yarlinghe
- 将有问题的 mermaid 示例改为 text 块以通过 CI（b8a7b1f）@Evgenij I
- 修复乌克兰语 claude_concepts_guide.md 目录中的 CP1251 编码（d970cc6）@Evgenij I
- 用完整翻译替换乌克兰语 README 占位稿，修复失效锚点（f6d73e2）@Evgenij I
- 将所有页脚的 Claude Code 版本更正为 2.1.97（63a1416）@Luong NGUYEN
- 应用 2026-04-09 文档准确性更新（e015f39）@Luong NGUYEN

### 文档

- 同步至 Claude Code v2.1.112（Opus 4.7、`xhigh` effort、`/ultrareview`、`/less-permission-prompts`、PowerShell 工具、Auto-match-terminal 主题）@Luong NGUYEN
- 同步至 Claude Code v2.1.110（TUI、推送通知、会话回顾）（15f0085）@Luong NGUYEN
- 同步至 Claude Code v2.1.101，含 `/team-onboarding`、`/ultraplan`、Monitor 工具（2deba3a）@Luong NGUYEN
- 将越南语文档与英文源同步（561c6cb）@Thiên Toán
- 更新所有文件的 Last Updated 日期和 Claude Code 版本（7f2e773）@Luong NGUYEN
- 在语言切换器中添加乌克兰语链接（9c224ff）@Luong NGUYEN
- 移除贡献者章节（f07313d）@Luong NGUYEN
- 更新 GitHub 指标至 21,800+ stars、2,585+ forks（4f55374）@Luong NGUYEN

**完整更新日志**：https://github.com/luongnv89/claude-howto/compare/v2.3.0...v2.1.112

---

## v2.3.0 — 2026-04-07

### 功能

- 构建和发布每种语言的 EPUB 制品（90e9c30）@Thiên Toán
- 将缺失的 pre-tool-check.sh hook 添加到 06-hooks（b511ed1）@JiayuWang
- 在 zh/ 目录中添加中文翻译（89e89d4）@Luong NGUYEN
- 添加性能优化器子代理和依赖检查 hook（f53d080）@qk

### Bug 修复

- Windows Git Bash 兼容性 + stdin JSON 协议（2cbb10c）@Luong NGUYEN
- 修正 08-checkpoints 中的 autoCheckpoint 配置文档（749c79f）@JiayuWang
- 用占位符替代 SVG 图像嵌入（1b16709）@Thiên Toán
- 修复内存 README 中的嵌套代码围栏渲染（ce24423）@Zhaoshan Duan
- 应用由 squash merge 丢弃的审查修复（34259ca）@Luong NGUYEN
- 使 hook 脚本与 Windows Git Bash 兼容并使用 stdin JSON 协议（107153d）@binyu li

### 文档

- 与 Claude Code 最新文档（2026 年 4 月）同步所有教程（72d3b01）@Luong NGUYEN
- 在语言切换器中添加中文语言链接（6cbaa4d）@Luong NGUYEN
- 在英文和越南文之间添加语言切换器（100c45e）@Luong NGUYEN
- 添加 GitHub #1 Trending 徽章（0ca8c37）@Luong NGUYEN
- 为上下文区域监控引入 cc-context-stats（d41b335）@Luong NGUYEN
- 引入 luongnv89/skills 集合和 luongnv89/asm skill 管理器（7e3c0b6）@Luong NGUYEN
- 更新 README 统计数据以反映当前 GitHub 指标（5900+ stars, 690+ forks）（5001525）@Luong NGUYEN
- 更新 README 统计数据以反映当前 GitHub 指标（3900+ stars, 460+ forks）（9cb92d6）@Luong NGUYEN

### 重构

- 使用本地 mmdc 渲染替代 Kroki HTTP 依赖（e76bbe4）@Luong NGUYEN
- 将质量检查转移到 pre-commit，CI 作为第二道防线（6d1e0ae）@Luong NGUYEN
- 缩小自动模式权限基线（2790fb2）@Luong NGUYEN
- 用一次性权限设置脚本替换自动适配 hook（995a5d6）@Luong NGUYEN

### 其他

- 左移质量关卡 — 向 pre-commit 添加 mypy，修复 CI 失败（699fb39）@Luong NGUYEN
- 添加越南语（Tiếng Việt）本地化（a70777e）@Thiên Toán

**完整更新日志**：https://github.com/luongnv89/claude-howto/compare/v2.2.0...v2.3.0

---

## v2.2.0 — 2026-03-26

### 文档

- 与 Claude Code v2.1.84（f78c094）同步更新所有教程和参考文档 @luongnv89
  - 将 slash commands 更新为 55+ 个内置命令 + 5 个 bundled skills，并标记 3 个已弃用项
  - 将 hook 事件从 18 个扩展到 25 个，新增 `agent` hook 类型（现在共 4 类）
  - 在高级功能中加入自动模式（Auto Mode）、通道（Channels）、语音输入（Voice Dictation）
  - 为 skill frontmatter 增加 `effort`、`shell` 字段；为 agent 增加 `initialPrompt`、`disallowedTools` 字段
  - 增加 WebSocket MCP transport、elicitation、2KB 工具上限
  - 增加 plugin 的 LSP 支持、`userConfig`、`${CLAUDE_PLUGIN_DATA}`
  - 更新所有参考文档（CATALOG、QUICK_REFERENCE、LEARNING-ROADMAP、INDEX）
- 将 README 重写为落地页结构化指南（32a0776）@luongnv89

### Bug 修复

- 为通过 CI 词典检查，补充缺失的 cSpell 词条和 README 章节（93f9d51）@luongnv89
- 在 cSpell 词典中加入 `Sandboxing`（b80ce6f）@luongnv89

**完整更新日志**：https://github.com/luongnv89/claude-howto/compare/v2.1.1...v2.2.0

---

## v2.1.1 — 2026-03-13

### Bug 修复

- 移除导致 CI 链接检查失败的失效 marketplace 链接（3fdf0d6）@luongnv89
- 将 `sandboxed` 和 `pycache` 加入 cSpell 词典（dc64618）@luongnv89

**完整更新日志**：https://github.com/luongnv89/claude-howto/compare/v2.1.0...v2.1.1

---

## v2.1.0 — 2026-03-13

### 功能

- 新增自适应学习路径，包含自我评估和 lesson quiz skills（1ef46cd）@luongnv89
  - `/self-assessment` - 覆盖 10 个功能领域的交互式能力测验，并生成个性化学习路径
  - `/lesson-quiz [lesson]` - 每课知识检查，包含 8-10 道针对性问题

### Bug 修复

- 更新失效 URL、弃用项和过时引用（8fe4520）@luongnv89
- 修复 resources 和 self-assessment skill 中的损坏链接（7a05863）@luongnv89
- 在 concepts guide 中为嵌套代码块使用 tilde fences（5f82719）@VikalpP
- 为 cSpell 词典补充缺失词汇（8df7572）@luongnv89

### 文档

- Phase 5 QA - 修复各文档中的一致性、URL 和术语问题（00bbe4c）@luongnv89
- 完成 Phase 3-4 - 补充新功能覆盖和参考文档更新（132de29）@luongnv89
- 在 MCP 上下文膨胀章节加入 MCPorter runtime（ef52705）@luongnv89
- 为 6 份指南补充缺失命令、功能和设置（4bc8f15）@luongnv89
- 基于仓库现有规范补充 style guide（84141d0）@luongnv89
- 在指南对比表中加入自我评估行（8fe0c96）@luongnv89
- 将 VikalpP 加入贡献者名单，记录 PR #7（d5b4350）@luongnv89
- 在 README 和 roadmap 中加入 self-assessment 与 lesson-quiz skill 参考（d5a6106）@luongnv89

### 新贡献者

- @VikalpP 完成了他们的首次贡献，见 #7

**完整更新日志**：https://github.com/luongnv89/claude-howto/compare/v2.0.0...v2.1.0

---

## v2.0.0 — 2026-02-01

### 功能

- 与 Claude Code 2026 年 2 月功能同步更新全部文档（487c96d）
  - 更新所有 10 个教程目录和 7 份参考文档中的 26 个文件
  - 补充 **Auto Memory** 文档 - 每个项目的持久学习能力
  - 补充 **Remote Control**、**Web Sessions** 和 **Desktop App** 文档
  - 补充 **Agent Teams** 文档（实验性多 agent 协作）
  - 补充 **MCP OAuth 2.0**、**Tool Search** 和 **Claude.ai Connectors** 文档
  - 补充 subagents 的 **Persistent Memory** 和 **Worktree Isolation** 文档
  - 补充 **Background Subagents**、**Task List**、**Prompt Suggestions** 文档
  - 补充 **Sandboxing** 和 **Managed Settings**（Enterprise）文档
  - 补充 **HTTP Hooks** 和 7 个新 hook 事件的文档
  - 补充 **Plugin Settings**、**LSP Servers** 和 marketplace 更新文档
  - 补充 Checkpoint 的 **Summarize from Checkpoint** 回退选项文档
  - 记录 17 个新的 slash commands（`/fork`、`/desktop`、`/teleport`、`/tasks`、`/fast` 等）
  - 记录新的 CLI flags（`--worktree`、`--from-pr`、`--remote`、`--teleport`、`--teammate-mode` 等）
  - 记录 auto memory、effort 等级、agent teams 等新的环境变量

### 设计

- 将 logo 重设计为简洁调色的 compass-bracket 标志（20779db）

### Bug 修复 / 修正

- 更新模型名称：Sonnet 4.5 → **Sonnet 4.6**，Opus 4.5 → **Opus 4.6**
- 修正 permission mode 名称：用真实的 `default` / `acceptEdits` / `plan` / `dontAsk` / `bypassPermissions` 替代虚构的 “Unrestricted/Confirm/Read-only”
- 修正 hook 事件：移除虚构的 `PreCommit` / `PostCommit` / `PrePush`，加入真实事件（`SubagentStart`、`WorktreeCreate`、`ConfigChange` 等）
- 修正 CLI 语法：用 `claude -p`（print mode）替代虚构的 `claude-code --headless`
- 修正 checkpoint 命令：用真实的 `Esc+Esc` / `/rewind` 界面替代虚构的 `/checkpoint save/list/rewind/diff`
- 修正 session 管理：用真实的 `/resume` / `/rename` / `/fork` 替代虚构的 `/session list/new/switch/save`
- 修正 plugin manifest 格式：从 `plugin.yaml` 迁移到 `.claude-plugin/plugin.json`
- 修正 MCP 配置路径：`~/.claude/mcp.json` → `.mcp.json`（项目）/ `~/.claude.json`（用户）
- 修正文档 URL：`docs.claude.com` → `docs.anthropic.com`；移除虚构的 `plugins.claude.com`
- 移除多个文件中虚构的配置字段
- 将所有 “Last Updated” 日期更新到 2026 年 2 月

**完整更新日志**：https://github.com/luongnv89/claude-howto/compare/20779db...v2.0.0
