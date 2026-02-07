---
description: 8、Git 分批提交并推送
---
1. **检查状态**（使用 PowerShell）：
   - 运行 `git status` 和 `git diff --stat` 查看待提交更改
   - 确认当前分支正确

2. **分批策略**（按优先级选择）：
   - 按功能模块分组（推荐）
   - 按文件类型分组（配置、代码、测试）
   - 按变更类型分组（新增、修改、删除）

3. **提交信息规范**（Conventional Commits）：
   - **格式**：
     ```
     <类型>(<范围>): <标题>

     - <变更1>
     - <变更2>
     ...
     ```
   - **类型**：feat | fix | refactor | docs | test | chore | style | perf
   - **标题**：一句话概述，控制在 50 字符内
   - **正文**：每个文件/模块的具体变更，使用 `- ` 列表格式
   - **示例**：
     ```
     refactor(auth): 重构登录模块并优化缓存

     - login.py: 提取验证逻辑到独立函数
     - cache.py: 添加 Redis 连接池支持
     - tests/test_login.py: 补充边界条件测试
     ```
   - ⚠️ 正文既不要太简单（仅标题）也不要太冗长（超过10行）

4. **执行提交**（使用 PowerShell，避免 CMD 中文编码问题）：
   - 暂存：`git add <文件列表>`
   - 提交（多行信息）：
     ```powershell
     git commit -m "<标题>" -m "<正文>"
     ```
   - 确认：`git log --oneline -1`

5. **推送与冲突处理**：
   - 提交完成后请求用户确认
   - 推送：`git push origin <当前分支>`
   - ⚠️ 若推送失败：先 `git pull --rebase`，解决冲突后重试
