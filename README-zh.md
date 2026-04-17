# Publish Claude Skill

[English version](README.md)

把本地 Claude Code skill 整理成一个适合放到 GitHub 的干净仓库，并且默认不直接在 `.claude/skills/` 里发布。

## 这个 skill 会做什么

这个 skill 用一条简单流程帮你开源 Claude Code skill：

1. 找到源 skill 文件夹
2. 把可公开的文件复制到当前工作目录下的新文件夹
3. 在复制后的文件夹里补齐开源所需文件
4. 在本地创建第一次提交
5. 等用户手动创建 GitHub 仓库
6. 等拿到仓库 URL 后再连接远程并推送

## 默认工作方式

默认假设如下：

- 源 skill 可能在 `.claude/skills/<skill-name>/`
- 公开仓库应该创建在当前工作目录下
- Git 初始化、README、LICENSE、`.gitignore`、remote 和 push 都在复制后的文件夹里完成

除非用户明确要求，否则不要直接在 `.claude/skills/` 里原地发布。

## 最小开源仓库内容

一个最小可发布的 skill 仓库应包含：

- `skill.md`
- `README.md`
- `README-zh.md`
- `LICENSE`
- `.gitignore`

## 常见发布流程

### Claude 先做

1. 确认源 skill 文件夹
2. 在当前工作目录下创建目标文件夹
3. 复制公开文件到目标文件夹
4. 清理并补齐开源所需内容
5. 在复制后的文件夹里初始化 Git
6. 创建第一次提交

### 用户手动做

7. 在 GitHub 上创建空仓库
8. 把仓库 URL 发回聊天

### 然后 Claude 继续

9. 添加远程仓库
10. 尝试第一次 push
11. 如果因为认证或网络失败，明确告诉用户下一步该在本地运行什么命令

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
