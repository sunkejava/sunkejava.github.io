# Sunkejava's Blog

[![Deploy Hexo](https://github.com/sunkejava/sunkejava.github.io/actions/workflows/deploy.yml/badge.svg?branch=source)](https://github.com/sunkejava/sunkejava.github.io/actions/workflows/deploy.yml)

这是 [sunkejava.com](https://www.sunkejava.com) 的 Hexo 源码分支。博客用于记录 .NET/C#、Windows 自动化、USB/串口与硬件集成、嵌入式墨水屏、本地 AI 和项目工程化实践。

## 分支约定

- `source`：Hexo 源码、Markdown 文章和自动部署工作流。
- `master`：GitHub Pages 静态产物，由工作流生成，不直接编辑。

## 本地运行

需要 Node.js 20 或更高版本。

```bash
npm ci
npm run clean
npm run server
```

浏览器访问 `http://localhost:4000`。生产构建：

```bash
npm run build
```

提交前可运行 `npm run check`，它会清理缓存并完整生成站点，用于发现文章 Front Matter、主题配置或生成器错误。

## 发布文章

在 `source/_posts/` 新建 Markdown 文件，至少包含：

```yaml
---
title: 文章标题
date: 2026-08-27 20:00:00
categories:
  - .NET
tags:
  - CSharp
description: 用一句话说明文章解决什么问题。
---
```

提交到 `source` 分支后，GitHub Actions 会构建并发布到 `master`，同时保留自定义域名 `sunkejava.com`。

## 内容原则

1. 技术结论尽量给出适用条件、失败现象和验证方式。
2. 项目复盘强调问题、选择、权衡和结果，不写成仓库功能清单。
3. 不公开私有仓库、客户信息、密钥、设备地址和生产环境数据。
4. 公开项目文章链接回对应仓库，方便读者核对最新实现。
