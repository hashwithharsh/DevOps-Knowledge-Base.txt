# Git & GitHub Complete DevOps Notes (Improved Edition)

> Revision-focused, interview-oriented, production-style Git notes for DevOps Engineers.

---

# 1. Why Git Exists

Before Git, developers faced major problems:

- code overwriting
- no rollback mechanism
- difficult collaboration
- manual file sharing
- no history tracking

Example:

Developer A:
- working on login feature

Developer B:
- working on payment feature

Without version control:
- files overwrite each other
- impossible to track changes safely

Git solves:
- versioning
- collaboration
- rollback
- auditing
- distributed development

---

# 2. What is Git?

Git is a:

- Distributed Version Control System (DVCS)
- open-source tool
- tracks file changes
- stores snapshots/history
- supports collaboration

Git allows:
- rollback to older versions
- branch-based development
- parallel development
- history tracking

---

# 3. What is GitHub?

GitHub is:

- cloud hosting platform for Git repositories

GitHub provides:
- repository hosting
- collaboration
- pull requests
- issue tracking
- CI/CD integrations
- code review

Git works locally.
GitHub works remotely.

---

# 4. Git vs GitHub

| Git | GitHub |
|---|---|
| Version control engine | Cloud hosting platform |
| Installed locally | Web platform |
| Tracks changes | Stores repositories online |
| Works offline | Requires internet |

---

# 5. Version Control System (VCS)

A Version Control System:

- tracks file changes
- maintains history
- supports rollback
- enables collaboration

---

# 6. Centralized vs Distributed VCS

## Centralized VCS

Examples:
- SVN
- CVS

Architecture:

Developer → Central Server ← Developer

Problems:
- single point of failure
- server dependency
- difficult offline work

---

## Distributed VCS

Example:
- Git

Architecture:

Developer ↔ Repository ↔ Developer

Advantages:
- every developer has full repository
- faster
- safer
- supports offline work

---

# 7. Git Life Cycle (MOST IMPORTANT)

```text
Working Directory (git init --> .git)
        ↓
git add
        ↓
Staging Area
        ↓
git commit
        ↓
Local Repository
        ↓
git push
        ↓
Remote Repository (GitHub)
```

---

# 8. Git Areas Explained

## Working Directory

Actual project files where development happens.

---

## Staging Area

Temporary preparation area.

Used before commit.

Command:

```bash
git add file.txt
```

---

## Local Repository

Commit history stored locally.

Command:

```bash
git commit -m "message"
```

---

## Remote Repository

Online repository.

Examples:
- GitHub
- GitLab
- Bitbucket

---

# 9. What is HEAD? (IMPORTANT INTERVIEW QUESTION)

HEAD is:

- pointer to current/latest commit

Example:

```text
Commit1 → Commit2 → Commit3 ← HEAD
```

HEAD usually points to:
- current branch latest commit

---

# Detached HEAD State

Occurs when:

```bash
git checkout <commit-id>
```

instead of branch.

Meaning:
- you are not on branch
- commits may become unreachable

Fix:

```bash
git checkout main
```

---

# 10. .git Directory Internals

When running:

```bash
git init
```

Git creates:

```text
.git/
```

Stores:
- commits
- branches
- objects
- hooks
- refs
- configuration
- logs

---

# Important Internal Directories

## objects/

Stores:
- commits
- trees
- blobs

Everything in Git stored as objects.

---

## refs/

Stores:
- branch references
- tags

---

## config

Stores:
- repository settings
- remotes
- username/email

---

# 11. Git Hooks

Location:

```bash
.git/hooks/
```

Hooks are:
- automated scripts triggered during Git events

---

# Pre-commit Hook

Runs BEFORE commit.

Use cases:
- prevent secret commits
- run lint checks
- formatting validation

---

# Post-commit Hook

Runs AFTER commit.

Use cases:
- notifications
- automation
- logging

---

# 12. .gitignore (VERY IMPORTANT)

Used to ignore files/folders.

Example:

```text
node_modules/
.env
*.log
__pycache__/
```

Why important:
- avoid committing secrets
- avoid unnecessary files
- cleaner repositories

---

# 13. Git Configuration

## Configure Username

```bash
git config --global user.name "Your Name"
```

---

## Configure Email

```bash
git config --global user.email "your@email.com"
```

---

## View Configuration

```bash
git config --list
```

---

# 14. Git Init vs Git Clone

## git init

Creates NEW repository.

```bash
git init
```

---

## git clone

Downloads EXISTING repository.

```bash
git clone <repo-url>
```

---

# 15. Fork vs Clone

## Clone

Creates local copy.

---

## Fork

Creates server-side copy under your account.

Common in open-source contribution.

---

# 16. Important Git Commands

# Git Status

Shows:
- modified files
- staged files
- untracked files

```bash
git status
```

---

# Git Add

Adds changes to staging area.

```bash
git add file.txt
```

All files:

```bash
git add .
```

---

# Git Commit

Creates snapshot.

```bash
git commit -m "message"
```

---

# Amend Latest Commit

```bash
git commit --amend
```

Used to:
- change latest commit message
- add forgotten staged files

IMPORTANT:
Works ONLY on latest commit.

---

# Git Log

Commit history.

```bash
git log
```

Compact:

```bash
git log --oneline
```

---

# Git Diff

Shows changes.

```bash
git diff
```

Compare commits:

```bash
git diff commit1 commit2
```

---

# Git Reflog

Shows ALL HEAD movements.

```bash
git reflog
```

Useful for:
- recovering deleted branches
- recovering lost commits

---

# 17. Branching

Branches allow:
- isolated development
- safe experimentation

---

# Main Branch

Usually:
- stable code
- production-ready

Examples:
- main
- master

---

# Feature Branch

Used for:
- new features
- isolated work

Example:

```text
feature-login
```

---

# Release Branch

Used for:
- final testing
- release stabilization

---

# Hotfix Branch

Used for:
- urgent production fixes

---

# Real Branching Workflow

```text
main
 ├── feature-login
 ├── feature-payment
 ├── release-v1
 └── hotfix-security
```

---

# Create Branch

```bash
git branch feature-login
```

---

# Switch Branch

```bash
git checkout feature-login
```

Modern method:

```bash
git switch feature-login
```
# Lists both local and remote branches

```bash
git branch -a
```
# Lists remote branches with verbose details

```bash
git branch -r -v
```

---

# Create + Switch

```bash
git checkout -b feature-login
```

---

# Delete Branch

Safe delete:

```bash
git branch -d branchname
```

Force delete:

```bash
git branch -D branchname
```

---

# 18. Merge

Combines branches.

```bash
git merge branchname
```

---

# Fast Forward Merge

Occurs when:
- no divergence exists

Git simply moves pointer forward.

---

# Recursive/Three-Way Merge

Occurs when:
- both branches changed independently

Git creates merge commit.

---

# Merge Conflict

Occurs when:
- same lines modified differently

Git cannot decide correct version.

---

# Resolve Conflict

Steps:
1. open file
2. remove conflict markers
3. keep correct code
4. stage file
5. commit

---

# 19. Git Fetch vs Pull

## Git Fetch

Downloads changes ONLY.

```bash
git fetch
```

Does NOT merge.

---

## Git Pull

Downloads + merges.

```bash
git pull
```

Equivalent:

```text
git fetch + git merge
```

---

# 20. Git Push

Uploads commits to remote repository.

```bash
git push
```

---

# Push Specific Branch

```bash
git push origin main
```

---

# Upstream Tracking

```bash
git push -u origin main
```

`-u` means:
- set upstream tracking

After this:
- future `git push`
- future `git pull`

work automatically.

---

# Force Push (DANGEROUS)

```bash
git push --force
```

WARNING:
- rewrites remote history
- can overwrite teammate work

Use carefully.

Safer alternative:

```bash
git push --force-with-lease
```

---

# 21. Git Reset vs Revert

# Git Reset

Moves HEAD backward.

```bash
git reset --hard <commit-id>
```

Danger:
- destroys changes

---

# Git Revert

Creates NEW commit undoing old commit.

```bash
git revert <commit-id>
```

Safer for shared branches.

---

# Simple Understanding

| Reset | Revert |
|---|---|
| Rewrite history | Safe undo |
| Dangerous | Safer |
| Removes commits | Adds reversing commit |

---

# 22. Git Rebase

Rewrites history into linear form.

```bash
git rebase main
```

Advantages:
- cleaner history

Disadvantages:
- rewrites commit history

---

# Golden Rule

Avoid rebasing shared/public branches.

---

# 23. Git Stash

Temporarily saves uncommitted work.

---

```bash
git stash            ( Temporarily Save Changes )
 
git stash pop        ( Restore Changes  )

git stash list       ( View Stashes )
```

---

# 24. Git Cherry-pick

Copies specific commit.

```bash
git cherry-pick <commit-id>
```

Use case:
- only one commit needed

---

# 25. Tags (IMPORTANT FOR DEVOPS)

Tags mark important versions/releases.--> A permanent label attached to a specific commit.

Commit A → Commit B → Commit C → Commit D
                           ↑
                         v1.0

Examples:
- v1.0
- v2.0

---

# Create Tag

```bash
git tag v1.0
```

---

# Push Tags

```bash
git push origin --tags
git push --tags (all tags pushed)
```

---

# Why Tags Matter

Used in:
- CI/CD pipelines :- Pipelines can trigger deployments only for tagged versions.
     git tag v1.0
        ↓
    Jenkins/GitHub Actions Trigger
        ↓
    Deploy to Production

- deployments :- Tags identify exact code version running in production. ex-v1.0 v1.1 v2.0 
- release management :- Rollbacks
If latest deployment fails: Rollback from v2.0 → v1.1 (Easy and safe recovery.)

---

# 26. SSH vs HTTPS Authentication

## HTTPS

Example:

```text
https://github.com/user/repo.git
```

Requires:
- username/password/token PAT (access key)

---

## SSH

Example:

```text
git@github.com:user/repo.git
```

Uses SSH keys.

Preferred in companies.

---

# Generate SSH Key

```bash
ssh-keygen -t rsa -b 4096
```
---
# View Public Key

```bash
cat ~/.ssh/id_rsa.pub
```

---

# 27. Important Troubleshooting Scenarios

# Scenario 1 — Deleted Branch Recovery

Use:

```bash
git reflog
```

Restore:

```bash
git checkout -b branchname <commit-id>
```

---

# Scenario 2 — Accidentally Deleted File

Restore:

```bash
git restore filename
```

---

# Scenario 3 — Wrong Commit Message

Fix latest commit:

```bash
git commit --amend
```

---

# Scenario 4 — Merge Conflict

Fix manually:
- edit file
- remove markers
- commit again

---

# 28. Real DevOps Usage

Git used heavily in:
- Jenkins pipelines   → Jenkins pulls code from Git repositories to build, test, and deploy applications automatically.
- Terraform           → Infrastructure-as-Code (IaC) files are version controlled using Git for tracking infrastructure changes safely.
- Kubernetes YAML     → Kubernetes manifests are stored in Git for deployment automation and rollback management.
- Helm charts         → Helm deployment templates are managed in Git for versioning and environment consistency.
 - Dockerfiles        → Docker image build instructions are stored in Git for reproducible container builds.
- Ansible             → Ansible playbooks are version controlled in Git for automation consistency and infrastructure management.
- CI/CD               → Git acts as the trigger source for automated build, test, and deployment pipelines.

---

# Typical DevOps Flow

```text
Developer -- Writes code / infrastructure files
    ↓
   Git -- Tracks changes & version history
    ↓
GitHub/GitLab -- Stores remote repository & enables collaboration
    ↓
Jenkins / GitHub Actions / GitLab CI -- Automatically triggers CI/CD pipeline
    ↓
Build Stage -- Application build / Docker image creation
    ↓
Test Stage -- Runs automated tests & validations
    ↓
Deploy Stage -- Deploys application to servers / Kubernetes / Cloud
    ↓
Production Environment 
    ↓
Users access application
```

---
# 29. Important Interview Questions

# Beginner

1. What is Git? 
   → Distributed Version Control System used for tracking code changes and collaboration.

2. Why Git is distributed?  
   → Every developer has full repository copy with complete history locally.

3. Git vs GitHub?  
   → Git = version control tool, GitHub = cloud hosting platform for Git repositories.

4. What is staging area?  
   → Temporary area where changes are prepared before commit.

5. Difference between add and commit?  
   → `git add` stages changes, `git commit` saves snapshot permanently.

6. What is HEAD?  
   → Pointer to current/latest commit in active branch.

7. What is merge conflict?  
   → Happens when Git cannot automatically merge same code changes.

8. Difference between fetch and pull?  
   → Fetch downloads changes only, pull downloads + merges changes.

9. Difference between reset and revert?  
   → Reset rewrites history, revert safely undoes changes using new commit.

10. What is stash?  
   → Temporarily stores uncommitted changes without creating commit.

---

# Intermediate

1. Explain Git lifecycle  
   → Working Directory → Staging Area → Local Repository → Remote Repository.

2. What happens internally during commit?  
   → Git creates snapshot object and stores metadata/history inside `.git`.

3. Difference between merge and rebase?  
   → Merge preserves history, rebase creates cleaner linear history.

4. What is detached HEAD?  
   → HEAD points directly to commit instead of branch.

5. What is reflog?  
   → Log of all HEAD and branch movements for recovery purposes.

6. Why use feature branches?  
   → Isolates development work safely without affecting main branch.

7. What are Git hooks?  
   → Automated scripts triggered before/after Git events like commit or push.

8. Difference between clone and fork?  
   → Clone creates local copy, fork creates server-side repository copy.
---

# Scenario-Based

## Developer committed AWS secret accidentally.

Solution:
- remove secret
- rotate credentials
- use pre-commit hooks
- use secret scanning

---

## Production broken after deployment.

Solution:
- git revert
- rollback deployment

---

## Deleted branch accidentally.

Solution:
- git reflog
- restore using commit id

---

# Git Important Use Cases (DevOps Practical Notes)

---

# 1. Delete File from Git + GitHub

## Delete tracked file

```bash
git rm test.txt
```

## Commit deletion

```bash
git commit -m "deleted test.txt"
```

## Push to GitHub

```bash
git push
```

### Result

- file deleted locally
- file deleted from GitHub repository

---

# 2. See Deleted Files and Restore Them

## Show deleted files history

```bash
git log --diff-filter=D --summary
```

---

## Restore deleted file from specific commit

```bash
git checkout <commit-id> filename
```

Example:

```bash
git checkout a1b2c3 app.py
```

---

## Restore recently deleted file

```bash
git restore filename
```

Example:

```bash
git restore app.py
```

### Result

- deleted file restored locally

---

# 3. Delete Branch from Git + GitHub

# Delete Local Branch

Safe delete:

```bash
git branch -d feature-login
```

Force delete:

```bash
git branch -D feature-login
```

---

# Delete Remote Branch (GitHub)

```bash
git push origin --delete feature-login
```

### Result

- branch removed from GitHub repository

---

# Restore Deleted Branch Locally

## Step 1 — Find branch commit

```bash
git reflog
```

---

## Step 2 — Restore branch

```bash
git checkout -b feature-login <commit-id>
```

Example:

```bash
git checkout -b feature-login a1b2c3
```

---

# OR Pull Branch Again from GitHub

```bash
git checkout -b feature-login origin/feature-login
```

### Result

- branch restored locally from remote repository

---

# 4. Delete File Locally and Pull File Again from GitHub

## Scenario

File deleted accidentally locally.

---

# Restore Single File from GitHub/Main Branch

```bash
git checkout main -- filename
```

Example:

```bash
git checkout main -- app.py
```

---

# OR Restore Using Git Restore

```bash
git restore filename
```

---

# OR Pull Latest Changes from GitHub

```bash
git pull
```

### Result

- missing file restored from remote repository

---

# 5. Recover Lost Commit Using Reflog

## Show HEAD movement history

```bash
git reflog
```

---

## Restore old commit

```bash
git checkout <commit-id>
```

OR create branch:

```bash
git checkout -b recovered-branch <commit-id>
```

### Result

- lost commit recovered

---

# 6. Undo Last Commit Safely

## Revert latest commit

```bash
git revert HEAD
```

### Result

- creates new commit undoing previous changes
- safe for shared branches

---

# 7. Remove Last Commit Completely

## WARNING: Dangerous

```bash
git reset --hard HEAD~1
```

### Result

- removes last commit permanently
- deletes changes

---

# 8. Temporarily Save Unfinished Work

## Save work

```bash
git stash
```

---

## Restore work

```bash
git stash pop
```

### Result

- unfinished changes restored

---

# 9. Change Last Commit Message

```bash
git commit --amend -m "new message"
```

### Result

- latest commit message updated

---

# 10. Add Forgotten File to Last Commit

```bash
git add forgotten.py
git commit --amend
```

### Result

- forgotten file added into previous commit

---

# 11. Clone Existing Repository

```bash
git clone https://github.com/user/repo.git
```

### Result

- full repository downloaded locally

---

# 12. Create and Switch Branch

```bash
git checkout -b feature-login
```

OR modern command:

```bash
git switch -c feature-login
```

### Result

- new branch created and switched

---

# 13. Merge Feature Branch into Main

## Switch to main

```bash
git checkout main
```

---

## Merge feature branch

```bash
git merge feature-login
```

### Result

- feature code merged into main branch

---

# 14. Resolve Merge Conflict

## Open conflicted file

Conflict example:

```text
<<<<<<< HEAD
main code
=======
feature code
>>>>>>> feature-login
```

---

## Keep correct code manually

Then:

```bash
git add .
git commit
```

### Result

- conflict resolved

---

# 15. Push Local Repository to GitHub

## Add remote

```bash
git remote add origin <repo-url>
```

---

## Push code

```bash
git push -u origin main
```

### Result

- local repository uploaded to GitHub

---

# 16. Pull Latest Changes from GitHub

```bash
git pull
```

### Result

- latest remote changes downloaded and merged

---

# 17. Download Latest Changes Without Merging

```bash
git fetch
```

### Result

- latest changes downloaded only
- no merge performed

---

# 18. Create Production Release Tag

## Create tag

```bash
git tag -a v1.0 -m "Production release"
```

---

## Push tags

```bash
git push origin --tags
```

### Result

- stable release version created

---

# 19. See Commit History

## Full history

```bash
git log
```

---

## Compact history

```bash
git log --oneline
```

### Result

- displays commit timeline

---

# 20. Compare File Changes

```bash
git diff
```

### Result

- shows unstaged changes

---

# 21. Compare Two Commits

```bash
git diff commit1 commit2
```

### Result

- shows difference between versions

---

# 22. See Remote Repositories

```bash
git remote -v
```

### Result

- displays configured remotes

---

# 23. Ignore Sensitive Files

## Create .gitignore

```text
.env
node_modules/
*.log
```

### Result

- files ignored from Git tracking

---

# 24. Generate SSH Key for GitHub

```bash
ssh-keygen -t rsa -b 4096
```

---

## View public key

```bash
cat ~/.ssh/id_rsa.pub
```

### Result

- SSH authentication setup for GitHub

---

# 25. Recover Deleted Local Changes

## Restore file

```bash
git restore filename
```

### Result

- local modifications discarded

---

# Quick Interview Keywords

| Scenario | Main Command |
|---|---|
| Delete tracked file | `git rm` |
| Restore deleted file | `git restore` |
| Recover deleted branch | `git reflog` |
| Delete remote branch | `git push origin --delete` |
| Restore branch from GitHub | `git checkout -b origin/branch` |
| Pull latest files | `git pull` |
| Temporary save work | `git stash` |
| Undo safely | `git revert` |
| Remove commit permanently | `git reset --hard` |
| Create release version | `git tag` |
| Download repository | `git clone` |
| Compare changes | `git diff` |


# 30. Final Important Rules

- Always check `git status`
- Avoid force push on shared branches
- Use meaningful commit messages
- Avoid direct commits to main
- Learn merge conflict handling practically
- Prefer revert over reset in teams
- Understand rebase before using
- Practice daily

---

# 31. One-Line Definitions

| Concept | Definition |
|---|---|
| Git | Distributed Version Control System |
| GitHub | Git hosting platform |
| Commit | Snapshot of changes |
| Branch | Separate line of development |
| Merge | Combine branches |
| Rebase | Rewrite history linearly |
| Clone | Download repository |
| Fork | Create server-side copy |
| HEAD | Pointer to current commit |
| Hook | Automated Git script |
| Revert | Undo via new commit |
| Reset | Move HEAD backward |
| Staging Area | Temporary area before commit |
| Tag | Named release/version |

---

# END
