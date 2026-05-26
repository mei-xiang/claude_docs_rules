# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

深圳小区信息查询系统：抓取安居客二手房小区数据和深圳住建局预售房源数据，提供 Vue 前端展示界面。

数据源：安居客小区 | 深圳住建局预售平台

## Language

使用中文回答用户问题。

## Rules

规则文件位于 `.claude/rules/`，按需加载：

- `ai-behavior.md` - AI 行为准则（核心原则、操作流程）
- `code-review.md` - 代码审查规范
- `testing-strategy.md` - 测试策略
- `git-conventions.md` - Git 规范
- `task-tracking.md` - 任务跟踪流程
- `error-handling.md` - 错误处理规范
- `security.md` - 安全规范

## Docs

文档文件位于 `.claude/docs/`，按需参考：

- `commands.md` - 命令汇总（前端/后端/爬虫）
- `architecture.md` - 系统架构
- `scraper-strategy.md` - 爬虫策略与反爬机制

## Quick Start

- 前端: `cd frontend && npm run dev`
- 后端: `cd backend && ./apache-maven-3.9.14/bin/mvn.cmd spring-boot:run`
- 爬虫: `cd backend/scraper && npm run dev`