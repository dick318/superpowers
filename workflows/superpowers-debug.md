---
description: 6、遇到Bug或异常行为时启动系统化调试流程
---
1. 加载并运行 `%USERPROFILE%/.gemini/antigravity/skills/systematic-debugging/SKILL.md` 指令。
2. **严格遵循四阶段调试法**：
   - **阶段1 - 根因调查**：仔细阅读错误信息、稳定复现、检查近期改动、收集证据
   - **阶段2 - 模式分析**：查找类似的正常代码、对比差异
   - **阶段3 - 假设测试**：形成单一假设、最小化测试、验证
   - **阶段4 - 实施修复**：创建失败测试用例、实施单一修复、验证
3. **铁律**：不找到根因，禁止尝试修复
4. **连续失败3次**：停止修复，质疑架构设计
5. **复杂调试的文件规划**（调试超过3轮时）：
   - 加载 `%USERPROFILE%/.gemini/antigravity/skills/planning-with-files/SKILL.md`
   - 📂 在任务目录 `docs/tasks/YYYY-MM-DD-<任务标识符>/` 下创建 `findings.md`
   - 将每次尝试、错误信息、假设记录到 `findings.md`
   - 遵循3次失败协议，避免重复无效尝试
6. 如需回归测试，加载 `%USERPROFILE%/.gemini/antigravity/skills/test-driven-development/SKILL.md`
