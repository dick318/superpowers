---
description: 4、审查代码修改是否符合计划，检查遗漏和Bug
---
1. 加载并运行 `%USERPROFILE%/.gemini/antigravity/skills/requesting-code-review/SKILL.md` 指令。
2. **对照计划逐项检查**：
   - 获取 `plan.md` 或 `implementation_plan.md` 路径
   - 逐条核对计划中的每个任务是否已完成
   - 标记：✅ 已完成 | ❌ 未完成 | ⚠️ 部分完成
3. **代码质量审查**：
   - 按严重级别分类问题：阻塞(Blocker) / 主要(Major) / 次要(Minor) / 细节(Nit)
   - 重点关注：正确性、边界条件、错误处理、安全性
4. **遗漏检测**：
   - 检查是否有计划外的必要修改被遗漏
   - 验证跨文件依赖是否完整处理
5. **Bug 排查**：
   - 检查新引入的潜在 Bug
   - 验证现有功能是否受影响（回归风险）
6. 加载 `%USERPROFILE%/.gemini/antigravity/skills/verification-before-completion/SKILL.md` 进行验证确认。
7. **输出审查报告**：
   - 路径：`docs/artifacts/{任务标识符}/review.md`
   - ⚠️ **防重名**：若文件已存在，使用 `review_v2.md`、`review_v3.md` 递增命名
   - 包含：计划完成度、问题清单、风险评估、后续建议
