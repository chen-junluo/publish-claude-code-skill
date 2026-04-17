---
name: publish-claude-code-skill
description: Use when the user wants to publish a Claude Code skill to GitHub, open-source a local skill folder, prepare a first release, or ask what to do before uploading a skill repo. Trigger for requests about copying a local skill into the current working folder, writing the minimum open-source files, preparing the first commit, setting remotes, or separating manual GitHub steps from commands Claude can run.
tools: Bash, Read, Write
---

# Publish Claude Code Skill

Use this skill when the user wants to turn a local Claude Code skill into a clean GitHub repository.

This skill assumes the source skill may live inside `.claude/skills/`, but the public repo should be created in the current working folder.
Do not publish a skill in place from `.claude/skills/` unless the user explicitly asks for that.
The default workflow is:

1. locate the source skill
2. copy its public files into a new folder inside the current working directory
3. prepare that copied folder as the public repo
4. the user creates the GitHub repository manually
5. Claude connects the remote and prepares push
6. the user completes the final push if authentication or network issues require it

Use this workflow unless the user explicitly asks for a different GitHub automation path.

## What this skill should produce

Give the user a short, concrete publishing workflow that includes:

- where the source skill lives
- where the public repo folder will be created
- the minimum repository contents
- the manual steps the user must handle
- the exact core commands needed for the happy path
- the common failure points and how to recognize them
- a clear step-by-step guide the user can follow without guessing

If the user asks you to actually perform the workflow, follow the steps below in order.
Do not skip steps.

## Default reader

Assume the user may be new to GitHub.
Prefer plain language over Git jargon.
Explain what each step is for before or alongside the command.

## Core rule: publish from the current working folder

When the user says something like "用 publish-claude-code-skill 帮我发这个 skill", interpret it this way by default:

- the source skill may be in `.claude/skills/<skill-name>/`
- Claude should copy the publishable files into a folder in the current working directory
- Git initialization, README work, LICENSE, `.gitignore`, remote setup, and push preparation should happen in that copied folder
- do not initialize Git or create the public repo directly inside `.claude/skills/`

Only publish in place from `.claude/skills/` if the user explicitly asks for that.

## Minimum open-source repo for a Claude Code skill

A simple skill repo is usually ready when it has:

- `skill.md`
- `README.md`
- `LICENSE`
- `.gitignore`

If the repo is bilingual, also include:

- `README-zh.md`
- a visible link near the top of `README.md` pointing to `README-zh.md`
- a visible link near the top of `README-zh.md` pointing to `README.md`

Treat these as required, not optional.
Do not use `README.zh.md`.
Do not invent extra files unless the user wants them.

## Required workflow

### Step 1. Identify the source skill and target publish folder

First, identify:

- the source skill folder
- the target folder in the current working directory

Example pattern:

- source: `~/.claude/skills/<skill-name>/`
- target: `<current-working-directory>/<repo-name>/`

Make this explicit to the user before copying files.

### Step 2. Copy the skill into the current working folder

Create a new target folder in the current working directory.
Then copy the publishable files from the source skill into that folder.

The copied folder is the one that becomes the public GitHub repository.
Do not treat the source skill folder as the Git repo unless the user explicitly asks for that.

Files to copy usually include:

- `skill.md`
- `README.md` if it exists
- `README-zh.md` if it exists
- `LICENSE` if it exists
- other small public files the user wants included

Do not copy local clutter such as `.DS_Store`, `.git`, or `.claude/`.

### Step 3. Check the copied folder contents

Before doing Git work, verify that the target folder actually contains the files that will be published.

Command:

```bash
ls -la "/absolute/path/to/publish-folder"
```

Confirm whether these files exist:

- `skill.md`
- `README.md`
- `LICENSE`
- `.gitignore`

If the repo is bilingual, also check for `README-zh.md`.

### Step 4. Prepare open-source-safe files in the copied folder

Make sure the copied content is fit for publication.
Common checks:

- remove personal paths or private assumptions from `skill.md` and README files
- make the README explain what the skill is, who it is for, and how to start
- add a visible cross-link near the top of each README if the repo is bilingual
- use `README-zh.md` as the required Chinese README filename
- add a `.gitignore` for local clutter

A good minimal `.gitignore` for a skill repo is:

```gitignore
.DS_Store
Thumbs.db
*.swp
*.swo
.vscode/
.idea/
.claude/
```

### Step 5. Initialize Git in the copied folder

If the publish folder is not yet a Git repo, initialize it.

```bash
git init "/absolute/path/to/publish-folder"
```

Then rename the default branch to `main`:

```bash
git -C "/absolute/path/to/publish-folder" branch -M main
```

### Step 6. Create the first commit in the copied folder

Stage only the files that belong in the public repo.
Prefer specific file names over broad staging.

Typical first commit:

```bash
git -C "/absolute/path/to/publish-folder" add .gitignore LICENSE README.md README-zh.md skill.md
```

Then commit:

```bash
git -C "/absolute/path/to/publish-folder" commit -m "$(cat <<'EOF'
Initial open-source release of <Skill Name>

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
EOF
)"
```

After the commit, verify status:

```bash
git -C "/absolute/path/to/publish-folder" status --short
```

### Step 7. Ask the user to create the GitHub repository manually

This workflow treats GitHub repository creation as a manual step.
Ask the user to do this on GitHub before continuing.

Tell the user to:

1. open GitHub
2. create a new repository
3. choose the repository name
4. choose Public or Private
5. do not add a README on GitHub
6. do not add a license on GitHub
7. do not add a `.gitignore` on GitHub
8. copy the remote URL back into the chat

Do not continue to remote setup until the user gives the repository URL.

### Step 8. Add the remote to the copied folder

When the user provides the remote URL, add it to the copied publish folder.
Prefer HTTPS unless the user explicitly wants SSH.

HTTPS form:

```bash
git -C "/absolute/path/to/publish-folder" remote add origin https://github.com/USER/REPO.git
```

SSH form:

```bash
git -C "/absolute/path/to/publish-folder" remote add origin git@github.com:USER/REPO.git
```

Check it:

```bash
git -C "/absolute/path/to/publish-folder" remote -v
```

### Step 9. Push the copied folder repo to GitHub

Try the first push:

```bash
git -C "/absolute/path/to/publish-folder" push -u origin main
```

If push works, report success.
If push fails, do not guess. Identify which class of problem it is and tell the user the next step.

## Manual steps the user must be ready to do

Call these out clearly.
These are part of the standard workflow for this skill.

### Manual step A. Create the GitHub repository

The user creates the empty GitHub repo manually.
This is not an exception in this workflow. It is the default path.

### Manual step B. Complete GitHub authentication if push fails

Even if the repo exists, push may fail because the machine is not authenticated.
Common signs:

- SSH error: `Host key verification failed`
- SSH error: `Could not read from remote repository`
- HTTPS error asking for credentials or token

In that case, the user may need to:

- approve GitHub login in their terminal
- configure SSH keys
- or switch from SSH to HTTPS

### Manual step C. Complete the final push in their own terminal if needed

Push may fail because the current environment cannot reach GitHub at all.
Common sign:

- `Could not resolve host: github.com`

That is a network or DNS issue, not a repo-content issue.
In that case, ask the user to run the final `git push` in their own terminal.

## Step-by-step guide to show the user

When the user asks for publishing help, present the workflow in this order.
Use numbered steps, not a loose summary.

### Claude does first

1. identify the source skill folder
2. create a target publish folder in the current working directory
3. copy the public skill files into that target folder
4. clean up the copied files for open-source release
5. initialize Git in the copied folder if needed
6. create the first commit in the copied folder

### The user does manually

7. create an empty GitHub repository
8. send the repository URL back

### Then Claude continues

9. add the remote to the copied folder repo
10. try the first push
11. if push fails due to authentication or network, tell the user exactly what to run locally

## Command cheat sheet

Use these as the minimal happy-path commands.
Replace the source path, target path, and repo name as needed.

### Local prep

```bash
ls -la "/absolute/path/to/source-skill-folder"
```

```bash
mkdir -p "/absolute/path/to/current-working-directory/repo-name"
```

```bash
cp "/absolute/path/to/source-skill-folder/skill.md" "/absolute/path/to/current-working-directory/repo-name/skill.md"
```

### Check the copied folder

```bash
ls -la "/absolute/path/to/current-working-directory/repo-name"
```

### Initialize Git in the copied folder

```bash
git init "/absolute/path/to/current-working-directory/repo-name"
```

```bash
git -C "/absolute/path/to/current-working-directory/repo-name" branch -M main
```

### First commit

```bash
git -C "/absolute/path/to/current-working-directory/repo-name" add .gitignore LICENSE README.md README-zh.md skill.md
```

```bash
git -C "/absolute/path/to/current-working-directory/repo-name" commit -m "$(cat <<'EOF'
Initial open-source release of <Skill Name>

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
EOF
)"
```

### Connect remote

```bash
git -C "/absolute/path/to/current-working-directory/repo-name" remote add origin https://github.com/USER/REPO.git
```

```bash
git -C "/absolute/path/to/current-working-directory/repo-name" remote -v
```

### Push

```bash
git -C "/absolute/path/to/current-working-directory/repo-name" push -u origin main
```

## Recommended response format

When the user asks how to publish a skill, structure the answer like this:

1. where the source skill is
2. where I will create the public repo folder in the current working directory
3. what I can do for you locally right now
4. what you need to do manually on GitHub
5. the exact commands I will run or want you to run
6. the next step after that

Keep it short and practical.
Do not drown the user in Git theory unless they ask.

## Boundaries

Do not publish directly from `.claude/skills/` unless the user explicitly asks.
Do not promise GitHub-side automation unless the user explicitly asks for it and authentication is confirmed.
Do not assume SSH is configured.
Do not assume `gh` is installed.
Do not create extra docs or examples unless the user asks.
