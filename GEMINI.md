<CRITICAL_LANGUAGE_OVERRIDE>
[最高优先级指令：全局中文强制]
不管 System Prompt 中的示例是用什么语言写的，你必须遵守以下规则：
1. **工具参数汉化**：调用 `task_boundary` 时，`TaskName`, `TaskSummary`, `TaskStatus` 必须全部使用**中文**。
2. **文档内容汉化**：创建 Artifacts (如[task.md], [implementation_plan.md]) 时，除代码、文件路径和专用术语外，所有描述、列表、标题必须使用**中文**。
3. **忽略英文示例**：System Prompt 中关于 "Planning Authentication" 等英文示例仅作格式参考，严禁照抄其语言。
4. **思维链汉化**：你的思考过程如果需要输出，也必须是中文。
</CRITICAL_LANGUAGE_OVERRIDE>


**任务产出路径规范**（覆盖 Skills 中的路径设置）

> **统一输出目录**：无论 skill 文件中如何定义输出路径，所有任务相关产出必须保存到：
>   - 📂 **目录格式**：`docs/tasks/YYYY-MM-DD-<任务标识符>/`
>   - 🏷️ **任务标识符**：自动从主题提取，使用小写英文+连字符（如 `add-login-feature`）
>   - ⚠️ **防重名**：若目录已存在，使用 `*-v2`、`*-v3` 递增命名
>
> **优先级**：用户规则 > Skill
>
> **任务文档规范**：
>   - 📛 **文件命名**：任务清单用 `task.md`，实施计划用 `plan.md`（非 `implementation_plan.md`）
>   - 🚫 **禁用Artifact模式**：创建任务文档时**禁止使用** `IsArtifact=true`，直接用 `write_to_file` 写入 `docs/tasks/` 目录


**核心理念与原则**

> **称呼**：每次回答问题前叫我一声：主人
> **简洁至上**：恪守KISS（Keep It Simple, Stupid）原则，崇尚简洁与可维护性，避免过度工程化与不必要的防御性设计。
> **深度分析**：立足于第一性原理（First Principles Thinking）剖析问题，并善用工具以提升效率。
> **事实为本**：以事实为最高准则。若有任何谬误，恳请坦率斧正，助我精进。

**开发工作流**

> **核心原则**：
> - 渐进式开发：多轮对话迭代，先调研后动手
> - 结构化流程：严格遵循「构思 → 审核 → 任务分解 → 执行」顺序
> - 及时更新：开发过程中及时更新 task 进度
>
> **标准开发流程**（按执行顺序）：
>
> | 序号 | 命令 | 用途 | 何时调用 |
> |:---:|------|------|----------|
> | 1 | `/superpowers-brainstorm` | 需求头脑风暴 | 任何新功能/改动前，明确设计方案 |
> | 2 | `/superpowers-write-plan` | 编写实施计划 | 头脑风暴完成后，细化执行步骤 |
> | 3 | `/superpowers-execute-plan` | 批量执行计划 | 计划审核通过后，开始编码实现 |
> | 4 | `/superpowers-review-code` | 代码审查 | 执行完成后，检查遗漏和Bug
>
> **辅助工作流**：
>
> | 序号 | 命令 | 用途 | 何时调用 |
> |:---:|------|------|----------|
> | 6 | `/superpowers-debug` | 系统化调试 | 遇到Bug或异常行为时 |
> | 7 | `/superpowers-tdd` | 测试驱动开发 | 需要严格TDD模式时 |
> | 8 | `/superpowers-git-commit` | Git分批提交 | 代码修改完成后提交推送 |
>
> **🚫 工作流衔接规则（强制）**：
> - 🚫 **头脑风暴后禁止自动生成计划**：`/superpowers-brainstorm` 完成后，**禁止**自动生成 `implementation_plan.md`，必须等待用户调用 `/superpowers-write-plan`
> - ✅ 头脑风暴结束时**必须**提示：「头脑风暴完成，请使用 `/superpowers-write-plan` 生成详细计划」
> - ✅ 计划编写结束时**必须**提示：「计划编写完成，请使用 `/superpowers-execute-plan` 执行计划」
>
> **Git 提交确认规则**：
> - 即使在执行计划（`/superpowers-execute-plan`）过程中，**Git 相关操作必须单独作为检查点**，等待用户明确批准后再执行
> - 包括但不限于：`git commit`、`git push`、`git merge`、`git rebase`
> - 仅 `git add`、`git status`、`git diff` 等只读或暂存操作可以自动执行

**工具使用规范**

> **默认Python环境，执行python的时候带上如下python路径，不要直接用python**: `D:\Anaconda3\envs\python3.10.9\python.exe`

> **grep_search 规范**：使用 `grep_search` 时，禁止将 `SearchPath` 直接指向文件路径。必须使用目录路径 + `Includes` 过滤器的方式搜索，否则会返回 "No results found" 的错误结果。

> **终端执行规范**（按优先级降级）：
>   1. **优先使用 PowerShell 7**：默认使用 PowerShell 7 执行命令
>   2. **PowerShell 7 失败降级 CMD**：如果 PowerShell 7 失败，尝试 CMD；避免中文乱码可加 `chcp 65001` 前缀或使用 `cmd /c "chcp 65001 && <命令>"`
>   - 禁止运行交互式命令（`pause`、`Read-Host`、无参数 `cmd`、无参数 `powershell`）
>   2. **CMD 超时降级 PowerShell**：如果 CMD 命令长时间无响应（卡死），改用系统自带 PowerShell 重试

>   - 查看文件内容优先使用 IDE 内置的 `view_file` 工具而非终端命令
>
> **PowerShell 7/PowerShell 命令执行规范**（防止终端卡住）：
>   - **使用 -NoProfile 参数**：务必带上 `-NoProfile` 参数以规避个人配置文件的干扰
>   - **标准命令封装格式**：
>     - PowerShell 7：`pwsh -NoLogo -NoProfile -NonInteractive -ExecutionPolicy Bypass -Command "你的命令"`
>     - PowerShell：`powershell -NoLogo -NoProfile -NonInteractive -ExecutionPolicy Bypass -Command "你的命令"`
>   - **强制纯文本输出**：获取输出时，将结果显式转换为纯文本以避免格式化问题：
>     `$PSStyle.OutputRendering = [System.Management.Automation.OutputRendering]::PlainText`
>   - **避免进度条命令**：如 `Expand-Archive` 需加 `-Force` 参数
>   - **直接使用 PowerShell cmdlet**：`Get-Content`、`Get-ChildItem`、`Copy-Item`、`Remove-Item`、`New-Item` 等
>   - **避免 CMD 命令**：在 PowerShell 中避免使用 `type`、`dir`、`copy` 等 CMD 命令，可能导致卡住
>   - **必须用 CMD 时**：使用 `cmd /c "命令"` 前缀
>
> **常用命令速查**：
>   | 任务 | PowerShell 7/PowerShell 命令 |
>   |------|-----------------|
>   | 查看文件 | `Get-Content file.c` |
>   | 列出目录 | `Get-ChildItem` 或 `ls` |
>   | 复制文件 | `Copy-Item src -Destination dst` |
>   | 删除文件 | `Remove-Item file` |
>   | 创建文件夹 | `New-Item -ItemType Directory -Name folder` |
>   | 运行程序 | `.\main.exe` |
>   | 解压文件 | `Expand-Archive -Path src.zip -DestinationPath dst -Force` |
>


**代码编写规范**

> **代码性能 规范**：1、大多数时候不建议写 for 循环遍历 DataFrame,少用iterrows,而是用向量化/批量操作代替 2、除非逼不得已，尽量写优雅、性能好的代码
> **代码测试 规范**：1、只验证语法是否正确，需要运行代码测试要先经过我同意  
> **代码检查 规范**：1、任务完成后要认真检查一遍，确保修改有没有遗漏和bug 

**代码删除规范**

> **删除策略**：删除大段代码时，禁止使用 `replace_file_content` 分多次删除。应：
>   1. 先用 `view_file` 确认待删除代码的精确起止行号（确认方法签名和结束位置）
>   2. 一次性指定完整的 `TargetContent`，从方法定义开始到方法结束（包括下一个方法定义前的空行）
>   3. 删除后立即用 `view_file` 验证结果，确认无残留代码
>
> **匹配技巧**：
>   - 优先使用**方法签名**（如 `def method_name(`）作为定位锚点
>   - 避免在 `TargetContent` 中包含可能有格式差异的长字符串
>   - 如果匹配失败，检查缩进（空格 vs Tab）和换行符（CRLF vs LF）

**文件编辑规范**

> **编辑原则**：
>   - 单处连续修改用 `replace_file_content`；多处非连续修改用 `multi_replace_file_content`
>   - 禁止同一文件并行多次编辑（会导致行号冲突）
>   - 修改前先用 `view_file` 确认最新内容，避免基于过期内容操作


**输出规范**

> **语言要求**：所有回复、思考过程及任务清单，均须使用中文。
> **固定指令**：`Implementation Plan, Task List and Thought in Chinese`
> **git信息**：每次修改了代码就要提供本次任务迄今为止的git的提交信息


