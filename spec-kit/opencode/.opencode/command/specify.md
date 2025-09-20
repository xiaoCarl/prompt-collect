---
description: 从自然语言功能描述创建或更新功能规格说明。
---

给定作为参数提供的功能描述，执行以下操作：

1. 从仓库根目录运行脚本 `.specify/scripts/bash/create-new-feature.sh --json "$ARGUMENTS"` 并解析其JSON输出以获取BRANCH_NAME和SPEC_FILE。所有文件路径必须是绝对路径。
  **重要** 您必须只运行此脚本一次。JSON在终端中作为输出提供 - 始终引用它以获取您正在查找的实际内容。
2. 加载 `.specify/templates/spec-template.md` 以理解必需的章节。
3. 使用模板结构将规格说明写入SPEC_FILE，用从功能描述(参数)派生的具体细节替换占位符，同时保留章节顺序和标题。
4. 报告完成情况，包括分支名称、规格说明文件路径以及下一阶段的准备情况。

注意：脚本在写入之前创建并检出新分支并初始化规格说明文件。
