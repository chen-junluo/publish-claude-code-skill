---
name: publish-claude-code-skill
description: Use when the user wants to publish a Claude Code skill to GitHub, open-source a local skill folder, prepare a first release, or ask what to do before uploading a skill repo. Trigger for requests about cleaning up a skill folder for publication, checking the minimum repo files, preparing the first commit, setting remotes, or separating manual GitHub steps from commands Claude can run.
tools: Bash, Read, Write
---

# Publish Claude Code Skill

Use this skill when the user wants to turn an existing local Claude Code skill folder into a clean GitHub repository.

Version notes:

- `v2.0.0`: treat the skill folder inside `.claude/skills/` as the canonical source directory and the repository itself
- `v1.0.0`: treated the local public repo and the `.claude/skills/` source folder as separate directories

This skill treats the actual skill folder as the repository by default.
Do not create a second copied repo folder unless the user explicitly asks for that.
The default workflow is:

1. locate the skill folder
2. prepare that same folder for open-source publication
3. create or verify the first local commit in that folder
4. the user creates the GitHub repository manually
5. Claude adds the remote and prepares push
6. the user completes authentication or the final push locally if needed

Use this workflow unless the user explicitly asks for a different GitHub automation path.

## What this skill should produce

Give the user a short, concrete publishing workflow that includes:

- where the skill folder lives
- whether that folder already has the minimum public files
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

## Core rule: publish from the skill folder itself

When the user says something like "用 publish-claude-code-skill 帮我发这个 skill", interpret it this way by default:

- the target is an existing skill folder such as `.claude/skills/<skill-name>/`
- Claude should inspect and clean up that same folder for publication
- Git initialization, README work, LICENSE, `.gitignore`, remote setup, and push preparation should happen in that same folder
- do not create a second publish folder unless the user explicitly asks for a copied export workflow

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

### Step 1. Identify the skill folder

First, identify the skill folder that will become the public repository.

Example pattern:

- skill folder: `~/.claude/skills/<skill-name>/`

Make this explicit to the user before editing or running Git commands.

### Step 2. Check the skill folder contents

Before doing Git work, verify that the skill folder actually contains the files that will be published.

Command:

```bash
ls -la "/absolute/path/to/skill-folder"
```

Confirm whether these files exist:

- `skill.md`
- `README.md`
- `LICENSE`
- `.gitignore`

If the repo is bilingual, also check for `README-zh.md`.

### Step 3. Prepare open-source-safe files in the skill folder

Make sure the folder content is fit for publication.
Common checks:

- remove personal paths or private assumptions from `skill.md` and README files
- make the README explain what the skill is, who it is for, and how to start
- add a visible cross-link near the top of each README if the repo is bilingual
- use `README-zh.md` as the required Chinese README filename
- add a `.gitignore` for local clutter if missing

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

### Step 4. Initialize Git in the skill folder if needed

If the skill folder is not yet a Git repo, initialize it.

```bash
git init "/absolute/path/to/skill-folder"
```

Then rename the default branch to `main`:

```bash
git -C "/absolute/path/to/skill-folder" branch -M main
```

If the folder is already a Git repo, inspect status instead of reinitializing it.

### Step 5. Create the first commit in the skill folder

Stage only the files that belong in the public repo.
Prefer specific file names over broad staging.

Typical first commit:

```bash
git -C "/absolute/path/to/skill-folder" add .gitignore LICENSE README.md README-zh.md skill.md
```

Then commit:

```bash
git -C "/absolute/path/to/skill-folder" commit -m "$(cat <<'EOF'
Initial open-source release of <Skill Name>

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
EOF
)"
```

After the commit, verify status:

```bash
git -C "/absolute/path/to/skill-folder" status --short
```

### Step 6. Ask the user to create the GitHub repository manually

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

### Step 7. Add the remote to the skill folder

When the user provides the remote URL, add it to the skill folder.
Prefer HTTPS unless the user explicitly wants SSH.

HTTPS form:

```bash
git -C "/absolute/path/to/skill-folder" remote add origin https://github.com/USER/REPO.git
```

SSH form:

```bash
git -C "/absolute/path/to/skill-folder" remote add origin git@github.com:USER/REPO.git
```

Check it:

```bash
git -C "/absolute/path/to/skill-folder" remote -v
```

### Step 8. Push the skill folder repo to GitHub

Try the first push:

```bash
git -C "/absolute/path/to/skill-folder" push -u origin main
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

1. identify the skill folder
2. check the files that will be published
3. clean up the folder for open-source release
4. initialize Git if needed or inspect the existing repo
5. create the first commit if needed

### The user does manually

6. create an empty GitHub repository
7. send the repository URL back

### Then Claude continues

8. add the remote to the skill folder repo
9. try the first push
10. if push fails due to authentication or network, tell the user exactly what to run locally

## Command cheat sheet

Use these as the minimal happy-path commands.
Replace the skill path and repo name as needed.

### Local prep

```bash
ls -la "/absolute/path/to/skill-folder"
```

### Initialize Git if needed

```bash
git init "/absolute/path/to/skill-folder"
```

```bash
git -C "/absolute/path/to/skill-folder" branch -M main
```

### First commit

```bash
git -C "/absolute/path/to/skill-folder" add .gitignore LICENSE README.md README-zh.md skill.md
```

```bash
git -C "/absolute/path/to/skill-folder" commit -m "$(cat <<'EOF'
Initial open-source release of <Skill Name>

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
EOF
)"
```

### Connect remote

```bash
git -C "/absolute/path/to/skill-folder" remote add origin https://github.com/USER/REPO.git
```

```bash
git -C "/absolute/path/to/skill-folder" remote -v
```

### Push

```bash
git -C "/absolute/path/to/skill-folder" push -u origin main
```

## Recommended response format

When the user asks how to publish a skill, structure the answer like this:

1. where the skill folder is
2. whether it already has the minimum publishable files
3. what I can do for you locally right now
4. what you need to do manually on GitHub
5. the exact commands I will run or want you to run
6. the next step after that

Keep it short and practical.
Do not drown the user in Git theory unless they ask.

## Boundaries

Do not create a second copied repo folder unless the user explicitly asks.
Do not promise GitHub-side automation unless the user explicitly asks for it and authentication is confirmed.
Do not assume SSH is configured.
Do not assume `gh` is installed.
Do not create extra docs or examples unless the user asks.
