---
description: 使用计划模板执行实施规划工作流以生成设计产出物。
---

给定作为参数提供的实施细节，执行以下操作：

1. 从仓库根目录运行 `.specify/scripts/bash/setup-plan.sh --json` 并解析JSON获取 FEATURE_SPEC, IMPL_PLAN, SPECS_DIR, BRANCH。所有未来文件路径必须是绝对路径。
2. 读取和分析功能规格说明以理解：
   - 功能需求和用户故事
   - 功能性和非功能性需求
   - 成功标准和验收标准
   - 任何提到的技术约束或依赖

3. 读取 `.specify/memory/constitution.md` 处的宪法以理解宪法要求。

4. 执行实施计划模板：
   - 加载 `.specify/templates/plan-template.md` (已复制到IMPL_PLAN路径)
   - 设置输入路径为FEATURE_SPEC
   - 运行执行流程(main)函数步骤1-9
   - 模板是自包含且可执行的
   - 按照指定的错误处理和关卡检查
   - 让模板指导在$SPECS_DIR中生成产出物：
     * 阶段0生成research.md
     * 阶段1生成data-model.md, contracts/, quickstart.md
     * 阶段2生成tasks.md
   - 将用户提供的细节从参数合并到技术上下文中: $ARGUMENTS
   - 在完成每个阶段时更新进度跟踪

5. 验证执行完成：
   - 检查进度跟踪显示所有阶段完成
   - 确保所有必需的产出物已生成
   - 确认执行中没有ERROR状态

6. 报告结果，包括分支名称、文件路径和生成的产出物。

对所有文件操作使用仓库根目录的绝对路径以避免路径问题。
