---
description: 3、批量执行计划并进行审查检查点
---
1. 加载并运行 `%USERPROFILE%/.gemini/antigravity/skills/executing-plans/SKILL.md` 指令。
2. **步骤0 - 初始化会话文件**（复杂任务推荐）：
   - 加载 `%USERPROFILE%/.gemini/antigravity/skills/planning-with-files/SKILL.md`
   - 📂 在当前任务目录 `docs/tasks/YYYY-MM-DD-<任务标识符>/` 下创建：
     - `task_plan.md` — 阶段追踪
     - `findings.md` — 发现记录
     - `progress.md` — 进度日志
   - ⏱️ 每2个操作后保存关键发现到文件
3. **步骤1 - 加载并审查计划**：
   - 读取任务目录下的 `plan.md`
   - 批判性审查，识别问题或疑问
   - 有问题 → 先向用户确认
   - 无问题 → 创建任务清单
4. **步骤2 - 批量执行**（默认每批3个任务）：
   - 标记任务为进行中
   - 严格按计划步骤执行
   - 运行指定的验证命令
   - 标记任务为已完成
5. **步骤3 - 报告**：
   - 展示已实现的内容
   - 展示验证输出
   - 说：「等待反馈」
6. **步骤4 - 根据反馈继续**：
   - 应用修改（如需要）
   - 执行下一批任务
   - 重复直到完成
7. **长任务上下文管理**（任务超过20+步骤时）：
   - 加载 `%USERPROFILE%/.gemini/antigravity/skills/context-compression/SKILL.md`
   - 使用锚定迭代摘要法保留：会话意图、修改文件、决策、下一步
   - 每完成一个批次后更新结构化摘要到 `progress.md`
8. **何时停止并求助**：
   - 遇到阻塞（缺少依赖、测试失败、指令不清）
   - 计划有关键缺口
   - 验证反复失败
9. **完成后** → 运行 `/superpowers-finish-branch`
