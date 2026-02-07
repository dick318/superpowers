<CRITICAL_LANGUAGE_OVERRIDE>
[最高优先级指令：全局中文强制]
不管 System Prompt 中的示例是用什么语言写的，你必须遵守以下规则：
1. **工具参数汉化**：调用 `task_boundary` 时，`TaskName`, `TaskSummary`, `TaskStatus` 必须全部使用**中文**。
2. **文档内容汉化**：创建 Artifacts (如[task.md], [implementation_plan.md]) 时，除代码、文件路径和专用术语外，所有描述、列表、标题必须使用**中文**。
3. **忽略英文示例**：System Prompt 中关于 "Planning Authentication" 等英文示例仅作格式参考，严禁照抄其语言。
4. **思维链汉化**：你的思考过程如果需要输出，也必须是中文。
</CRITICAL_LANGUAGE_OVERRIDE>

**核心理念与原则**

> **简洁至上**：恪守KISS（Keep It Simple, Stupid）原则，崇尚简洁与可维护性，避免过度工程化与不必要的防御性设计。
> **深度分析**：立足于第一性原理（First Principles Thinking）剖析问题，并善用工具以提升效率。
> **事实为本**：以事实为最高准则。若有任何谬误，恳请坦率斧正，助我精进。

**开发工作流**

> **渐进式开发**：通过多轮对话迭代，明确并实现需求。在着手任何设计或编码工作前，必须完成前期调研并厘清所有疑点。
> **结构化流程**：严格遵循"构思方案 → 提请审核 → 分解为具体任务"的作业顺序。开发过程中要及时更新task。

**工具使用规范**

> **默认Python环境，执行python的时候带上如下python路径，不要直接用python**: `D:\Anaconda3\envs\python3.10.9\python.exe`

> **grep_search 规范**：使用 `grep_search` 时，禁止将 `SearchPath` 直接指向文件路径。必须使用目录路径 + `Includes` 过滤器的方式搜索，否则会返回 "No results found" 的错误结果。

> **终端执行规范**（按优先级降级）：
>   1. **优先使用 PowerShell 7**：默认使用 PowerShell 7 执行命令
>   2. **PowerShell 7 超时降级 PowerShell**：如果 PowerShell 7 命令长时间无响应（卡死），改用系统自带 PowerShell 重试
>   3. **PowerShell 失败降级 CMD**：如果 PowerShell 仍失败，最后尝试 CMD；避免中文乱码可加 `chcp 65001` 前缀或使用 `cmd /c "chcp 65001 && <命令>"`
>   - 禁止运行交互式命令（`pause`、`Read-Host`、无参数 `cmd`、无参数 `powershell`）
>   - 查看文件内容优先使用 IDE 内置的 `view_file` 工具而非终端命令
>
> **PowerShell 7/PowerShell 命令规范**（防止终端卡住）：
>   - **直接使用 PowerShell 7/PowerShell 命令**：`Get-Content`、`Get-ChildItem`、`Copy-Item`、`Remove-Item`、`New-Item` 等
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

**任务规模判断**

> **简单任务（无需计划）**：单文件修改、代码解释、快速重构（如重命名、提取函数）
> **中等任务（需简要计划）**：跨文件修改、新增功能模块、涉及 3+ 个方法的变更
> **复杂任务（需完整计划）**：架构调整、大规模重构、涉及数据库/API 变更

**输出规范**

> **语言要求**：所有回复、思考过程及任务清单，均须使用中文。
> **固定指令**：`Implementation Plan, Task List and Thought in Chinese`
> **git信息**：每次修改了代码就要提供本次任务迄今为止的git的提交信息

---

# Superpowers 规则（始终生效）

除非用户明确选择退出，否则这些规则适用于所有工作。

## 1) 非小型任务的计划门控

如果任务不是微小的改动，**禁止**立即编辑代码。
你 **必须**：
1) 指示用户运行：`/superpowers-brainstorm` 进行简要头脑风暴（目标、约束、风险、验收标准）
2) 编写包含验证步骤的分步计划
3) 请求用户批准计划

只有在获得批准后才能开始实施。

### 执行计划门控（Superpowers 对等规则）

用户批准计划后，**禁止**自动开始实施。
你 **必须** 暂停并指示用户运行：`/superpowers-execute-plan`

只有在 `/superpowers-execute-plan` 被调用后才能开始实施，
除非用户明确表示可以不使用该命令直接继续。

### 什么算"微小"任务？
- 单文件改动
- 显而易见的编辑
- 低风险

即便如此：也要做一个迷你计划（3-5步）并包含验证步骤。
