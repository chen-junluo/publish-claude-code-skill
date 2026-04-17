# Publish Claude Code Skill

[中文说明](README-zh.md)

Turn a local Claude Code skill into a clean GitHub repository without publishing directly from `.claude/skills/`.

## What this skill does

This skill helps you open-source a Claude Code skill by following a simple workflow:

1. locate the source skill folder
2. copy its public files into a new folder in the current working directory
3. prepare that copied folder for publication
4. create the first commit locally
5. wait for the user to create the GitHub repository manually
6. connect the remote and push when the repository URL is ready

## Default workflow

By default, this skill assumes:

- the source skill may live in `.claude/skills/<skill-name>/`
- the public repository should be created in the current working directory
- Git work should happen in the copied folder, not in the source skill folder

Do not publish directly from `.claude/skills/` unless the user explicitly asks for that.

## Minimum repository contents

A minimal open-source skill repository should include:

- `skill.md`
- `README.md`
- `README-zh.md`
- `LICENSE`
- `.gitignore`

## Typical publishing flow

### Claude does first

1. identify the source skill folder
2. create a target folder in the current working directory
3. copy the public skill files into that target folder
4. clean up the copied files for open-source release
5. initialize Git in the copied folder
6. create the first commit

### The user does manually

7. create an empty GitHub repository
8. send the repository URL back

### Then Claude continues

9. add the remote
10. try the first push
11. if push fails because of authentication or network issues, tell the user exactly what to run locally

## Manual GitHub step

When creating the GitHub repository, the user should:

1. create a new repository on GitHub
2. choose the repository name
3. choose Public or Private
4. not add a README on GitHub
5. not add a license on GitHub
6. not add a `.gitignore` on GitHub
7. copy the repository URL back into the chat

## Common failure signs

- `Host key verification failed`: SSH is not ready
- `Could not read from remote repository`: wrong remote or missing access
- HTTPS asks for credentials or token: GitHub login is needed
- `Could not resolve host: github.com`: network or DNS problem

## Source file

The actual skill instructions live in [skill.md](skill.md).
