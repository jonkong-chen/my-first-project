# Git & GitHub Learning Notes — Day 02

> 学习日期：2026-10-18  
> 学习主题：Git Branch、Merge、Pull Request、Code Review、远程同步与 Git 日志  
> 学习设备：MacBook Pro  
> 实践仓库：my-first-project

---

# 目录

1. [今日学习目标](#1-今日学习目标)
2. [Git Branch（分支）](#2-git-branch分支)
3. [Git Merge（分支合并）](#3-git-merge分支合并)
4. [Pull Request（拉取请求）](#4-pull-request拉取请求)
5. [Code Review（代码审查）](#5-code-review代码审查)
6. [Git 远程同步](#6-git-远程同步)
7. [Git Reflog（操作历史）](#7-git-reflog操作历史)
8. [GitHub Desktop 使用](#8-github-desktop-使用)
9. [完整开发流程](#9-完整开发流程)
10. [今日实际问题与解决方法](#10-今日实际问题与解决方法)
11. [Git 命令速查表](#11-git-命令速查表)
12. [常见误区](#12-常见误区)
13. [学习总结](#13-学习总结)

---



# 1. 今日学习目标

掌握 Git 的基础分支管理与 GitHub 协作流程。

主要内容：

- 创建和切换分支
- 理解不同分支之间的关系
- 在新分支中独立开发
- 将开发分支合并回 main
- 理解 Fast-forward Merge
- 创建 Pull Request
- 查看代码差异
- 完成 Code Review
- 通过 GitHub 合并 PR
- 同步本地与远程仓库
- 利用 Git 日志排查问题

核心工作流程：

```text
创建 Branch
    ↓
修改文件
    ↓
git add
    ↓
git commit
    ↓
git push
    ↓
Pull Request
    ↓
Code Review
    ↓
Merge
    ↓
更新本地 main
```

---



# 2. Git Branch（分支）



## 2.1 什么是 Branch？

Branch（分支）是 Git 中用于组织不同开发历史的机制。

开发新功能时，可以创建独立分支，而不是直接修改 main。

例如：

```text
main
 │
 ├───────────────┐
 │               │
 │          feature/login
 │               │
 │          开发登录功能
 │               │
 │          测试与修改
 │               │
 └───────────────┘
         Merge
```

主要作用：

1. 隔离不同任务的开发历史。
2. 避免未经验证的新提交直接进入 main。
3. 方便多人并行开发。
4. 方便进行代码审查和合并。

注意：

创建 Branch 不会自动修复代码错误，也不是创建一个完全独立的仓库。

## 2.2 查看分支

```bash
git branch
```

示例结果：

```text
  feature/login
* main
```

其中：

`*` 表示当前所在分支。

## 2.3 创建并切换分支

```bash
git switch -c feature/login
```

命令解释：


| 命令部分          | 含义       |
| ------------- | -------- |
| git           | Git 工具   |
| switch        | 切换分支     |
| -c            | 创建新分支并切换 |
| feature/login | 新分支名称    |




## 2.4 切换已有分支

```bash
git switch main
```

作用：

切换回 main 主分支。

注意：

切换分支时，Git 通常会根据目标分支的版本更新工作区文件。

如果存在未提交修改，Git 可能阻止切换，以避免覆盖这些修改。

## 2.5 不同分支之间的关系

假设当前历史为：

```text
A --- B --- C  ← main
```

从 C 创建新分支，并分别产生新提交：

```text
          D --- E  ← main
         /
A --- B --- C
         \
          F --- G  ← feature/login
```

说明：

- A、B、C 是两个分支共享的历史。
- D、E 属于 main 后续开发历史。
- F、G 属于 feature/login 后续开发历史。

重要结论：

**分支可以共享历史，但能够独立推进新的 Commit。**

---



# 3. Git Merge（分支合并）



## 3.1 什么是 Merge？

Merge（合并）用于将其他分支的修改整合进当前分支。

例如：

已经完成 feature/login 的开发，希望把它合并到 main。

先切换到 main：

```bash
git switch main
```

再执行：

```bash
git merge feature/login
```

这表示：

将 feature/login 的修改合并到当前的 main。

重要：

**Merge 的方向取决于当前所在的分支。**

## 3.2 Fast-forward Merge（快进合并）

假设：

```text
A --- B --- C  ← main
             \
              D  ← feature/login
```

如果 main 自分支创建后没有新的提交，合并可能采用 Fast-forward。

```bash
git merge feature/login
```

合并后：

```text
A --- B --- C --- D
                  ↑
                 main
                  ↑
             feature/login
```

Fast-forward Merge 的本质：

**将目标分支指针向前移动到已有提交，不需要额外创建合并提交。**

常见终端提示：

```text
Fast-forward
```

注意：

Fast-forward 不代表删除历史，也不代表自动解决了所有类型的代码冲突。

## 3.3 Merge Conflict（合并冲突）

如果两个分支对同一部分内容进行了不兼容的修改，合并时可能发生冲突。

例如：

```text
main:
用户登录方式：密码

feature/login:
用户登录方式：验证码
```

Git 无法自动决定保留哪个版本时，需要人工解决冲突。

今天尚未实际练习冲突解决。

后续学习内容：

- 如何识别冲突
- 如何编辑冲突文件
- 如何完成冲突解决
- 如何验证合并结果

---



# 4. Pull Request（拉取请求）



## 4.1 什么是 Pull Request？

Pull Request，简称 PR。

它是 GitHub 提供的协作机制，用于请求将一个分支的修改合并到另一个分支。

例如：

```text
feature/login
      │
      │ Pull Request
      ▼
     main
```

PR 可以用来：

1. 展示代码差异。
2. 讨论修改方案。
3. 请求其他开发者审查。
4. 运行自动化检查。
5. 在满足要求后执行合并。



## 4.2 创建 PR 的流程

第一步：创建开发分支。

```bash
git switch -c feature/login
```

第二步：修改文件。

第三步：暂存修改。

```bash
git add 文件名
```

第四步：创建 Commit。

```bash
git commit -m "Add login feature"
```

第五步：发布分支。

```bash
git push -u origin feature/login
```

第六步：

打开 GitHub 仓库。

点击：

```text
Compare & pull request
```

第七步：选择分支。

```text
base: main

compare: feature/login
```

其中：

- base：目标分支
- compare：来源分支

第八步：

填写 PR 标题和说明。

第九步：

点击 Create pull request。

## 4.3 PR 的基本结构

一个 PR 通常包括：


| 页面            | 作用      |
| ------------- | ------- |
| Conversation  | 讨论与反馈   |
| Commits       | 查看提交记录  |
| Checks        | 查看自动化检查 |
| Files changed | 查看修改内容  |




## 4.4 PR 状态

常见状态：

- Open：PR 仍处于开放状态。
- Merged：PR 已经合并。
- Closed：PR 已关闭，但不一定已合并。

注意：

**创建 PR 不等于已经完成 Merge。**

---



# 5. Code Review（代码审查）



## 5.1 什么是 Code Review？

Code Review 是在代码合并之前，对修改内容进行检查的过程。

主要检查四个方面。

### Correctness（正确性）

检查代码是否能够正确实现需求。

例如：

- 是否存在逻辑错误
- 是否满足功能需求
- 是否存在明显 Bug



### Scope（修改范围）

检查修改是否只涉及本次任务需要的内容。

例如：

开发登录功能时，不应无故修改支付系统代码。

### Maintainability（可维护性）

检查代码是否容易理解、修改和维护。

例如：

- 命名是否清晰
- 是否存在不必要的重复
- 结构是否合理



### Security（安全性）

检查是否存在安全风险。

例如：

- API Key 泄露
- 密码泄露
- 不安全的数据处理



## 5.2 Able to merge 是什么意思？

GitHub 可能显示：

```text
Able to merge
```

含义：

GitHub 当前没有检测到阻止自动合并的冲突。

但这不代表：

- 代码一定正确
- 功能测试通过
- 安全审查通过
- 符合项目需求

因此：

**Able to merge ≠ Code Review Passed**

## 5.3 Checks 0 是什么意思？

如果显示：

```text
Checks 0
```

通常表示当前 PR 没有显示自动化检查。

不能理解为已经通过全部测试。

---



# 6. Git 远程同步



## 6.1 Fetch、Pull、Push 的区别


| 命令        | 作用          |
| --------- | ----------- |
| git fetch | 获取远程提交和引用信息 |
| git pull  | 获取并整合远程更新   |
| git push  | 将本地提交推送到远程  |




### Fetch

```bash
git fetch origin
```

作用：

获取远程仓库的最新信息。

通常不会直接修改当前本地分支的提交历史。

### Pull

```bash
git pull
```

作用：

获取远程更新，并整合到当前本地分支。

如果当前分支配置了上游分支，通常可以直接使用。

### Push

```bash
git push
```

作用：

将本地提交推送到对应远程仓库。

## 6.2 第一次发布新分支

```bash
git push -u origin feature/login
```

解释：


| 部分            | 含义       |
| ------------- | -------- |
| push          | 推送提交     |
| -u            | 设置上游跟踪关系 |
| origin        | 远程仓库名称   |
| feature/login | 要推送的分支   |


origin 通常是克隆仓库时自动创建的远程仓库名称。

设置上游关系后，以后通常可以直接使用：

```bash
git push
```



## 6.3 如何更新本地 main？

先切换到 main：

```bash
git switch main
```

检查状态：

```bash
git status
```

确认没有需要处理的未提交修改后：

```bash
git pull
```

再次检查：

```bash
git status
```

如果同步成功，通常显示：

```text
On branch main

Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

注意：

这条信息是根据本地保存的远程跟踪引用进行比较的。

如果没有先获取最新远程信息，它不一定代表 GitHub 服务器此刻没有更新。

---



# 7. Git Reflog（操作历史）



## 7.1 什么是 Reflog？

Reflog 用于记录本地 Git 引用的变动历史。

例如：

- 分支指针发生移动
- Commit
- Merge
- Pull
- 某些分支切换产生的 HEAD 变化

Reflog 常用于排查 Git 历史问题，以及寻找之前的提交位置。

## 7.2 查看 main 的历史变动

```bash
git reflog main -10
```

作用：

查看本地 main 最近10条引用变动记录。

## 7.3 查看带时间的历史

```bash
git reflog --date=iso main -10
```

作用：

显示 main 最近10条引用变动及其具体时间。

示例：

```text
9bfe418 main@{2026-10-18 21:42:35 +0800}: pull: fast-forward
```

含义：

- 9bfe418：提交编号缩写
- main：被记录的本地分支
- 2026-10-18 21:42:35：引用更新的时间
- pull: fast-forward：通过 Pull 快进更新分支



## 7.4 Reflog 与 Log 的区别



### git log

```bash
git log --oneline -5
```

查看当前分支可达的最近5个提交。

### git reflog

```bash
git reflog -5
```

查看 HEAD 最近的本地引用变动记录。

区别：

**Log 主要查看提交历史。**

**Reflog 主要追踪本地引用如何移动。**

## 7.5 Reflog 的限制

Reflog 可以帮助判断：

- 哪个分支发生了更新
- 更新后的提交编号
- 引用更新发生的时间
- 某些操作的类型

但通常不能直接确定：

- 是哪个软件执行的 Git 命令
- 是哪个界面按钮触发的
- 用户是否亲自输入了命令

需要结合软件日志进一步判断。

---



# 8. GitHub Desktop 使用

GitHub Desktop 是 GitHub 官方提供的图形界面客户端。

它可以帮助完成：

- 切换分支
- 查看修改
- 创建 Commit
- 发布分支
- Fetch
- Pull
- Push
- 查看历史记录



## 8.1 常见按钮


| 按钮             | 作用        |
| -------------- | --------- |
| Current Branch | 查看或切换当前分支 |
| Publish branch | 发布本地新分支   |
| Push origin    | 推送本地提交    |
| Fetch origin   | 获取远程更新信息  |
| Pull origin    | 获取并整合远程更新 |




## 8.2 查看 GitHub Desktop 日志

Mac 操作步骤：

1. 打开 GitHub Desktop。
2. 点击顶部 Help。
3. 选择 Show Logs in Finder。
4. 打开对应日期的日志文件。
5. 搜索操作时间或 Git 命令。

日志通常包含：

```text
Executing fetch
Executing pull
Executing push
```

日志时间可能采用 UTC。

中国标准时间为 UTC+8。

例如：

```text
2026-10-18T13:42:33Z
```

对应北京时间：

```text
2026-10-18 21:42:33
```

---



# 9. 完整开发流程

以下是以后开发 AI 项目时可以参考的简化工作流程。

## 9.1 开发新功能前

进入仓库：

```bash
cd ~/Documents/my-first-project
```

检查状态：

```bash
git status
```

切换到主分支：

```bash
git switch main
```

获取最新版本：

```bash
git pull
```

注意：

执行 Pull 前，应确认本地没有需要另外处理的未提交修改。

## 9.2 创建功能分支

```bash
git switch -c feature/new-feature
```

检查：

```bash
git branch
```



## 9.3 修改文件

在 Cursor 中编辑项目文件。

保存：

```text
Command + S
```

检查：

```bash
git status
```



## 9.4 暂存修改

```bash
git add 文件名
```

检查：

```bash
git status
```

确认需要提交的修改已进入暂存区。

## 9.5 创建 Commit

```bash
git commit -m "Add new feature"
```

检查：

```bash
git status
```



## 9.6 发布分支

```bash
git push -u origin feature/new-feature
```

如果已经设置上游：

```bash
git push
```



## 9.7 创建 PR

在 GitHub 网页：

1. 打开仓库。
2. 选择 Compare & pull request。
3. 确认 base 为 main。
4. 确认 compare 为开发分支。
5. 查看 Files changed。
6. 填写标题与说明。
7. 创建 Pull Request。



## 9.8 审查并合并

检查：

- Correctness
- Scope
- Maintainability
- Security

确认符合要求后：

```text
Merge pull request
    ↓
Confirm merge
```

实际团队项目还需遵守相应的审核和测试要求。

## 9.9 更新本地仓库

```bash
git switch main
```

```bash
git pull
```

```bash
git status
```

检查项目文件：

```bash
ls
```

完成。

---



# 10. 今日实际问题与解决方法



## 问题一：为什么切换分支后文件内容消失？

现象：

在 feature/branch-practice 中修改 README.md 并 Commit。

切换到 main 后，新增内容消失。

原因：

main 尚未包含开发分支的新提交。

解决方式：

通过 Merge 或 Pull Request 将修改合并到 main。

结论：

**文件内容变化是分支切换带来的正常现象，不代表提交丢失。**

---



## 问题二：为什么 Merge 显示 Fast-forward？

原因：

当前分支没有产生需要另外合并的分叉历史，可以直接将分支指针向前移动。

示例：

```text
A --- B --- C  ← main
             \
              D  ← feature
```

合并后：

```text
A --- B --- C --- D  ← main
```

---



## 问题三：为什么没有看到 Pull origin？

实际情况：

在 GitHub 网页合并 PR #2 后，点击了 GitHub Desktop 的 Fetch origin。

随后发现本地 main 已经包含最新文件，但没有看到需要执行 Pull origin 的提示。

通过 Git Reflog 发现：

```text
2026-10-18 21:42:35
pull: fast-forward
```

说明本地 main 确实执行过 Pull。

进一步查看 GitHub Desktop 日志：

```text
2026-10-18T13:42:33.341Z
Executing fetch
```

对应北京时间：

```text
2026-10-18 21:42:33
```

两个操作时间非常接近。

但提供的 GitHub Desktop 日志中，没有显示21:42对应的 Executing pull 记录。

因此：

**可以确认本地发生过 Pull，但不能仅凭现有日志确定具体由哪个软件触发。**

注意：

普通 Fetch 不等于 Pull，也不能简单认为点击 Fetch 一定会自动更新本地 main。

---



## 问题四：如何确认同步是否真正完成？

检查当前分支：

```bash
git branch
```

确认位于 main。

查看状态：

```bash
git status
```

查看文件：

```bash
ls
```

必要时查看提交编号：

```bash
git log --oneline -5
```

如果需要调查过去发生的更新：

```bash
git reflog --date=iso main -10
```

重要：

**不能仅凭 working tree clean 判断本地已经获取所有远程更新。**

---



# 11. Git 命令速查表


| 命令                               | 作用                    |
| -------------------------------- | --------------------- |
| `git --version`                  | 查看 Git 版本             |
| `pwd`                            | 查看当前路径                |
| `ls`                             | 查看文件                  |
| `cd 路径`                          | 切换目录                  |
| `git clone 地址`                   | 克隆远程仓库                |
| `git status`                     | 查看仓库状态                |
| `git branch`                     | 查看本地分支                |
| `git switch main`                | 切换到 main              |
| `git switch -c 分支名`              | 创建并切换分支               |
| `git add 文件名`                    | 暂存指定文件                |
| `git add .`                      | 暂存当前目录范围内的修改          |
| `git commit -m "说明"`             | 创建本地提交                |
| `git push`                       | 推送本地提交                |
| `git push -u origin 分支名`         | 发布分支并设置上游             |
| `git fetch origin`               | 获取远程更新信息              |
| `git pull`                       | 获取并整合远程更新             |
| `git merge 分支名`                  | 将指定分支合并到当前分支          |
| `git log --oneline -5`           | 查看最近5个提交              |
| `git reflog -5`                  | 查看 HEAD 最近5条引用变动      |
| `git reflog --date=iso main -10` | 查看 main 最近10条带时间的引用变动 |


---



# 12. 常见误区



## 误区一：Branch 是完全独立的仓库

错误。

不同分支通常共享部分提交历史，只是在不同位置继续开发。

## 误区二：Commit 后 GitHub 会自动更新

错误。

Commit 默认创建本地版本记录。

需要 Push 才能将新提交同步到远程。

## 误区三：Able to merge 代表代码正确

错误。

这只表示 GitHub 当前认为可以自动合并，不代表已经通过完整审核。

## 误区四：Fetch 和 Pull 是一样的

错误。

Fetch 主要获取远程更新。

Pull 会进一步将更新整合到当前本地分支。

## 误区五：Working tree clean 代表远程已同步

错误。

它表示当前工作区没有未提交的修改。

还需要检查当前分支及同步状态。

## 误区六：切换分支会自动合并修改

错误。

Switch 与 Merge 是不同操作。

## 误区七：Fast-forward 一定来自 Merge 命令

错误。

Fast-forward 既可能发生在 Merge 中，也可能发生在 Pull 中。

---



# 13. 学习总结



## 今日掌握的核心知识



### Branch

隔离不同开发任务，允许分支独立推进。

### Merge

将其他分支的修改整合到当前分支。

### Pull Request

在 GitHub 上提出合并请求，方便讨论、审查和协作。

### Code Review

在合并前检查修改的正确性、范围、可维护性与安全性。

### Fetch

获取远程更新信息。

### Pull

获取远程更新，并将其整合到本地分支。

### Push

将本地提交推送到远程仓库。

### Reflog

追踪 Git 本地引用的变化历史，辅助排查问题。

---



## 最重要的开发流程

```text
main
 │
 │ 创建开发分支
 ▼
feature/new-feature
 │
 │ 修改代码
 ▼
git add
 │
 ▼
git commit
 │
 ▼
git push
 │
 ▼
Pull Request
 │
 ▼
Code Review
 │
 ▼
Merge
 │
 ▼
GitHub main
 │
 │ git pull
 ▼
本地 main
```



## 今日学习反思

1. 理解了分支可以共享历史，但不会自动共享新提交。
2. 学会了在不同分支之间切换和合并。
3. 第一次创建并合并了 GitHub Pull Request。
4. 理解了 Code Review 与自动合并检查的区别。
5. 学会通过 Git Reflog 和软件日志调查实际发生的操作。
6. 认识到观察软件界面不能代替检查 Git 的实际状态。



## 后续学习方向

- Merge Conflict：合并冲突
- Branch Management：分支管理
- Git Diff：差异检查
- Git Restore：恢复修改
- Git Stash：临时保存工作区修改
- GitHub Actions：自动化工作流
- 实际代码项目中的 Pull Request 审查

---

> 核心原则：
>
> **先确认当前分支，再检查文件状态；理解每一次 Git 操作对本地和远程仓库产生的影响。**

