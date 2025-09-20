# 任务: [功能名称]

**输入**: 设计文档来自 `/specs/[###-功能名称]/`
**前提条件**: plan.md (必需), research.md, data-model.md, contracts/

## 执行流程 (main)
```
1. 从功能目录加载plan.md
   → 如果未找到: 错误 "未找到实施计划"
   → 提取: 技术栈, 库, 结构
2. 加载可选设计文档:
   → data-model.md: 提取实体 → 模型任务
   → contracts/: 每个文件 → 契约测试任务
   → research.md: 提取决策 → 设置任务
3. 按类别生成任务:
   → 设置: 项目初始化, 依赖, 代码检查
   → 测试: 契约测试, 集成测试
   → 核心: 模型, 服务, CLI命令
   → 集成: 数据库, 中间件, 日志
   → 优化: 单元测试, 性能, 文档
4. 应用任务规则:
   → 不同文件 = 标记 [P] 表示并行
   → 相同文件 = 顺序执行 (无 [P])
   → 测试先于实施 (TDD)
5. 顺序编号任务 (T001, T002...)
6. 生成依赖图
7. 创建并行执行示例
8. 验证任务完整性:
   → 所有契约都有测试吗？
   → 所有实体都有模型任务吗？
   → 所有端点都实施了吗？
9. 返回: 成功 (任务准备就绪可用于执行)
```

## 格式: `[ID] [P?] 描述`
- **[P]**: 可以并行运行 (不同文件, 无依赖关系)
- 在描述中包含确切文件路径

## 路径约定
- **单项目**: `src/`, `tests/` 在仓库根目录
- **Web应用**: `backend/src/`, `frontend/src/`
- **移动端**: `api/src/`, `ios/src/` 或 `android/src/`
- 下面显示的路径假设单项目 - 根据plan.md结构调整

## 阶段3.1: 设置
- [ ] T001 根据实施计划创建项目结构
- [ ] T002 使用[框架]依赖初始化[语言]项目
- [ ] T003 [P] 配置代码检查和格式化工具

## 阶段3.2: 测试优先 (TDD) ⚠️ 必须在3.3之前完成
**关键: 这些测试必须编写并且必须在任何实施之前失败**
- [ ] T004 [P] 契约测试 POST /api/users 在 tests/contract/test_users_post.py
- [ ] T005 [P] 契约测试 GET /api/users/{id} 在 tests/contract/test_users_get.py
- [ ] T006 [P] 集成测试用户注册在 tests/integration/test_registration.py
- [ ] T007 [P] 集成测试认证流程在 tests/integration/test_auth.py

## 阶段3.3: 核心实施 (仅在测试失败后)
- [ ] T008 [P] 用户模型在 src/models/user.py
- [ ] T009 [P] UserService CRUD 在 src/services/user_service.py
- [ ] T010 [P] CLI --create-user 在 src/cli/user_commands.py
- [ ] T011 POST /api/users 端点
- [ ] T012 GET /api/users/{id} 端点
- [ ] T013 输入验证
- [ ] T014 错误处理和日志

## 阶段3.4: 集成
- [ ] T015 连接UserService到数据库
- [ ] T016 认证中间件
- [ ] T017 请求/响应日志
- [ ] T018 CORS和安全头

## 阶段3.5: 优化
- [ ] T019 [P] 验证的单元测试在 tests/unit/test_validation.py
- [ ] T020 性能测试 (<200ms)
- [ ] T021 [P] 更新 docs/api.md
- [ ] T022 移除重复
- [ ] T023 运行 manual-testing.md

## 依赖关系
- 测试 (T004-T007) 先于实施 (T008-T014)
- T008 阻塞 T009, T015
- T016 阻塞 T018
- 实施先于优化 (T019-T023)

## 并行示例
```
# 同时启动 T004-T007:
任务: "契约测试 POST /api/users 在 tests/contract/test_users_post.py"
任务: "契约测试 GET /api/users/{id} 在 tests/contract/test_users_get.py"
任务: "集成测试注册在 tests/integration/test_registration.py"
任务: "集成测试认证在 tests/integration/test_auth.py"
```

## 注意事项
- [P] 任务 = 不同文件, 无依赖关系
- 在实施前验证测试失败
- 每个任务后提交
- 避免: 模糊任务, 相同文件冲突

## 任务生成规则
*在main()执行期间应用*

1. **从契约**:
   - 每个契约文件 → 契约测试任务 [P]
   - 每个端点 → 实施任务
   
2. **从数据模型**:
   - 每个实体 → 模型创建任务 [P]
   - 关系 → 服务层任务
   
3. **从用户故事**:
   - 每个故事 → 集成测试 [P]
   - 快速开始场景 → 验证任务

4. **排序**:
   - 设置 → 测试 → 模型 → 服务 → 端点 → 优化
   - 依赖关系阻止并行执行

## 验证检查清单
*关卡: 在main()返回前检查*

- [ ] 所有契约都有相应的测试
- [ ] 所有实体都有模型任务
- [ ] 所有测试都先于实施
- [ ] 并行任务真正独立
- [ ] 每个任务指定确切文件路径
- [ ] 没有任务修改与另一个[P]任务相同的文件