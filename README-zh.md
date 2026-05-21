# Publish Claude Code Skill

[English version](README.md)

把一个已经存在的本地 Claude Code skill 文件夹整理成适合发布到 GitHub 的仓库。

## Versions

- `v2.0.0`：把 `.claude/skills/` 里的 skill 文件夹当作 canonical source directory，同时也当作仓库本身
- `v1.0.0`：把本地公开 repo 与 `.claude/skills/` 里的源 skill 文件夹视为两个分离目录

## 这个 skill 会做什么

这个 skill 用一条简单流程帮你开源 Claude Code skill：

1. 找到实际的 skill 文件夹
2. 检查这个文件夹是否已经具备最小公开文件集
3. 直接在这个文件夹里清理并补齐开源所需内容
4. 在这个文件夹里创建或确认本地 Git 提交
5. 等用户手动创建 GitHub 仓库
6. 等拿到仓库 URL 后再连接远程并推送

## 默认工作方式

默认假设如下：

- skill 已经存在于一个真实文件夹里，例如 `.claude/skills/<skill-name>/`
- 这个同一个文件夹本身就应该成为 Git 仓库
- Git 初始化、README、LICENSE、`.gitignore`、remote 和 push 都在这个 skill 文件夹里完成

## 最小开源仓库内容

一个最小可发布的 skill 仓库应包含：

- `skill.md`
- `README.md`
- 如果是双语仓库，则包含 `README-zh.md`
- `LICENSE`
- `.gitignore`

## 常见发布流程

### Claude 先做

1. 确认 skill 文件夹
2. 检查将被发布的文件
3. 清理并补齐开源所需内容
4. 如有需要则初始化 Git，否则检查现有 repo 状态
5. 如有需要则创建第一次提交

### 用户手动做

6. 在 GitHub 上创建空仓库
7. 把仓库 URL 发回聊天

### 然后 Claude 继续

8. 添加远程仓库
9. 尝试第一次 push
10. 如果因为认证或网络失败，明确告诉用户下一步该在本地运行什么命令

## 用户需要手动处理的 GitHub 步骤

创建 GitHub 仓库时，请：

1. 新建一个仓库
2. 选择仓库名
3. 选择 Public 或 Private
4. 不要让 GitHub 自动添加 README
5. 不要让 GitHub 自动添加 LICENSE
6. 不要让 GitHub 自动添加 `.gitignore`
7. 创建完成后，把仓库 URL 发回聊天

## 常见失败信号

- `Host key verification failed`：SSH 还没配好
- `Could not read from remote repository`：remote 错了，或者没有权限
- HTTPS 要求输入 credentials 或 token：需要完成 GitHub 登录
- `Could not resolve host: github.com`：这是网络或 DNS 问题

## 说明文件

真正的 skill 指令在 [skill.md](skill.md) 里。
