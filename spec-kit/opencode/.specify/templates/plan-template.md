
# 实施计划: [功能名称]

**分支**: `[###-功能名称]` | **日期**: [日期] | **规格**: [链接]
**输入**: 功能规格说明来自 `/specs/[###-功能名称]/spec.md`

## 执行流程 (/plan 命令范围)
```
1. 从输入路径加载功能规格说明
   → 如果未找到: 错误 "在{path}处未找到功能规格说明"
2. 填写技术上下文 (扫描NEEDS CLARIFICATION标记)
   → 根据上下文检测项目类型 (web=前端+后端, mobile=应用+API)
   → 根据项目类型设置结构决策
3. 根据宪法文档内容填写宪法检查部分
4. 评估下面的宪法检查部分
   → 如果存在违规: 在复杂性跟踪中记录
   → 如果无法提供理由: 错误 "首先简化方法"
   → 更新进度跟踪: 初始宪法检查
5. 执行阶段0 → research.md
   → 如果仍有NEEDS CLARIFICATION: 错误 "解决未知问题"
6. 执行阶段1 → contracts, data-model.md, quickstart.md, 智能体特定模板文件 (例如: `CLAUDE.md`, `.github/copilot-instructions.md`, `GEMINI.md`, `QWEN.md` 或 `AGENTS.md`)
7. 重新评估宪法检查部分
   → 如果有新的违规: 重构设计, 返回阶段1
   → 更新进度跟踪: 设计后宪法检查
8. 规划阶段2 → 描述任务生成方法 (不要创建tasks.md)
9. 停止 - 准备执行/tasks命令
```

**重要**: /plan 命令在第7步停止。阶段2-4由其他命令执行:
- 阶段2: /tasks 命令创建 tasks.md
- 阶段3-4: 实施执行 (手动或通过工具)

## 摘要
[从功能规格说明中提取: 主要需求 + 研究中的技术方法]

## 技术上下文
**语言/版本**: [例如: Python 3.11, Swift 5.9, Rust 1.75 或 NEEDS CLARIFICATION]  
**主要依赖**: [例如: FastAPI, UIKit, LLVM 或 NEEDS CLARIFICATION]  
**存储**: [如果适用, 例如: PostgreSQL, CoreData, 文件 或 N/A]  
**测试**: [例如: pytest, XCTest, cargo test 或 NEEDS CLARIFICATION]  
**目标平台**: [例如: Linux服务器, iOS 15+, WASM 或 NEEDS CLARIFICATION]
**项目类型**: [single/web/mobile - 决定源代码结构]  
**性能目标**: [领域特定, 例如: 1000 请求/秒, 10k 行/秒, 60 fps 或 NEEDS CLARIFICATION]  
**约束**: [领域特定, 例如: <200ms p95, <100MB 内存, 离线能力 或 NEEDS CLARIFICATION]  
**规模/范围**: [领域特定, 例如: 10k 用户, 1M 代码行, 50 个屏幕 或 NEEDS CLARIFICATION]

## 宪法检查
*关卡: 必须在阶段0研究前通过。阶段1设计后重新检查。*

[根据宪法文件确定的关卡]

## 项目结构

### 文档 (本功能)
```
specs/[###-功能]/
├── plan.md              # 本文件 (/plan 命令输出)
├── research.md          # 阶段0输出 (/plan 命令)
├── data-model.md        # 阶段1输出 (/plan 命令)
├── quickstart.md        # 阶段1输出 (/plan 命令)
├── contracts/           # 阶段1输出 (/plan 命令)
└── tasks.md             # 阶段2输出 (/tasks 命令 - 不由/plan创建)
```

### 源代码 (仓库根目录)
```
# 选项1: 单项目 (默认)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# 选项2: Web应用 (当检测到"前端" + "后端"时)
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# 选项3: 移动端 + API (当检测到"iOS/Android"时)
api/
└── [同上backend结构]

ios/ 或 android/
└── [平台特定结构]
```

**结构决策**: [默认为选项1，除非技术上下文表明是web/mobile应用]

## 阶段0: 大纲与研究
1. **从技术上下文中提取未知问题**:
   - 每个NEEDS CLARIFICATION → 研究任务
   - 每个依赖 → 最佳实践任务
   - 每个集成 → 模式任务

2. **生成并派遣研究智能体**:
   ```
   对于技术上下文中的每个未知问题:
     任务: "研究{未知问题}以支持{功能上下文}"
   对于每个技术选择:
      任务: "查找{领域}中{技术}的最佳实践"
   ```

3. **在`research.md`中整合发现**，使用格式:
   - 决策: [选择了什么]
   - 理由: [为什么选择]
   - 考虑的替代方案: [评估了哪些其他选项]

**输出**: research.md，所有NEEDS CLARIFICATION已解决

## 阶段1: 设计与契约
*前提条件: research.md 完成*

1. **从功能规格说明中提取实体** → `data-model.md`:
   - 实体名称、字段、关系
   - 来自需求的验证规则
   - 如果适用，状态转换

2. **从功能需求生成API契约**:
   - 每个用户操作 → 端点
   - 使用标准REST/GraphQL模式
   - 输出OpenAPI/GraphQL schema到`/contracts/`

3. **从契约生成契约测试**:
   - 每个端点一个测试文件
   - 断言请求/响应schema
   - 测试必须失败 (尚未实施)

4. **从用户故事中提取测试场景**:
   - 每个故事 → 集成测试场景
   - 快速开始测试 = 故事验证步骤

5. **增量更新智能体文件** (O(1)操作):
   - 运行`.specify/scripts/bash/update-agent-context.sh opencode`为您的AI助手
   - 如果存在: 仅添加当前计划中的新技术
   - 保留标记之间的手动添加内容
   - 更新最近变更 (保留最后3个)
   - 保持在150行以内以提高token效率
   - 输出到仓库根目录

**输出**: data-model.md, /contracts/*, 失败的测试, quickstart.md, 智能体特定文件

## 阶段2: 任务规划方法
*本节描述/tasks命令将做什么 - 不要在/plan期间执行*

**任务生成策略**:
- 加载`.specify/templates/tasks-template.md`作为基础
- 从阶段1设计文档生成任务 (契约, 数据模型, 快速开始)
- 每个契约 → 契约测试任务 [P]
- 每个实体 → 模型创建任务 [P] 
- 每个用户故事 → 集成测试任务
- 实施任务以使测试通过

**排序策略**:
- TDD顺序: 测试先于实施
- 依赖顺序: 模型先于服务先于UI
- 标记[P]表示并行执行 (独立文件)

**预计输出**: 25-30个编号、有序的任务在tasks.md中

**重要**: 此阶段由/tasks命令执行，不由/plan执行

## 阶段3+: 未来实施
*这些阶段超出/plan命令的范围*

**阶段3**: 任务执行 (/tasks命令创建tasks.md)  
**阶段4**: 实施 (执行tasks.md，遵循宪法原则)  
**阶段5**: 验证 (运行测试, 执行quickstart.md, 性能验证)

## 复杂性跟踪
*仅在宪法检查有必须证明的违规时填写*

| 违规 | 为什么需要 | 拒绝更简单替代方案的原因 |
|-----------|------------|-------------------------------------|
| [例如: 第4个项目] | [当前需求] | [为什么3个项目不足] |
| [例如: 仓库模式] | [特定问题] | [为什么直接数据库访问不足] |


## 进度跟踪
*此检查清单在执行流程中更新*

**阶段状态**:
- [ ] 阶段0: 研究完成 (/plan 命令)
- [ ] 阶段1: 设计完成 (/plan 命令)
- [ ] 阶段2: 任务规划完成 (/plan 命令 - 仅描述方法)
- [ ] 阶段3: 任务生成完成 (/tasks 命令)
- [ ] 阶段4: 实施完成
- [ ] 阶段5: 验证通过

**关卡状态**:
- [ ] 初始宪法检查: 通过
- [ ] 设计后宪法检查: 通过
- [ ] 所有NEEDS CLARIFICATION已解决
- [ ] 复杂性偏差已记录

---
*基于宪法 v2.1.1 - 参见`/memory/constitution.md`*
