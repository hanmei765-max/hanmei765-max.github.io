# 🚀 Hugo + Stack 博客搭建指南

> 一份让**完全不懂技术**的人也能快速搭建个人博客的实战教程  
> 适合：GitHub Pages + Hugo + Stack主题

---

## 📋 目录

1. [前置准备](#1-前置准备)
2. [GitHub仓库创建](#2-github仓库创建)
3. [本地Hugo环境配置](#3-本地hugo环境配置)
4. [创建项目与文档](#4-创建项目与文档)
5. [Stack主题导入与配置](#5-stack主题导入与配置)
6. [本地预览](#6-本地预览)
7. [部署到GitHub Pages](#7-部署到github-pages)
8. [常见问题](#8-常见问题)

---

## 1. 前置准备

### 需要安装的软件

| 软件     | 作用           | 下载地址             |
| -------- | -------------- | -------------------- |
| **Git**  | 版本控制工具   | https://git-scm.com/ |
| **Hugo** | 静态网站生成器 | https://gohugo.io/   |

### 本地Hugo环境配置：下载Hugo

**Windows用户**：

1. 访问 https://github.com/gohugoio/hugo/releases
2. 下载 `hugo_extended_*.exe`（完整版，支持SCSS）

**Mac用户**：

```bash
brew install hugo
```

**Linux用户**：

```bash
sudo apt-get install hugo
```

### 创建项目文件夹

在D盘（或任意盘）创建文件夹，例如：

```
D:/hugoblog
```

### 初始化Hugo项目

打开命令行，进入该文件夹：

```bash
cd D:/hugoblog
hugo new site mysite
```

> 如果提示"hugo"不是命令，请将Hugo安装路径添加到系统环境变量中

### 克隆你的GitHub仓库

```bash
git remote add origin 你的仓库地址
git checkout -b main
git push -u origin main
```

---

### 验证安装

打开**命令行/Terminal**，输入以下命令：

```bash

# 检查Git
git --version

# 检查Hugo
hugo version
```

如果显示版本号，说明安装成功✅

---

## 2. GitHub仓库创建

### 步骤2.1：登录GitHub
1. 访问 https://github.com
2. 点击 **Sign up** 注册账号（已有账号则登录）

### 步骤2.2：创建新仓库
1. 点击右上角 **+** → **New repository**
2. 填写信息：
   - **Repository name**: `你的用户名.github.io`（必须是这个格式）
   - **Public**: 勾选公开
   - 不要勾选"Add README"（会干扰后续步骤）
3. 点击 **Create repository**

### 步骤2.3：获取仓库地址
创建成功后，复制仓库地址（例如）：
```

https://github.com/ 你的用户名/你的用户名.github.io.git
```

---

---

## 3. 创建项目与文档

### 步骤3.1：下载Stack主题

Stack主题位于 `themes` 文件夹下：

```bash

cd D:/hugoblog
git clone https://github.com/CaiJimmy/hugo-theme-stack.git  themes/stack
```

### 步骤3.2：修改配置文件

创建或修改 `hugo.toml` 文件：

```toml
baseURL = "个人博客地址"
title = "嵌入式技术总结"
theme = "stack"

# 默认语言
defaultContentLanguage = "zh-cn"

[params]
  author = "hanmei765-max"
  description = "记录嵌入式Linux、驱动开发与RTOS学习笔记"

[permalinks]
  post = "/p/:slug/"
  page = "/:slug/"
```

### 步骤3.3：创建第一篇文档

```bash

# 创建文章
hugo new posts/你好世界.md
```

### 步骤3.4：编辑文章内容

打开 `content/posts/你好世界.md`，输入以下内容：

```markdown

---
title: "第一篇文章"
date: 2026-04-25
categories: ["类别：嵌入式"]
tags: ["标签：test"]
author: "hanmei765-max"
---

这是我写的第一篇技术博客文章！

## 为什么写博客？

1. 记录学习历程
2. 分享技术心得

## 下一步

```

---

## 4. 本地预览

### 启动本地服务器

在命令行输入：

```bash

hugo server -D --buildDrafts
```

- `-D`: 包含草稿文章
- `--buildDrafts`: 包含未发布文章

### 访问本地博客

浏览器打开：
```

http://localhost:1313 
```

> 如果看到Stack主题的博客界面，说明配置成功✅

---

## 7. 部署到GitHub Pages

### 步骤7.1：生成静态文件

```bash

hugo
```

生成完成后，在根目录查看 `public/` 文件夹。

### 步骤7.2：推送到GitHub

```bash

git add .
git commit -m "提交博客"
git push
```

### 步骤7.3：启用GitHub Pages

注意：GitHub Pages分支选择需和你代码实际分支关联

1. 进入仓库 → **Settings**
2. 左侧菜单 → **Pages**
3. **Source** 选择 `main` 分支 → **/root**
4. 点击 **Save**

### 步骤7.4：等待部署完成

1-2分钟后访问：
```

https://你的用户名.github.io
```

---

## 8. 常见问题

### Q1: `hugo: command not found`
**解决方法**：安装Hugo后，需要将安装路径添加到环境变量

### Q2: 主题样式不显示
**解决方法**：确保 `themes/stack` 文件夹存在且未损坏

### Q3: 部署后页面为空白
**解决方法**：
1. 检查 `hugo.toml` 中的 `baseURL` 是否与你仓库地址一致
2. 在仓库Settings→Pages中查看部署日志

### Q4: 如何发布新文章
**解决方法**：
```bash

hugo new posts/文章标题.md
# 编辑内容
# 重新生成和推送
hugo && git add . && git commit -m "更新" && git push
```

### Q5: 本地预览与部署不一致
**解决方法**：清理缓存重新生成
```bash

hugo server --minify
```

---

## 📁 项目结构说明

```

你的用户名.github.io/
├── content/           # 文章内容
│   └── posts/        # 文章文件夹
├── themes/           # 主题文件夹
│   └── stack/        # Stack主题
├── layouts/          # 页面模板（可选）
├── static/           # 静态资源（图片、CSS等）
├── hugo.toml         # 配置文件
└── public/           # 生成的静态文件（不要手动修改）
```

---

## 🚀 快速命令速查

| 操作         | 命令                       |
| ------------ | -------------------------- |
| 创建新文章   | `hugo new posts/文章名.md` |
| 本地预览     | `hugo server`              |
| 生成静态文件 | `hugo`                     |
| 推送到GitHub | `git push`                 |

---

## ✨ 最后一步

现在你已经成功搭建了一个个人技术博客！

接下来可以：
1. 📝 持续更新文章
2. 🎨 自定义主题样式
3. 🔧 添加插件和扩展功能

祝您运营顺利！🎉

---

> **提示**：遇到问题可以查看Stack主题官方文档：https://stack.jimmycai.com/

```