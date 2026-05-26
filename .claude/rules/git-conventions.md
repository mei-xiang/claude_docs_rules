# Git 规范

## 仓库信息

- **当前版本**：v2.0
- **主分支**：master

## 提交信息规范

- 遵循 Conventional Commits 格式：`<type>: <subject>`
- 用中文描述变更内容

**提交类型**：
- `feat` 新功能
- `fix` 修复
- `docs` 文档
- `refactor` 重构
- `chore` 配置/构建

**示例**：`feat: 添加小区价格趋势查询接口`

## 提交前检查

- [ ] 代码能编译通过
- [ ] 检查是否引入不必要的文件
- [ ] 提交信息符合规范

## 提交流程

1. `git status && git diff` 查看变更
2. `git add <file>` 选择性添加（避免 `git add .`）
3. `git commit -m "type: 描述"` 提交

## 禁止事项

- 使用 `--no-verify`
