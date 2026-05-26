# 系统架构

## Tech Stack

- **Frontend**: Vue 3 (Composition API + `<script setup>`), TypeScript, Vite, Element Plus, Vue Router, Axios
- **Backend**: Spring Boot 2.7.18, Spring Data JPA, MySQL 8 (远程数据库 10.207.0.27:3306/szxqxxk)
- **Scraper**: Node.js + TypeScript + Playwright（小区爬虫）/ axios（预售爬虫）

## 数据流

```
┌─────────────────┐     ┌──────────────────┐
│   安居客网站      │────▶│  小区爬虫         │
│ anjuke.com      │     │ Playwright       │
└─────────────────┘     └──────────┬───────┘
                                   │
┌─────────────────┐     ┌──────────┴───────┐
│ 住建局预售平台     │────▶│  预售爬虫         │
│ fdc.zjj.sz.gov  │     │ axios            │
└─────────────────┘     └──────────┬───────┘
                                   │
                                   ▼
                           ┌─────────────┐
                           │ Spring Boot │
                           │   API       │
                           └──────┬──────┘
                                  │
                                  ▼
                           ┌─────────────┐
                           │   MySQL     │
                           └──────┬──────┘
                                  │
                                  ▼
                           ┌─────────────┐
                           │ Spring Boot │
                           │   API       │
                           └──────┬──────┘
                                  │
                                  ▼
                           ┌─────────────┐
                           │  Vue 前端   │
                           └─────────────┘
```

**写入路径**：爬虫通过 HTTP 调用 Spring Boot API 写入 MySQL（不直接连接数据库）
**读取路径**：Vue 前端通过 HTTP 调用 Spring Boot API 读取 MySQL 数据

## 爬虫架构概览

### 小区爬虫（Playwright）

- **技术**：Node.js + TypeScript + Playwright
- **挑战**：安居客有强反爬机制（验证码、登录要求）
- **策略**：模拟真实浏览器行为 + Cookie 持久化 + 用户手动处理验证码
- **输出**：小区基本信息、价格历史、小区解读、图片

详见 `.claude/docs/scraper-strategy.md`

### 预售爬虫（axios）

- **技术**：Node.js + TypeScript + axios（无浏览器）
- **优势**：政府公开 API，无反爬限制
- **策略**：直接调用 REST API，支持断点续传（基于 passdate 过滤）
- **输出**：项目、楼栋、单元、房间信息及状态变更历史

详见 `.claude/docs/scraper-strategy.md`

## Project Structure

```
project-root/
├── frontend/                    # Vue 3 前端
│   ├── src/
│   │   ├── main.ts              # 入口
│   │   ├── App.vue              # 根组件
│   │   ├── api/                 # API 服务层
│   │   ├── router/              # Vue Router
│   │   ├── components/          # 组件
│   │   └── style.css            # 样式
│   └── vite.config.ts           # Vite 配置
├── backend/                     # Spring Boot 后端
│   ├── src/main/java/.../
│   │   ├── controller/          # 控制器
│   │   ├── service/             # 服务
│   │   ├── entity/              # 实体
│   │   └── repository/          # Repository
│   └── src/main/resources/
│       ├── application.yml      # 配置
│       └── db/                  # 数据库脚本
└── backend/scraper/             # TypeScript 爬虫
    ├── src/
    │   ├── index.ts             # 小区爬虫入口
    │   ├── presale/             # 预售爬虫模块
    │   └── ...                  # 其他模块
    └── data/                    # 进度文件
```

## Database Tables (szxqxxk)

**小区系统**：
- `community` - 小区基本信息
- `community_price_history` - 月度价格记录
- `community_interpretation` - 小区解读
- `community_image` - 小区图片
- `community_progress` - 开发进度

**预售系统**：
- `presale_project` - 预售项目
- `presale_building` - 楼栋信息
- `presale_unit` - 单元信息
- `presale_unit_floor` - 单元楼层
- `presale_room` - 房间信息
- `presale_room_status_mapping` - 房间状态映射
- `presale_room_status_history` - 房间状态历史记录
- `presale_cert_info` - 证书信息

完整字段定义见 `backend/src/main/resources/db/init.sql` 和 `presale-init.sql`

## 功能架构

### 小区系统 (/)

小区浏览与管理，支持：
- 小区列表查询（分页、关键词搜索、区域筛选）
- 小区详情展示（基本信息、价格趋势、解读、图片）
- 价格历史查询与趋势分析

### 预售房源 (/presale)

预售房源展示，支持：
- 项目树结构浏览（区→项目→楼栋→单元）
- 楼栋详情与统计信息
- 房间状态查看与历史追踪

### 系统管理 (/admin)

爬虫进程管理，支持：
- 启动/停止小区爬虫和预售爬虫
- 实时查看爬虫日志
- 数据库备份
