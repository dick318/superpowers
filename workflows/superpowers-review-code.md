---
description: 4、审查代码修改是否符合计划，检查遗漏和Bug
---
1. 加载并运行 `%USERPROFILE%/.gemini/antigravity/skills/requesting-code-review/SKILL.md` 指令。
2. **定位任务目录**：
   - 📂 使用当前任务目录 `docs/tasks/YYYY-MM-DD-<任务标识符>/`
   - 读取其中的 `plan.md` 作为审查基准
3. **对照计划逐项检查**：
   - 逐条核对计划中的每个任务是否已完成
   - 标记：✅ 已完成 | ❌ 未完成 | ⚠️ 部分完成
4. **代码质量审查**：
   - 按严重级别分类问题：阻塞(Blocker) / 主要(Major) / 次要(Minor) / 细节(Nit)
   - 重点关注：正确性、边界条件、错误处理、安全性
5. **遗漏检测**：
   - 检查是否有计划外的必要修改被遗漏
   - 验证跨文件依赖是否完整处理
6. **Bug 排查**：
   - 检查新引入的潜在 Bug
   - 验证现有功能是否受影响（回归风险）
7. 加载 `%USERPROFILE%/.gemini/antigravity/skills/verification-before-completion/SKILL.md` 进行验证确认。
8. **输出审查报告**：
   - 📄 路径：`docs/tasks/YYYY-MM-DD-<任务标识符>/review.md`
   - 包含：计划完成度、问题清单、风险评估、后续建议
