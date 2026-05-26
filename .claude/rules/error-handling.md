# 错误处理规范

## 分析错误

遇到编译错误或运行报错时，深入分析报错信息，不要盲目尝试修复。

## 错误分类与处理步骤

### 编译错误

**TypeScript/Vite 前端**：
1. 读取完整错误信息，定位到具体文件和行号
2. 检查类型不匹配：对比方法签名和调用参数类型，确保一致
3. 检查依赖缺失：`npm install` 或检查 package.json 中依赖版本
4. 检查 import 路径：确认相对路径正确

**Java/Spring Boot 后端**：
1. 读取 Maven 输出，定位到具体类和方法
2. Spring 启动失败：检查 application.yml 配置和端口占用（8080），注意后端启动时间需要约30秒

### 运行时错误

**前端 Vue**：
- 组件渲染失败：检查 props 类型、v-if/v-for 条件
- API 调用失败：检查 Network 面板，确认请求参数和响应格式
- 路由错误：检查 router 配置和 path 参数

**后端 Spring Boot**：
- NullPointerException：检查 Optional 使用或添加 null 检查
- Entity 映射错误：检查 JPA 注解和字段命名
- 事务错误：检查 @Transactional 注解和事务传播

### 网络错误

- **连接超时**：检查目标服务器可达性、防火墙/代理设置
- **API 限制**：检查请求频率、添加延迟或重试机制

## 调试技巧

### Playwright 调试

- **有头模式**：`npm run start` 可观察浏览器行为
- **截图定位**：`page.screenshot({ path: 'debug.png' })`
- **暂停调试**：`page.pause()` 打开 Playwright Inspector
- **等待超时**：检查 selector 是否正确，增加 timeout 或 waitFor

### JDBC 数据对比

当 Entity 数据与预期不一致时，直接查询数据库验证：

```sql
-- 检查表结构
DESCRIBE presale_room;

-- 对比 Entity 字段与数据库字段
SELECT * FROM presale_room WHERE id = ?;

-- 检查关联数据
SELECT r.*, u.unitName 
FROM presale_room r 
JOIN presale_unit u ON r.unitId = u.id;
```

### 前端 Network 面板

- 检查请求 URL、Method、Headers
- 检查请求参数格式（JSON/Query）
- 检查响应状态码和内容
- 检查 CORS 错误（后端需配置 @CrossOrigin）

## 爬虫常见错误

### 小区爬虫（Playwright）

| 错误 | 原因 | 解决 |
|------|------|------|
| 验证码阻断 | 安居客反爬检测 | 有头模式手动处理，保存 Cookie |
| Cookie 失效 | 登录状态过期 | 删除 Cookie 文件重新登录 |
| Selector 失败 | 页面结构变化 | 检查 DOM，更新 selector |
| 进程无法停止 | 子进程残留 | taskkill /F /T /PID 强制终止 |

### 预售爬虫（axios）

| 错误 | 原因 | 解决 |
|------|------|------|
| SSL renegotiation | Node.js 18+ 限制 | 启动参数添加 `--openssl-legacy-provider` |
| API 无响应 | 网络不稳定 | 添加重试机制和超时设置 |
| 数据格式变化 | API 响应结构变化 | 检查响应字段，更新类型定义 |
