---
title: "Hugo + Stack 博客搭建指南"
date: 2026-04-25
categories: ["平台搭建"]
tags: ["标签","GitHubPages", "Hugo", "静态网站"]
author: "hanmei765-max"
---

# Hugo + Stack 博客搭建完整指南

> GitHub Pages + Hugo + Stack 主题部署方案  
> 涵盖：环境配置、项目结构、docs 目录部署、自动部署策略



## 📋 目录

1. [前置准备与环境配置](#1-前置准备与环境配置)
2. [GitHub 仓库创建](#2-github-仓库创建)
3. [本地 Hugo 环境配置](#3-本地-hugo-环境配置)
4. [项目结构与主题配置](#4-项目结构与主题配置)
5. [内容创作与本地预览](#5-内容创作与本地预览)
6. [GitHub Pages 部署方案](#6-github-pages-部署方案)
7. [自动部署流程分析](#7-自动部署流程分析)
8. [常见问题与解决方案](#8-常见问题与解决方案)
9. [更新发布流程](#9-更新发布流程)



## 1. 前置准备与环境配置

### 1.1 软件安装

| 软件     | 说明           | 下载链接             |
| -------- | -------------- | -------------------- |
| **Git**  | 版本控制工具   | https://git-scm.com/ |
| **Hugo** | 静态站点生成器 | https://gohugo.io/   |

### 1.2 Hugo 环境变量配置（Windows）

1. 下载 Hugo 解压后，将 `hugo.exe` 所在目录添加至系统环境变量
2. 打开 **控制面板 → 系统 → 高级系统设置 → 环境变量**
3. 在 **Path** 中添加 Hugo 安装路径（如：`C:\Program Files\Hugo\`）
4. 重启命令行，验证：

```bash
hugo version
```

### 1.3 验证安装

```bash
git --version
hugo version
```

> 显示版本号即表示安装成功。



## 2. GitHub 仓库创建

### 2.1 创建规则

| 配置项   | 要求               | 说明           |
| -------- | ------------------ | -------------- |
| 仓库命名 | `用户名.github.io` | 必须严格匹配   |
| 可见性   | Public             | 仅支持公共仓库 |
| 初始化   | 不勾选 README      | 手动初始化项目 |

### 2.2 仓库地址

```
https://github.com/ 用户名/用户名.github.io.git
```



## 3. 本地 Hugo 环境配置

### 3.1 创建项目

```bash
hugo new site myblog
cd myblog
```

### 3.2 安装 Stack 主题

```bash
git clone https://github.com/CaiJimmy/hugo-theme-stack.git   themes/stack
```

## 4. 项目结构与主题配置

### 4.1 标准项目结构

```
myblog/
├── content/              # 内容文件
│   ├── posts/           # 文章目录
│   └── pages/           # 页面文件
├── data/                # 数据文件
├── layouts/             # 模板文件
├── static/              # 静态资源
│   ├── css/            # CSS 样式
│   ├── js/             # JavaScript
│   └── images/         # 图片资源
├── themes/              # 主题目录
│   └── stack/          # Stack 主题
├── config/              # 配置文件
├── docs/                # 生成目录（部署目标）
├── hugo.toml           # 主配置文件
└── public/             # 可选的 public 目录
```

### 4.2 修改 `hugo.toml`

```toml
baseURL = "https://用户名.github.io/"
publishDir = "docs"
title = "个人技术博客"
theme = "stack"

defaultContentLanguage = "zh-cn"
languageCode = "zh-cn"

[params]
  author = "作者名称"
  description = "技术文章、学习笔记与项目总结"
  favicon = "/favicon.ico"

[permalinks]
  post = "/p/:slug/"
  page = "/:slug/"

[services]
  [services.disqus]
    enable = false
```

## 5. 内容创作与本地预览

### 5.1 创建文章

```bash
hugo new posts/文章标题.md
```

### 5.2 本地预览

```bash
hugo server -D --minify
```

访问：`http://localhost:1313`

> `-D` 显示草稿文章，`--minify` 优化输出



## 6. GitHub Pages 部署方案

### 6.1 生成静态文件

```bash
hugo
```

> 执行后根目录将生成 `docs` 文件夹，即为待部署网站。

### 6.2 提交至 GitHub

```bash
git add .
git commit -m "初始化博客"
git push origin main
```

### 6.3 GitHub Pages 配置

备注：如果是平台自动部署Branch->main Folder->root 如果手动部署Branch->自己代码分支 Folder->docs(docs需配置toml文件 下面有)

进入仓库 → **Settings** → **Pages**

| 配置项 | 值                   |
| ------ | -------------------- |
| Source | Deploy from a branch |
| Branch | main                 |
| Folder | /docs                |

点击 **Save** 后等待约 1 分钟即可访问。


## 7. 自动部署流程分析

### 7.1 自动部署（GitHub Actions）

#### 工作原理

```
.github/
└── workflows/
    └── deploy.yml      # 自动部署工作流
```

当代码推送至仓库后，GitHub Actions 自动触发部署流程。

#### 不推荐的原因

| 问题           | 说明                          |
| -------------- | ----------------------------- |
| **配置复杂**   | 需要理解 YAML 工作流语法      |
| **依赖网络**   | 部署时网络波动会导致失败      |
| **调试困难**   | 出错后需查看 Actions 日志     |
| **与手动冲突** | 手动部署 + 自动部署易产生冲突 |
| **版本混乱**   | 多路部署导致内容不一致        |

### 7.2 推荐方案：手动部署

| 优势     | 说明               |
| -------- | ------------------ |
| 可控性强 | 部署前可验证结果   |
| 调试简单 | 本地先验证，再推送 |
| 无需配置 | 无额外工作流文件   |
| 内容一致 | 单一路径部署       |

### 7.3 如需保留自动部署（可选）

#### 7.3.1 配置 `deploy.yml`

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "latest"
      - name: Build
        run: hugo --minify
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

#### 7.3.2 配置说明

| 配置项        | 值                    |
| ------------- | --------------------- |
| `publish_dir` | `./public`（非 docs） |
| `theme`       | 保持一致              |



## 8. 常见问题与解决方案

### 8.1 docs 目录未生成

**错误**：`No such file or directory - docs`

**原因**：未执行 `hugo` 命令

**解决**：
```bash
hugo
git add .
git commit -m "生成网站文件"
git push
```

### 8.2 网站显示旧内容

**原因**：
- 自动部署未关闭
- 浏览器缓存

**解决**：
1. 确认已删除或禁用 Actions 工作流
2. 强制刷新：`Ctrl + Shift + R`

### 8.3 public 目录处理

**说明**：配置 `publishDir = "docs"` 后，`public` 目录可安全删除或保留（不影响部署）。

### 8.4 样式丢失或页面空白

**原因**：`baseURL` 配置错误

**修正**：
```toml
baseURL = "https://用户名.github.io/"
```

### 8.5 环境变量未生效

**解决**：
```bash
# Windows：重新打开命令行
# macOS/Linux：source -/.bashrc 或重启终端
```



## 9. 更新发布流程

每次更新文章，执行以下三步：

| 步骤        | 命令                                            | 说明               |
| ----------- | ----------------------------------------------- | ------------------ |
| 1. 创建文章 | `hugo new posts/标题.md`                        | 新建 Markdown 文件 |
| 2. 生成网站 | `hugo`                                          | 更新 docs 目录     |
| 3. 提交推送 | `git add . && git commit -m "更新" && git push` | 推送至 GitHub      |

> 约 1 分钟后网站自动更新


## 📋 常用命令速查

| 操作         | 命令                                            | 说明           |
| ------------ | ----------------------------------------------- | -------------- |
| 创建文章     | `hugo new posts/标题.md`                        | 新建文章       |
| 本地预览     | `hugo server -D --minify`                       | 本地开发预览   |
| 生成静态文件 | `hugo`                                          | 生成 docs 目录 |
| 推送更新     | `git add . && git commit -m "更新" && git push` | 推送至 GitHub  |
| 撤销更改     | `git reset --hard HEAD`                         | 本地回退       |


## ⚠️ 关键要点总结

1. **必须执行 `hugo` 命令生成 `docs` 目录**
2. **GitHub Pages 配置为：分支 + /docs**
3. **手动部署更推荐：避免自动部署冲突**
4. **本地预览验证后再推送：降低出错率**


> 本文档版本：1.1.0  
> 最后更新：2026-04-26

