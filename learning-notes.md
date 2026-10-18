# My Git Learning Notes

> Git 与 GitHub 学习笔记  
> 学习目标：掌握基础版本管理，能够独立管理 AI 开发项目。

---

## 1. 基础概念

### Git

Git 是一种版本控制工具，用于记录和管理项目文件的修改历史。

### GitHub

GitHub 是代码托管和协作平台，可以存储 Git 仓库，并支持多人协作。

### Repository（Repo）

Repository 是代码仓库，用于存储项目文件及其版本历史。

- Local Repository：本地仓库
- Remote Repository：远程仓库

### README.md

项目说明文件，通常包含项目介绍、安装方法和使用说明。

### Issues

用于记录、讨论和追踪项目问题、Bug 和新功能需求。

---

## 2. Git 的核心工作流程

Git 的基本工作流程：

工作区 → 暂存区 → 本地仓库 → 远程仓库

对应操作：

```text
Working Directory（工作区）
        |
        | git add
        ↓
Staging Area（暂存区）
        |
        | git commit
        ↓
Local Repository（本地仓库）
        |
        | git push
        ↓
Remote Repository（GitHub远程仓库）
```

### git add

将指定文件的修改加入暂存区。

### git commit

将暂存区中的修改创建为本地版本记录。

### git push

将本地提交推送到远程仓库。

注意：Commit 不等于 Push。

---

## 3. 常用终端命令

### 查看 Git 版本

```bash
git --version
```

作用：检查 Git 是否安装及其版本。

### 切换文件夹

```bash
cd ~/Documents/my-first-project
```

作用：进入指定项目目录。

### 查看文件

```bash
ls
```

作用：查看当前目录中的文件。

### 查看当前路径

```bash
pwd
```

作用：显示当前所在文件夹的完整路径。

### 克隆 GitHub 仓库

```bash
git clone <仓库地址>
```

作用：将远程仓库复制到本地，包括项目文件和版本历史。

### 查看 Git 状态

```bash
git status
```

作用：查看当前分支、文件修改和暂存状态。

### 暂存指定文件

```bash
git add learning-notes.md
```

作用：将指定文件的修改加入暂存区。

### 暂存当前目录的修改

```bash
git add .
```

作用：暂存当前目录及其子目录中的相关修改。

注意：执行前应确认没有不需要提交的文件。

### 创建本地提交

```bash
git commit -m "Update learning notes"
```

作用：创建一次本地版本记录。

其中：

- commit：创建版本提交
- -m：指定提交说明
- 引号中的内容：本次修改的描述

### 推送到 GitHub

```bash
git push
```

作用：将本地提交推送到对应远程分支。

### 查看本地分支

```bash
git branch
```

作用：列出本地分支，星号表示当前分支。

---

## 4. 完整项目操作流程

### 第一步：进入项目

```bash
cd ~/Documents/my-first-project
```

### 第二步：检查状态

```bash
git status
```

### 第三步：使用 Cursor 修改文件

打开项目文件，进行编辑。

修改完成后，按：

Command + S

保存文件。

### 第四步：再次检查状态

```bash
git status
```

如果显示：

```text
Changes not staged for commit:
    modified: learning-notes.md
```

说明文件已修改，但尚未暂存。

### 第五步：暂存修改

```bash
git add learning-notes.md
```

### 第六步：检查暂存状态

```bash
git status
```

如果显示：

```text
Changes to be committed:
    modified: learning-notes.md
```

说明文件已进入暂存区。

### 第七步：创建版本记录

```bash
git commit -m "Update learning notes"
```

### 第八步：检查本地提交

```bash
git status
```

如果显示：

```text
Your branch is ahead of 'origin/main' by 1 commit.
```

说明本地领先远程跟踪分支一个提交。

### 第九步：同步到 GitHub

```bash
git push
```

### 第十步：验证结果

```bash
git status
```

正常同步后通常显示：

```text
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

最后打开 GitHub 网页，确认文件内容已经更新。

---

## 5. 常见错误与解决方法

### 错误一：git add 没有指定文件

错误：

```bash
git add
```

可能显示：

```text
Nothing specified, nothing added.
```

解决方法：

```bash
git add learning-notes.md
```

### 错误二：命令拼写错误

错误：

```bash
git statues
```

正确：

```bash
git status
```

Git 通常会提示最接近的正确命令。

### 错误三：找不到文件夹

可能显示：

```text
cd: no such file or directory
```

原因：路径不存在或输入错误。

解决方法：确认项目所在位置，注意路径拼写及大小写。

### 错误四：Git 用户身份未配置

可能显示：

```text
Author identity unknown
```

解决方法：

```bash
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱"
```

建议使用 GitHub 已验证邮箱或提供的隐私邮箱。

注意：Git 用户身份配置不等于 GitHub 登录认证。

### 错误五：Push 失败

可能原因：

- 网络连接异常
- GitHub 身份验证失败
- 没有远程仓库写入权限
- 远程分支存在尚未获取的更新

应先阅读错误信息，再针对具体原因处理。

可以使用 GitHub Desktop 辅助完成认证和推送。

---

## 6. Markdown 基础语法

### 标题

```markdown
# 一级标题
## 二级标题
### 三级标题
```

### 加粗

```markdown
**重要内容**
```

### 列表

```markdown
- 第一项
- 第二项
- 第三项
```

### 代码

使用三个反引号包围多行代码。

Markdown 文件可以直接在 GitHub 上预览。

---

## 7. 开发注意事项

1. 修改代码前，先检查 Git 状态。
2. Git 命令要注意拼写、空格和文件名。
3. Commit 前确认暂存内容正确。
4. 每次 Commit 尽量使用清晰的提交说明。
5. 不要将密码、API Key 等敏感信息提交到仓库。
6. Public 仓库中的公开文件可以被其他人查看。
7. 遇到错误时先阅读终端信息，不要盲目重复命令。

---

## 8. 学习总结

Git 负责版本管理。

GitHub 负责远程托管和协作。

Cursor 用于编辑代码。

三个核心操作：

- Add：暂存修改
- Commit：记录本地版本
- Push：同步到远程仓库

核心原则：

理解命令的作用，比机械记忆命令更重要。