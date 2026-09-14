---
title: Git 与 GitHub 入门：用版本控制管理博客
description: 从什么是 Git 开始，讲解代码版本控制的基本概念，以及如何用 GitHub 托管博客代码、实现自动部署。
published: true
date: 2026-09-14T10:57:02.557Z
tags: git, github, 版本控制
editor: markdown
dateCreated: 2026-09-14T10:57:02.557Z
---

## TL;DR

Git 是代码"时光机"，GitHub 是它的云备份盘。用好这两样工具，你的博客代码不会丢，改错了也能一键回退，每次更新自动推送到线上。

## 为什么要学版本控制

没有版本控制时候，修改代码只能靠"备份文件夹"：`code-v1`、`code-v2-final`、`code-v3-really-final`。

这种方式问题：不知道每个版本改了什么，找不到特定时间文件，回退只能靠手动对比。

Git 解决了这一切。每一次保存（commit）都有记录，改了什么、什么时候改、为什么改，全都在案。

## 核心概念

### 仓库（Repository）

仓库是 Git 管理的项目文件夹。云图札记博客整个目录就是一个仓库。

### 提交（Commit）

一次 commit 就像游戏存档。每次你觉得"这个版本可以保存了"，就 commit 一次。

```bash
git add .
git commit -m "添加了关于页面"
```

commit 信息要用中文描述你做了什么，不是"修改"或"更新"。

### 分支（Branch）

分支是从主线分出去工作副本。默认分支叫 `main`（旧项目可能是 `master`）。

```bash
git branch new-feature   # 创建分支
git checkout new-feature # 切换到新分支
git checkout main        # 切回主线
```

写博客一般不需要手动操作分支，直接在 main 上工作就好。

### 推送与拉取（Push / Pull）

```bash
git push    # 把本地提交推送到远程
git pull    # 把远程更新拉取到本地
```

## 开始使用 GitHub

### 注册账号

去 [github.com](https://github.com) 注册，选择免费计划（Free）。

### 创建仓库

1. 点击右上角 **+** → **New repository**
2. Repository name 填 `my-blog`（或任意名字）
3. 选择 **Private**（博客代码通常选私有）或 **Public**
4. 不要勾选 "Add a README"（已有项目）
5. 点击 **Create repository**

创建完成后，GitHub 会给你几个命令，把本地仓库和远程连接：

```bash
git remote add origin https://github.com/你的用户名/my-blog.git
git branch -M main
git push -u origin main
```

## 日常使用流程

写完文章后，在博客项目目录下运行：

```bash
# 1. 查看当前改动
git status

# 2. 添加所有改动到暂存区
git add .

# 3. 创建提交（写清楚做了什么）
git commit -m "新文章：Git 与 GitHub 入门"

# 4. 推送到 GitHub
git push
```

## 回退操作

### 改错了，想撤销上一次提交？

```bash
# 撤销上一次 commit，但保留改动
git reset --soft HEAD~1

# 撤销上一次 commit，并取消暂存
git reset --soft HEAD~1  # 先撤 commit
git reset HEAD .          # 再撤 add
```

### 想查看某次 commit 改了哪些内容？

```bash
git log                  # 查看提交历史
git show abc1234         # 查看具体某次提交的内容
```

### 想完全回到某个历史版本？

```bash
git checkout abc1234     # 临时查看（进入 detached HEAD 状态）
git checkout main        # 切回主线
git revert abc1234       # 生成一个新提交，撤销指定 commit 的改动
```

## 与 Cloudflare Pages 配合

Cloudflare Pages 绑定 GitHub 仓库后，每次 `git push` 都会自动触发一次构建部署。

流程是这样：

```bash
# 本地写文章、改样式
→ git add . && git commit -m "新文章"
→ git push
→ GitHub 收到推送
→ 通知 Cloudflare Pages
→ Cloudflare Pages 执行 npm run build
→ 新网站部署上线
```

整个过程无需手动操作，1-2 分钟完成。

## 常见问题

### 推送时要求输入用户名和密码？

那是 HTTPS 方式，每次都要验证。改用 SSH 方式一劳永逸：

```bash
git remote set-url origin git@github.com:你的用户名/my-blog.git
```

前提是你已经配置了 SSH 密钥（GitHub Settings → SSH Keys）。

### commit 信息写错了怎么办？

```bash
git commit --amend
```

这会打开编辑器让你修改最新 commit 信息。注意：这是修改了 commit，不要在已推送 commit 上使用。

### 多人协作时 push 冲突怎么办？

用 `git pull --rebase` 拉取远程最新代码，解决冲突后再 push。具体场景比较复杂，建议从单人使用开始，逐步熟悉。

## 写在

Git 的学习曲线不陡，基本的 add / commit / push 三板斧就够用。随着需求增长再学习分支、回退、协作等高级功能。

不要怕折腾代码—Git 永远能帮你找回任何一个版本。

## 延伸阅读

- [Git 官方文档](https://git-scm.com/book/zh/v2)
- [GitHub 官方入门指南](https://guides.github.com/activities/hello-world/)
- [从零搭建 Astro 博客并部署到 Cloudflare Pages](/blog/astro-blog-from-scratch)
