# 测试策略

## 单元测试

- 覆盖核心功能和边界情况
- 测试失败时先定位根因，不要盲目修改代码

## 集成测试

- 验证模块间的交互是否正常
- 检查数据流转和接口契约是否符合预期

## 端到端测试

- 验证完整的业务流程是否符合预期
- 涉及页面功能的需要使用 Playwright 进行模拟验证，确保页面元素和交互符合预期
- 涉及数据库操作的需要使用 JDBC 进行测试，确保数据库操作符合预期结果

## 测试命名规范

测试方法名应清晰描述测试场景：
- `testMethodName_givenCondition_expectedResult`
- 例：`testGetCommunity_givenInvalidId_returnsBadRequest`

## 测试数据管理

- 测试数据应独立，不依赖生产数据
- 测试完成后清理测试数据