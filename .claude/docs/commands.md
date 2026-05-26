# 命令汇总

## Frontend

在 `frontend` 目录下运行：

- `npm install` - 安装依赖
- `npm run dev` - 启动开发服务器 (http://localhost:5173)
- `npm run build` - 生产构建 (vue-tsc 类型检查 + Vite 构建)
- `npm run preview` - 预览生产构建

## Backend

在 `backend` 目录下运行：

- `./apache-maven-3.9.14/bin/mvn.cmd clean compile` - 构建
- `./apache-maven-3.9.14/bin/mvn.cmd spring-boot:run` - 启动 Spring Boot，约30秒后启动完成
- `./apache-maven-3.9.14/bin/mvn.cmd test` - 运行测试
- `./apache-maven-3.9.14/bin/mvn.cmd test -Dtest=TestClassName` - 运行单个测试

**注意**：
- Maven 安装在 `backend/apache-maven-3.9.14/` 目录，系统 PATH 中没有 `mvn` 命令
- 必须使用相对路径 `./apache-maven-3.9.14/bin/mvn.cmd` 调用
- 后端启动前需确保 MySQL 数据库连接正常（远程数据库 10.207.0.27:3306/szxqxxk）
- 数据库（10.207.0.27:3306/szxqxxk）是生产环境，做验证时如果产生了测试数据，验证后需要清理还原。
- 重启前务必停止 8080、5173 端口上已有的进程

## 小区爬虫

在 `backend/scraper` 目录下运行：

- `npm install` - 安装依赖
- `npm run build` - 构建 TypeScript
- `npm run start` - 有头模式启动
- `npm run start:headless` - 无头模式启动
- `npm run start -- --limit=100` - 限制抓取数量
- `npm run dev` - 开发模式（直接运行 TypeScript）
- `npm run test` - 验证反爬机制
- `npm run clean:cache` - 清理浏览器缓存

- 数据源：安居客网站 https://shenzhen.anjuke.com/community/
- 进度文件：`backend/scraper/data/progress.json`

## 预售爬虫

在 `backend/scraper` 目录下运行：

- `npm run presale` - 构建后运行
- `npm run presale:dev` - 开发模式

- 数据源：深圳市房地产信息平台 https://fdc.zjj.sz.gov.cn
- 进度文件：`backend/scraper/data/presale-progress.json`