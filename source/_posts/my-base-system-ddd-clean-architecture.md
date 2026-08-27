---
title: MyBaseSystem：用 DDD 与整洁架构构建可复用的 .NET 10 基础系统
date: 2026-08-27 20:30:00
categories:
  - .NET
tags:
  - DotNet10
  - DDD
  - CleanArchitecture
  - Vue3
description: 说明通用后台为什么仍需要清晰边界，以及 MyBaseSystem 如何组织前后端目录、权限和模块扩展。
---

[MyBaseSystem](https://github.com/sunkejava/MyBaseSystem) 的目标，是把登录、用户、角色、权限、菜单、审计和基础配置等重复工作沉淀为长期复用的平台。前端基于 Vue 3 Monorepo，后端使用 .NET 10、DDD 与 Clean Architecture。

<!-- more -->

## 通用系统最容易变成“大杂烩”

基础系统功能多、关联广，很容易让控制器直接访问数据库，让权限判断散落在页面和接口里，最终每次复用都需要大幅裁剪。真正的复用不是拥有更多公共方法，而是拥有稳定的边界。

后端依赖方向始终指向内层：

```text
Api -> Application -> Domain
Infrastructure -> Application / Domain
```

Domain 不依赖 EF Core 或 ASP.NET Core；Application 编排用例并定义端口；Infrastructure 实现存储、JWT 和外部服务；Api 只负责 HTTP 映射、认证与统一响应。

## 前后端目录必须明确分离

所有前端代码放在 `frontend/`，所有 .NET 解决方案、源码和测试放在 `backend/`。这不是单纯为了目录美观，而是为了让工具链、依赖缓存、容器构建和 CI 任务拥有清晰作用域。

前端业务应用与通用组件继续分层；后端则按照领域、应用、基础设施和 API 分层。一个新业务模块应作为限界上下文接入，而不是继续向 SystemService 中堆方法。

## 权限是服务端契约

菜单用于导航，权限码用于授权，两者不能混为一谈。权限采用 `领域:资源:动作` 的形式，例如 `system:user:update`。前端可以据此隐藏按钮，但服务端仍必须执行最终授权。

用户获得角色后，菜单树应从权限结果动态生成；角色维护则同时展示菜单名称和代码，避免配置人员面对无法理解的标识符。

## 开箱即用与生产安全之间的平衡

SQLite 和管理员种子数据让项目可以快速启动，但生产环境必须更换 JWT 密钥和初始密码，并按目标数据库增加 Infrastructure Provider。领域层和应用层不应因为数据库从 SQLite 切换到 PostgreSQL 或 SQL Server 而改变。

基础系统的价值不在于功能列表有多长，而在于新项目接入后能否继续保持边界清楚、行为可测试、升级可控。

