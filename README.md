# Publish Claude Code Skill

[中文说明](README-zh.md)

Turn an existing local Claude Code skill folder into a clean GitHub repository.

## Versions

- `v2.0.0`: the skill folder inside `.claude/skills/` is treated as the canonical source directory and the repository itself
- `v1.0.0`: the workflow treated the local public repo and the `.claude/skills/` source folder as separate directories

## What this skill does

This skill helps you open-source a Claude Code skill by following a simple workflow:

1. locate the actual skill folder
2. check whether that folder already has the minimum public files
3. clean up the same folder for publication
4. create or verify the local Git commit in that folder
5. wait for the user to create the GitHub repository manually
6. connect the remote and push when the repository URL is ready

## Default workflow

By default, this skill assumes:

- the skill already lives in a real folder such as `.claude/skills/<skill-name>/`
- that same folder should become the Git repository
- Git work should happen in the skill folder itself, not in a copied export folder

## Minimum repository contents

A minimal open-source skill repository should include:

- `skill.md`
- `README.md`
- `README-zh.md` if the repo is bilingual
- `LICENSE`
- `.gitignore`

## Typical publishing flow

### Claude does first

1. identify the skill folder
2. inspect the files that will be published
3. clean up the folder for open-source release
4. initialize Git if needed, or inspect the existing repo
5. create the first commit if needed

### The user does manually

6. create an empty GitHub repository
7. send the repository URL back

### Then Claude continues

8. add the remote
9. try the first push
10. if push fails because of authentication or network issues, tell the user exactly what to run locally

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
