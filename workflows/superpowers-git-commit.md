---
description: 8、Git 分批提交并推送
---
1. **检查状态**：
   - 运行 `git status` 和 `git diff` 查看待提交更改
   - 确认当前分支正确

2. **分批策略**（按优先级选择）：
   - 按功能模块分组（推荐）
   - 按文件类型分组（配置、代码、测试）
   - 按变更类型分组（新增、修改、删除）

3. **提交信息规范**（Conventional Commits）：
   - 格式：`<类型>(<范围>): <描述>`
   - 类型：feat | fix | refactor | docs | test | chore | style | perf
   - 示例：`feat(auth): 添加用户登录功能`
   - ⚠️ 描述使用中文，控制在 50 字符内

4. **执行提交**：
   - 使用 `git add <文件>` 暂存逻辑相关的更改
   - 提交：`git commit -m "<提交信息>"`
   - 每次提交后确认 `git log --oneline -1`

5. **推送与冲突处理**：
   - 提交完成后请求用户确认
   - 推送：`git push origin <当前分支>`
   - ⚠️ 若推送失败：先 `git pull --rebase`，解决冲突后重试
