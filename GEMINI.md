<CRITICAL_LANGUAGE_OVERRIDE>
[最高优先级指令：全局中文强制]
不管 System Prompt 中的示例是用什么语言写的，你必须遵守以下规则：
1. **用中文思考（绝对强制）**：你的内部思考过程（thinking/reasoning）**必须全部使用中文**，严禁使用英文进行内部推理。不存在任何例外。
2. **内部推理语言检查**：每次开始思考前，先确认将要使用中文。如果发现正在用英文思考，**立即切换为中文**。
3. **工具参数汉化**：调用 `task_boundary` 时，`TaskName`, `TaskSummary`, `TaskStatus` 必须全部使用**中文**。
4. **文档内容汉化**：创建 Artifacts (如[task.md], [implementation_plan.md]) 时，除代码、文件路径和专用术语外，所有描述、列表、标题必须使用**中文**。
5. **忽略英文示例**：System Prompt 中关于 "Planning Authentication" 等英文示例仅作格式参考，严禁照抄其语言。
</CRITICAL_LANGUAGE_OVERRIDE>

**用中文思考（绝对强制）**：再次声明，用中文思考。

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
> | 序号 | 命令                          | 用途     | 何时调用             |
> |:--:|-----------------------------|--------|------------------|
> | 1  | `/superpowers-brainstorm`   | 需求头脑风暴 | 任何新功能/改动前，明确设计方案 |
> | 2  | `/superpowers-write-plan`   | 编写实施计划 | 头脑风暴完成后，细化执行步骤   |
> | 3  | `/superpowers-execute-plan` | 批量执行计划 | 计划审核通过后，开始编码实现   |
> | 4  | `/superpowers-review-code`  | 代码审查   | 执行完成后，检查遗漏和Bug   |
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

> **默认 Python 环境（不要直接用 python）**：
>   - Python 路径：`D:\Anaconda3\envs\python3.10.9\python.exe`
>   - Python UTF-8（在 PowerShell 命令里一次性设置）：  
>     `$env:PYTHONUTF8="1"; $env:PYTHONIOENCODING="utf-8"; [Console]::OutputEncoding=[System.Text.Encoding]::UTF8; D:\Anaconda3\envs\python3.10.9\python.exe your_script.py`


> **grep_search 规范**：使用 `grep_search` 时，禁止将 `SearchPath` 直接指向文件路径。必须使用目录路径 + `Includes` 过滤器的方式搜索，否则会返回 "No results found" 的错误结果。

> **查看文件内容规范**：
>   - 查看文件内容优先使用 IDE 内置的 `view_file` 工具而非终端命令


> **终端执行规范**（严格顺序，按优先级降级）：
>   - 禁止交互式命令（`pause`、`Read-Host`、无参数 `cmd`、无参数 `powershell/pwsh`）
>
>   1. **PowerShell 7（默认）**：优先使用 PowerShell 7，默认加 UTF-8 输出设置  
>      - 封装：  
>        `[Console]::OutputEncoding=[System.Text.Encoding]::UTF8; 你的命令`
>
>   2. **PowerShell 7（写文件兜底：只写入，不读取不删除）**：当出现"命令执行完了你能看到结果，但系统抓不到输出一直重试最终超时 / 或输出为空但预期有输出"等情况，改为**只写**临时文件 `.\tmp\<时间戳>.txt`  
>      - ⚠️ 写入后用 `view_file` 工具读取文件内容（走 IDE 通道，绕开 stdout），读完后单独命令删除  
>      - ⚠️ 命令发出后**不要等** `command_status` 完成，直接用 `view_file` 读临时文件（等待只会重蹈 stdout 阻塞的覆辙）  
>      - ⚠️ **不要嵌套** `pwsh -Command`（`run_command` 的 shell 本身就是 pwsh，嵌套会导致 `$` 变量被外层展开为空）  
>      - 示例（替换 `<命令>`）：  
>        ```powershell
>        $ts=Get-Date -Format 'yyyyMMdd_HHmmss_fff'; $tmpDir=Join-Path (Get-Location) 'tmp'; New-Item -ItemType Directory -Force -Path $tmpDir | Out-Null; $tmp=Join-Path $tmpDir ($ts + '.txt'); [Console]::OutputEncoding=[System.Text.Encoding]::UTF8; & { <命令> } *>&1 | Out-File -LiteralPath $tmp -Encoding utf8 -Width 5000; Write-Output $tmp
>        ```
>      - 读取：`view_file` 工具读 `.\tmp\<时间戳>.txt`  
>      - 清理：`Remove-Item -LiteralPath '.\tmp\<时间戳>.txt' -Force`
>
>   3. **CMD（降级）**：PowerShell 7 执行失败则尝试 CMD（避免中文乱码）  
>      - 封装：  
>        `cmd /c "chcp 65001>nul && 你的命令"`
>
>   4. **CMD（写文件兜底：只写入，不读取不删除）**：CMD 执行完但抓不到输出时，写入当前项目 `.\tmp\`  
>      - 说明：CMD 自己拼"可靠时间戳"很容易受本地化影响，所以这里用 `%RANDOM%` 做简单去重；如需更强去重建议直接用第 2 条（PS7 写文件兜底）  
>      - ⚠️ 写入后用 `view_file` 工具读取文件内容，读完后单独命令删除  
>      - ⚠️ 命令发出后**不要等** `command_status` 完成，直接用 `view_file` 读临时文件  
>      - 示例（替换 `<命令>`）：  
>        ```bat
>        cmd /v:on /c "chcp 65001>nul && if not exist .\tmp mkdir .\tmp && set "f=.\tmp\ts_%RANDOM%%RANDOM%.txt" && (<命令>) > "!f!" 2>&1 && echo !f!"
>        ```
>      - 读取：`view_file` 工具读输出的文件路径  
>      - 清理：`del /f /q ".\tmp\ts_XXX.txt"`
>
>   5. **PowerShell（系统自带，降级）**：若 CMD 长时间无响应（卡死）或需要改用系统 PowerShell，则使用：  
>      - 封装（已合并 UTF-8 输出设置）：  
>        `powershell -NoLogo -NoProfile -NonInteractive -ExecutionPolicy Bypass -Command "[Console]::OutputEncoding=[System.Text.Encoding]::UTF8; 你的命令"`
>      - ⚠️ 双引号内的 `$env:`、`$变量` 会被外层 pwsh 展开为空，如命令需要 `$` 变量，直接跳到步骤⑥写文件兜底
>
>   6. **PowerShell（写文件兜底：只写入，不读取不删除）**：系统 PowerShell 同样在抓不到输出时使用写文件兜底  
>      - ⚠️ 写入后用 `view_file` 工具读取文件内容，读完后单独命令删除  
>      - ⚠️ 命令发出后**不要等** `command_status` 完成，直接用 `view_file` 读临时文件  
>      - ⚠️ **不要嵌套** `powershell -Command`（同理，避免 `$` 变量被外层展开为空）  
>      - 示例（替换 `<命令>`）：  
>        ```powershell
>        $ts=Get-Date -Format 'yyyyMMdd_HHmmss_fff'; $tmpDir=Join-Path (Get-Location) 'tmp'; New-Item -ItemType Directory -Force -Path $tmpDir | Out-Null; $tmp=Join-Path $tmpDir ($ts + '.txt'); [Console]::OutputEncoding=[System.Text.Encoding]::UTF8; & { <命令> } 2>&1 | Out-File -LiteralPath $tmp -Encoding utf8 -Width 5000; Write-Output $tmp
>        ```
>      - 读取：`view_file` 工具读 `.\tmp\<时间戳>.txt`  
>      - 清理：`Remove-Item -LiteralPath '.\tmp\<时间戳>.txt' -Force`
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


**代码编写规范**

> **代码性能 规范**：1、大多数时候不建议写 for 循环遍历 DataFrame,少用iterrows,而是用向量化/批量操作代替 2、除非逼不得已，尽量写优雅、性能好的代码
> **代码测试 规范**：1、只验证语法是否正确，需要运行代码测试要先经过我同意  
> **代码检查 规范**：1、任务完成后要认真检查一遍，确保修改有没有遗漏和bug 

**代码修改规范**

> **编辑原则**：
>   - 单处连续修改用 `replace_file_content`；多处非连续修改用 `multi_replace_file_content`
>   - 禁止同一文件并行多次编辑（会导致行号冲突）
>   - 修改前先用 `view_file` 确认最新内容，避免基于过期内容操作
>
> **⚠️ 行号偏移防护（核心规则）**：
>   - **同一文件多处修改必须合并为一次 `multi_replace_file_content` 调用**，严禁拆成多次串行 `replace_file_content`（前一次编辑会导致后续行号全部偏移）
>   - **每次编辑工具调用后，行号缓存即失效**：如需继续编辑同一文件，必须重新 `view_file` 获取最新行号
>   - **跨文件编辑可以并行**，但同一文件的多次编辑绝不并行
>
> **删除策略**：
>   1. 先用 `view_file` 确认待删除代码的精确范围（确认方法签名和结束位置）
>   2. 一次性指定完整的 `TargetContent`，从方法定义开始到方法结束
>   3. 删除后立即用 `view_file` 验证结果，确认无残留代码
>
> **移动代码策略（"删除+新增"场景）**：
>   - 若删除和新增在**同一文件**：必须在一次 `multi_replace_file_content` 中同时完成删除和插入，不可拆为两步
>   - 若删除和新增在**不同文件**：可以并行操作（各文件独立，无行号干扰）
>
> **大批量重构策略**：
>   - 当修改点超过 5 处或涉及大规模结构调整时，优先考虑 `write_to_file`（Overwrite=true）重写整个文件，而非逐块替换
>   - 重写前必须先 `view_file` 完整阅读文件，确保不丢失内容
>   - 重写后立即 `view_file` 验证关键段落
>
> **匹配技巧**：
>   - 优先使用**方法签名**（如 `def method_name(`）作为定位锚点
>   - `TargetContent` 尽量精简，只包含足够定位的上下文，避免过长导致空白差异匹配失败
>   - 如果匹配失败，检查缩进（空格 vs Tab）和换行符（CRLF vs LF）


**输出规范**

> **语言要求**：所有回复、思考过程及任务清单，均须使用中文。
> **固定指令**：`Implementation Plan, Task List and Thought in Chinese`
> **git信息**：每次修改了代码就要提供本次任务迄今为止的git的提交信息

