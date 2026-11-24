# Git and GitHub — Industry-Style Practical Lab
## From First Commit to Branching, Pull Requests, Conflict Resolution, and Team Workflow

**Level:** Absolute Beginner to Industry Practice  
**Tools:** Git, GitHub, Terminal, and optionally VS Code  
**Project scenario:** ShopEasy — a sample e-commerce application  
**Goal:** Understand not only which commands to run, but why teams use them and how Git and GitHub work together in real software development.

> Work through the labs in order. Read each explanation, run the command, inspect the result, and understand the effect. Replace `YOUR-USERNAME`, `YOUR-HOME`, and example URLs with your own values.

---

# Contents

1. Learning objectives
2. Git vs GitHub
3. How Git works internally
4. Install Git and configure identity
5. Create a GitHub repository
6. Lab 1 — Create a local repository and first commit
7. The Git file lifecycle and staging area
8. Lab 2 — Stage, commit, inspect, and compare changes
9. `.gitignore`
10. Connect a local repository to GitHub
11. Lab 3 — Push, fetch, pull, and clone
12. Branching concepts and commands
13. Lab 4 — Feature branches and merging
14. Merge strategies
15. Merge conflicts
16. Lab 5 — Deliberately create and resolve a conflict
17. Pull Requests and code review
18. Lab 6 — Complete GitHub Pull Request workflow
19. Collaborator and fork-based workflows
20. Undoing mistakes safely
21. Stash and cherry-pick
22. Tags and releases
23. Repository protection and GitHub Actions
24. Typical industry workflow
25. Troubleshooting
26. Final team simulation
27. Exercises and interview questions
28. Command cheat sheet

---

# 1. Learning objectives

By the end of this lab, you should be able to:

- Explain Git, GitHub, repositories, commits, branches, remotes, and `HEAD`.
- Configure Git on your computer.
- Create a repository locally and publish it to GitHub.
- Understand the working directory, staging area, and local repository.
- Inspect changes and write meaningful commit messages.
- Create, switch, rename, merge, and delete branches.
- Push and pull changes between your computer and GitHub.
- Clone a remote repository.
- Create and resolve merge conflicts.
- Open Pull Requests and participate in code review.
- Understand collaborators, forks, `origin`, and `upstream`.
- Use `restore`, `revert`, `reset`, `stash`, `cherry-pick`, and tags safely.
- Follow a feature-branch workflow used by many development teams.
- Understand branch protection and a basic CI workflow.

# 2. What are Git and GitHub?

## 2.1 Git

Git is a **distributed version control system**. It records changes to files over time. Developers use it to see what changed, compare versions, collaborate, and recover earlier versions of their work.

Imagine you are developing an e-commerce application. You add a login page, then a product page, and later a code change breaks the login page. Without version control, you may have folders named `project-final`, `project-final-new`, and `project-final-fixed`. That approach quickly becomes confusing.

Git gives the project a history of commits.

## 2.2 GitHub

GitHub is a platform for hosting Git repositories and collaborating on software projects. It provides features such as Pull Requests, Issues, code review, repository permissions, releases, and GitHub Actions.

Git is the version control tool. GitHub is one platform that hosts Git repositories. Other Git hosting platforms include GitLab and Bitbucket.

## 2.3 Git vs GitHub

| Git | GitHub |
|---|---|
| Version control software | Repository hosting and collaboration platform |
| Runs locally on your computer | Provides cloud-hosted repositories and web features |
| Tracks file changes and commits | Hosts repositories and supports PRs, Issues, Actions |
| Local operations can work offline | Remote collaboration requires network access |
| Commands include `git add`, `git commit`, `git branch` | Features include Pull Requests, reviews, and Actions |

You can use Git without GitHub. GitHub uses Git to manage repository history.

# 3. How Git works internally

## 3.1 The three main areas

```text
+--------------------------+
| Working Directory        |
| Your actual project files|
| Create / edit / delete   |
+------------+-------------+
             |
             | git add
             v
+--------------------------+
| Staging Area (Index)     |
| Changes selected for the |
| next commit              |
+------------+-------------+
             |
             | git commit
             v
+--------------------------+
| Local Git Repository     |
| Saved commits and history|
+------------+-------------+
             |
             | git push
             v
+--------------------------+
| Remote Repository        |
| GitHub repository        |
+--------------------------+
```

### Working directory

The project folder where you create and edit files.

### Staging area

A preparation area where you select exactly which changes should go into the next commit.

### Local repository

When you commit, Git records a snapshot of the staged content in the repository history.

### Remote repository

Another copy of the repository, usually hosted on GitHub. `git push` sends local commits to a remote. `git fetch` downloads remote information without automatically integrating it into your current branch.

## 3.2 Repository

A repository (repo) is a project tracked by Git. Running `git init` creates a hidden `.git` directory containing Git metadata, objects, references, and configuration.

```bash
git init
ls -la
```

Do not manually delete or edit `.git` unless you intentionally want to remove or repair repository metadata.

## 3.3 Commit

A commit is a saved snapshot in the repository's history. It includes a unique hash, author and committer metadata, a timestamp, a message, a reference to the project tree, and parent commit(s).

A commit is not simply a copy of a folder. Git stores content-addressed objects and connects snapshots through commit history.

## 3.4 Branch

A branch is a movable reference to a commit. It provides a separate line of development without copying the whole project into a new folder.

```text
main
  |
  A---B---C
           \
            D---E  feature/login
```

## 3.5 HEAD

`HEAD` normally identifies the currently checked-out branch. In detached HEAD state, it identifies a specific commit instead.

```bash
git status
git branch --show-current
```

# 4. Install Git and configure identity

## 4.1 Ubuntu / Debian

```bash
sudo apt update
sudo apt install git
```

## 4.2 RHEL-based Linux

```bash
sudo dnf install git
```

Older distributions may use:

```bash
sudo yum install git
```

## 4.3 macOS

With Homebrew:

```bash
brew install git
```

## 4.4 Windows

Install Git for Windows from https://git-scm.com/downloads. Git Bash is useful for following the Linux-style commands in this guide.

## 4.5 Verify

```bash
git --version
```

## 4.6 Configure your commit identity

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global --list
```

Use an email associated with your GitHub account if you want GitHub to associate commits with your profile. You may also use a GitHub-provided private `noreply` email address.

`--global` applies to your user account on this computer. Without `--global`, the setting applies to the current repository.

Set a different identity for one repository:

```bash
git config user.name "Work Name"
git config user.email "work@example.com"
```

Inspect where a value comes from:

```bash
git config --show-origin --get user.email
```

**Security:** Git author email is commit metadata. Never put passwords, access tokens, or private credentials in commits or configuration files.

# 5. Create a GitHub repository

1. Open https://github.com/ and sign in or create an account.
2. Verify your email if prompted.
3. Enable multi-factor authentication.
4. Select **New repository**.
5. Name it `ShopEasy`.
6. Description: `A learning project for practising Git and GitHub team workflows.`
7. Choose Public or Private.
8. Create the repository.

For the simplest first push, create an empty repository if you already have a local project with commits. If GitHub creates a README or licence commit, the remote history is not empty; fetch and integrate that history before pushing your local history.

Never upload passwords, access keys, private certificates, customer information, or production secrets to any repository.

# 6. Practical Lab 1 — Create your first local repository

## Scenario

You have joined the development team of **ShopEasy**, a sample e-commerce application. Your first task is to create the project structure and start tracking it with Git.

### Step 1: Create the project folder

```bash
mkdir ShopEasy
cd ShopEasy
pwd
```

### Step 2: Create the structure

```bash
mkdir -p src docs tests
touch README.md
touch src/index.html
touch src/style.css
touch docs/project-notes.md
touch tests/test-plan.md
```

### Step 3: Add starter content to README.md

```markdown
# ShopEasy

ShopEasy is a learning project used to practise Git and GitHub workflows.

## Modules

- Authentication
- Product catalogue
- Shopping cart
- Payment integration

## Purpose

Learn version control, branching, collaboration, and code review.
```

Add to `src/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ShopEasy</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Welcome to ShopEasy</h1>
    </header>
    <main>
        <p>Your learning e-commerce application.</p>
    </main>
</body>
</html>
```

Add to `src/style.css`:

```css
body {
    font-family: Arial, sans-serif;
    margin: 2rem;
}

header {
    padding: 1rem;
    border-bottom: 1px solid #cccccc;
}
```

### Step 4: Initialise Git

```bash
git init
ls -la
```

Git creates the hidden `.git` directory.

### Step 5: Check status

```bash
git status
```

The files are untracked: Git can see them, but they are not included in a commit.

### Step 6: Stage the files

```bash
git add .
git status
```

The files should appear under `Changes to be committed`.

### Step 7: Commit

```bash
git commit -m "chore: initialise ShopEasy project"
```

### Step 8: Inspect history

```bash
git log
git log --oneline
```

**Expected result:** A local Git repository exists and contains its first commit.

# 7. Understand the Git file lifecycle

```text
Untracked
   | git add
   v
Staged
   | git commit
   v
Committed / tracked
   | edit a tracked file
   v
Modified
   | git add
   v
Staged again
```

| State | Meaning |
|---|---|
| Untracked | Git is not tracking the file yet |
| Modified | A tracked file changed in the working directory |
| Staged | A change has been selected for the next commit |
| Committed | The staged snapshot is saved in history |
| Ignored | A matching ignore rule hides the file from ordinary untracked status |

A file can be staged and then modified again. The staged version and the latest working copy may then be different.

# 8. Practical Lab 2 — Stage, commit, inspect, and compare

### Step 1: Modify the home page

Add this section to `src/index.html`:

```html
<section>
    <h2>Featured Products</h2>
    <p>Explore our latest products.</p>
</section>
```

### Step 2: Inspect unstaged changes

```bash
git status
git diff
```

`git diff` shows working-directory changes that have not been staged.

### Step 3: Stage only one file

```bash
git add src/index.html
git status
```

### Step 4: Inspect staged changes

```bash
git diff --staged
```

This shows what will enter the next commit.

### Step 5: Commit

```bash
git commit -m "feat: add featured products section"
```

### Step 6: Inspect history and a commit

```bash
git log --oneline --decorate
git show HEAD
```

### Step 7: Compare the latest two commits

```bash
git diff HEAD~1 HEAD
```

`HEAD~1` means the first parent of the current commit. This example assumes at least two commits exist.

Useful history commands:

```bash
git log --oneline
git log --graph --oneline --decorate --all
git log --stat
git show HEAD
```

# 9. `.gitignore` — Keep unnecessary files out of Git

Real projects contain generated files, dependencies, logs, and local configuration that should usually not be committed.

Create `.gitignore` in the repository root:

```bash
touch .gitignore
```

Example contents:

```gitignore
# Dependencies
node_modules/

# Build output
dist/
build/

# Environment files and secrets
.env
.env.*
!.env.example

# Logs
*.log
logs/

# Operating system files
.DS_Store
Thumbs.db

# Editor folders (team may choose to share selected settings)
.vscode/
.idea/
```

Commit it:

```bash
git add .gitignore
git commit -m "chore: add gitignore rules"
```

**Important:** `.gitignore` does not remove files already tracked by Git.

To stop tracking a file but keep the local copy:

```bash
git rm --cached path/to/file
git commit -m "chore: stop tracking local file"
```

If a credential was committed, treat it as exposed and revoke or rotate it immediately. A later deletion does not remove it from earlier history. History cleanup may also be required.

# 10. Connect local Git to GitHub

A remote is a named reference to another repository. `origin` is the common default name.

### Step 1: Create an empty GitHub repository named ShopEasy

Copy its HTTPS URL.

### Step 2: Add the remote

```bash
git remote add origin https://github.com/YOUR-USERNAME/ShopEasy.git
```

Replace `YOUR-USERNAME`.

### Step 3: Inspect remotes

```bash
git remote -v
```

### Step 4: Check and name the branch main

```bash
git branch --show-current
git branch -M main
```

### Step 5: Push

```bash
git push -u origin main
```

- `git push` sends commits to a remote.
- `origin` is the remote name.
- `main` is the branch.
- `-u` sets the upstream tracking relationship.

After the first successful upstream setup, `git push` is often sufficient.

## Authentication

GitHub does not accept an account password for ordinary Git-over-HTTPS authentication. Common methods include GitHub CLI, a personal access token stored securely through a credential manager, or SSH keys.

GitHub CLI: https://cli.github.com/

Never paste tokens into source files, commits, screenshots, or shared messages.

# 11. Practical Lab 3 — Push, fetch, pull, and clone

## Push

```bash
git push
```

## Clone

Clone creates a local copy of a remote repository, including its history:

```bash
cd ..
git clone https://github.com/YOUR-USERNAME/ShopEasy.git
cd ShopEasy
git status
git remote -v
git branch -a
```

## Fetch

```bash
git fetch origin
git branch -r
```

Fetch downloads remote updates and remote-tracking references. It does not automatically merge those changes into the current branch.

## Pull

```bash
git pull
```

Pull fetches and then integrates remote changes. Depending on configuration, integration may use merge or rebase.

An explicit merge-style pull:

```bash
git pull --no-rebase origin main
```

Check local work before pulling:

```bash
git status
```

Commit or stash local changes if necessary.

## Fetch vs pull

| Fetch | Pull |
|---|---|
| Downloads remote updates | Downloads and integrates remote updates |
| Does not update the current branch by itself | Can update current branch and working tree |
| Good for inspecting first | Good when ready to integrate |

### Simulate a remote change

1. Open ShopEasy on GitHub.
2. Edit `README.md` in the browser.
3. Add a sentence and commit the change on GitHub.
4. In your terminal:

```bash
git fetch origin
git log --oneline --decorate --all
```

5. Integrate the remote change:

```bash
git pull --no-rebase origin main
```

6. Verify:

```bash
git status
git log --oneline --decorate -5
```

# 12. Branching fundamentals

## Why branches?

Suppose three developers are working on ShopEasy:

- Developer A builds authentication.
- Developer B builds the product catalogue.
- Developer C builds the shopping cart.

If all unfinished work is committed directly to `main`, changes can interfere with one another. Branches isolate work.

## Common branch names

| Branch | Typical purpose |
|---|---|
| `main` | Stable, reviewed, releasable code |
| `feature/...` | New feature |
| `bugfix/...` | Defect correction |
| `hotfix/...` | Urgent production fix |
| `release/...` | Release preparation in teams that use release branches |

Examples:

```text
main
feature/user-authentication
feature/product-catalogue
feature/shopping-cart
bugfix/login-validation
hotfix/payment-timeout
```

## Create a branch

```bash
git branch feature/user-authentication
```

This creates but does not switch to the branch.

Switch:

```bash
git switch feature/user-authentication
```

Create and switch in one step:

```bash
git switch -c feature/user-authentication
```

Older equivalent:

```bash
git checkout -b feature/user-authentication
```

## List branches

```bash
git branch
git branch -a
```

The `*` marks the current local branch.

## Switch branches

```bash
git switch main
```

## Rename a branch

Rename current branch:

```bash
git branch -m new-branch-name
```

Rename another local branch:

```bash
git branch -m old-name new-name
```

## Delete a local branch

After it has been merged:

```bash
git branch -d feature/user-authentication
```

Force-delete an unmerged branch:

```bash
git branch -D feature/user-authentication
```

`-D` can discard unmerged work. Use it only when you are certain the work is no longer needed.

## Push a new branch to GitHub

```bash
git push -u origin feature/user-authentication
```

# 13. Practical Lab 4 — Build features using branches

**Scenario:** ShopEasy needs authentication, a product catalogue, and a shopping cart. You will create a branch for each feature.

## Feature 1 — Authentication

Start from updated main:

```bash
git switch main
git pull --no-rebase origin main
```

Create the branch:

```bash
git switch -c feature/user-authentication
```

Create files:

```bash
mkdir -p src/auth
touch src/auth/login.html
```

Add sample HTML:

```html
<h2>ShopEasy Login</h2>
<form>
    <label>Email</label>
    <input type="email" name="email">
    <label>Password</label>
    <input type="password" name="password">
    <button type="submit">Login</button>
</form>
```

Commit and push:

```bash
git add src/auth/login.html
git commit -m "feat: add login page"
git push -u origin feature/user-authentication
```

## Feature 2 — Product catalogue

```bash
git switch main
git pull --no-rebase origin main
git switch -c feature/product-catalogue
mkdir -p src/products
touch src/products/products.html
```

Add:

```html
<h2>Product Catalogue</h2>
<ul>
    <li>Laptop</li>
    <li>Keyboard</li>
    <li>Mouse</li>
</ul>
```

Commit and push:

```bash
git add src/products/products.html
git commit -m "feat: add product catalogue page"
git push -u origin feature/product-catalogue
```

## Feature 3 — Shopping cart

```bash
git switch main
git pull --no-rebase origin main
git switch -c feature/shopping-cart
mkdir -p src/cart
touch src/cart/cart.html
```

Add:

```html
<h2>Shopping Cart</h2>
<p>Your selected products will appear here.</p>
```

Commit and push:

```bash
git add src/cart/cart.html
git commit -m "feat: add shopping cart page"
git push -u origin feature/shopping-cart
```

Inspect all branches and history:

```bash
git branch -a
git log --graph --oneline --decorate --all
```

## Merge a feature locally (learning exercise)

If direct pushes to main are permitted in this personal lab:

```bash
git switch main
git merge --no-ff feature/user-authentication
git push origin main
```

`--no-ff` requests a merge commit even when Git could fast-forward.

In many professional teams, `main` is protected. Developers use Pull Requests instead of pushing directly to it. Practise that workflow in Section 17.

After confirming a branch has been merged:

```bash
git branch -d feature/user-authentication
```

Remove its remote branch if it was not automatically deleted:

```bash
git push origin --delete feature/user-authentication
```

# 14. Merge strategies

## Fast-forward

If the target branch has not moved since the feature branch was created, Git may move the target branch pointer forward:

```text
A---B  main
     \
      C---D  feature

After:
A---B---C---D  main
```

## Three-way merge

If both branches have new commits, Git uses a common ancestor and both branch tips to create a merge result. A merge commit may be created.

## No-fast-forward

```bash
git merge --no-ff feature/example
```

This requests a merge commit even if a fast-forward would be possible.

## Squash merge

A squash merge combines a branch's changes into one new commit on the target branch. The branch's individual commits are not preserved as separate commits on the target branch.

Command-line example:

```bash
git switch main
git merge --squash feature/example
git commit -m "feat: add example feature"
```

GitHub also offers **Squash and merge** when enabled.

No single strategy is best for every team. Follow the repository's contribution policy.

# 15. What is a merge conflict?

A merge conflict occurs when Git cannot automatically combine changes. For example, two developers may change the same line in the same file differently.

A conflicted file can contain:

```text
<<<<<<< HEAD
Content from the current branch
=======
Content from the incoming branch
>>>>>>> feature/branch-name
```

- `<<<<<<< HEAD` begins the current branch's version.
- `=======` separates the versions.
- `>>>>>>>` marks the incoming branch's version.

Edit the file, choose the correct final content, and remove all conflict markers. A conflict is not necessarily a Git failure; it requires a human decision.

# 16. Practical Lab 5 — Deliberately create and resolve a conflict

**Objective:** Create a real conflict so you understand how to resolve one.

### Step 1: Start from main

```bash
git switch main
git status
```

Make sure your working tree is clean.

### Step 2: Create the first branch and file

```bash
git switch -c lab/conflict-main
```

Create `conflict-demo.txt` containing:

```text
ShopEasy welcome message: Welcome customers!
```

Commit:

```bash
git add conflict-demo.txt
git commit -m "test: add initial conflict demo file"
```

### Step 3: Create another branch from the same base

```bash
git switch -c lab/conflict-feature
```

Change the same line to:

```text
ShopEasy welcome message: Hello shoppers!
```

Commit:

```bash
git add conflict-demo.txt
git commit -m "test: update welcome message from feature"
```

### Step 4: Return to the first branch

```bash
git switch lab/conflict-main
```

Change the same line differently:

```text
ShopEasy welcome message: Welcome to our store!
```

Commit:

```bash
git add conflict-demo.txt
git commit -m "test: update welcome message from main"
```

### Step 5: Merge the feature branch

```bash
git merge lab/conflict-feature
```

Git should report a conflict because both branches changed the same line differently.

### Step 6: Inspect

```bash
git status
```

Open `conflict-demo.txt`. You should see conflict markers.

### Step 7: Resolve manually

Choose a final version, for example:

```text
ShopEasy welcome message: Welcome to ShopEasy — happy shopping!
```

Remove all conflict markers and save.

### Step 8: Stage the resolved file

```bash
git add conflict-demo.txt
```

### Step 9: Complete the merge

```bash
git commit -m "merge: resolve welcome message conflict"
```

### Step 10: Verify

```bash
git status
git log --graph --oneline --decorate --all
cat conflict-demo.txt
```

**Expected result:** The merge is complete and the file contains only the final selected content.

To cancel a merge that is still in progress:

```bash
git merge --abort
```

Inspect `git status` first. This returns the working tree to the pre-merge state where possible.

# 17. Pull Requests and code review

## What is a Pull Request?

A Pull Request (PR) is a GitHub collaboration feature that proposes integrating changes from one branch into another.

```text
feature/product-catalogue
          |
          | Pull Request
          v
         main
```

A PR allows a team to:

- Review code changes.
- Discuss implementation.
- Request improvements.
- Run automated checks.
- Approve work.
- Merge after required conditions are met.

Opening a PR does not itself create a Git commit. It is a collaboration and review process around proposed changes.

## Typical PR lifecycle

```text
Create feature branch
        |
        v
Make code changes
        |
        v
Commit changes
        |
        v
Push branch to GitHub
        |
        v
Open Pull Request
        |
        v
Review comments and checks
        |
        v
Make requested changes
        |
        v
Approval
        |
        v
Merge Pull Request
        |
        v
Delete feature branch (optional)
```

## What a good PR should contain

- Clear title.
- Summary of what changed.
- Reason for the change.
- How it was tested.
- Screenshots for UI changes, where useful.
- Related issue or ticket, if applicable.
- Known risks or limitations.

Example title:

```text
feat: add product catalogue page
```

Example description:

```markdown
## Summary
- Added the ShopEasy product catalogue page.
- Added sample product entries.

## Testing
- Opened the page in a browser.
- Verified that sample products are visible.

## Notes
- Product data is currently static.
```

# 18. Practical Lab 6 — Complete GitHub Pull Request workflow

**Objective:** Create a feature branch, push it, open a PR, review it, merge it, and clean up.

### Step 1: Update main

```bash
git switch main
git pull --no-rebase origin main
```

### Step 2: Create a branch

```bash
git switch -c feature/product-search
```

### Step 3: Add the feature

```bash
mkdir -p src/search
touch src/search/search.html
```

Add:

```html
<section>
    <h2>Search Products</h2>
    <form>
        <label for="search">Product name</label>
        <input id="search" type="search" placeholder="Search products">
        <button type="submit">Search</button>
    </form>
</section>
```

### Step 4: Review, stage, and commit

```bash
git status
git diff
git add src/search/search.html
git diff --staged
git commit -m "feat: add product search page"
```

### Step 5: Push branch

```bash
git push -u origin feature/product-search
```

### Step 6: Open GitHub

1. Open the ShopEasy repository.
2. Select **Compare & pull request** if GitHub shows the prompt, or open the Pull Requests tab and select **New pull request**.
3. Set base branch to `main`.
4. Set compare branch to `feature/product-search`.
5. Add a clear title and description.
6. Select **Create pull request**.

### Step 7: Review

1. Open the **Files changed** tab.
2. Read the diff.
3. Check whether the code meets the requirement.
4. Add comments if changes are needed.
5. Approve if you have reviewer permission and the change is acceptable.
6. Inspect automated checks if configured.

For practice, ask a classmate or use a second GitHub account as reviewer. Reviewing your own work is useful for learning the interface but does not replace independent review in a real team.

### Step 8: Merge

When approvals and required checks are satisfied, select the merge option allowed by the repository and confirm.

### Step 9: Update local main

```bash
git switch main
git pull --no-rebase origin main
```

### Step 10: Clean up

```bash
git branch -d feature/product-search
git push origin --delete feature/product-search
```

The remote branch may already have been deleted automatically by GitHub. If so, the remote-delete command may report that it does not exist.

Verify:

```bash
git branch -a
git log --graph --oneline --decorate --all
```

**Expected result:** The feature is merged into `main`, and the PR records the discussion, review, checks, and merge.

# 19. Collaboration: collaborators, forks, origin, and upstream

## 19.1 Collaborator model

A repository owner grants another developer access to the same repository.

```text
Company GitHub repository
       |
       | clone
       v
Developer local repository
       |
       | feature branch + commits
       v
Push branch to company repository
       |
       v
Pull Request
       |
       v
Review and merge
```

## 19.2 Fork model

A fork is a separate GitHub repository under another user's account. It is commonly used for open-source contributions when a contributor does not have direct write access to the original repository.

```text
Original repository
       |
       | Fork on GitHub
       v
Contributor's fork
       |
       | clone
       v
Contributor's computer
       |
       | branch, commit, push
       v
Contributor's fork
       |
       | Pull Request
       v
Original repository
```

## 19.3 Fork workflow

1. Open a public repository that welcomes contributions.
2. Select **Fork**.
3. Clone your fork:

```bash
git clone https://github.com/YOUR-USERNAME/REPOSITORY.git
cd REPOSITORY
```

4. Add the original repository as `upstream`:

```bash
git remote add upstream https://github.com/ORIGINAL-OWNER/REPOSITORY.git
```

5. Inspect remotes:

```bash
git remote -v
```

Usually:
- `origin` points to your fork.
- `upstream` points to the original project.

6. Create a branch:

```bash
git switch -c docs/improve-readme
```

7. Make a small permitted documentation improvement.
8. Commit and push:

```bash
git add README.md
git commit -m "docs: improve README explanation"
git push -u origin docs/improve-readme
```

9. Open a Pull Request from your fork's branch to the original repository.

## 19.4 Sync a fork

```bash
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```

The original project may use a different default branch name. Check it before running these commands.

# 20. Undoing mistakes safely

Before undoing anything, inspect:

```bash
git status
git log --oneline --decorate -5
```

## 20.1 Discard unstaged changes in one file

```bash
git restore src/index.html
```

This replaces the working copy with the version in the index (or current commit if not staged). Uncommitted edits in that file are lost.

## 20.2 Unstage a file but keep its edits

```bash
git restore --staged src/index.html
```

## 20.3 Discard staged and unstaged changes in one file

```bash
git restore --source=HEAD --staged --worktree src/index.html
```

This is destructive for that file. Use only when you intend to discard both staged and unstaged changes.

## 20.4 Revert a commit

`git revert` creates a new commit that reverses the changes from an earlier commit:

```bash
git revert COMMIT_HASH
```

Replace the placeholder with a real hash. This is usually safer for undoing a commit already pushed to a shared branch.

## 20.5 Reset

`git reset` moves the current branch reference and changes the staging area and/or working directory depending on the mode.

Keep changes staged:

```bash
git reset --soft HEAD~1
```

Keep changes in working directory but unstaged:

```bash
git reset --mixed HEAD~1
```

Discard affected staged and working-tree changes:

```bash
git reset --hard HEAD~1
```

**Warning:** `--hard` can permanently discard uncommitted work. Avoid rewriting shared branch history unless the team has coordinated it.

## Revert vs reset

| Revert | Reset |
|---|---|
| Creates a new commit reversing earlier changes | Moves the current branch reference |
| Preserves shared history | Can rewrite local history |
| Often preferred for shared branches | Commonly used for unpublished local history |
| Safer for already-pushed changes | Risky on branches used by others |

# 21. Stash — temporarily save unfinished work

Suppose you are editing a product page, but you must switch to a bug fix before your feature is ready to commit.

### Step 1: Make an uncommitted edit

Edit a tracked file.

### Step 2: Stash it

```bash
git stash push -m "work in progress: homepage"
```

### Step 3: List stashes

```bash
git stash list
```

### Step 4: Switch tasks

```bash
git switch main
```

### Step 5: Return to your feature branch

```bash
git switch feature/some-feature
```

### Step 6: Reapply the changes

```bash
git stash apply
```

Apply and remove the stash entry:

```bash
git stash pop
```

Delete one stash:

```bash
git stash drop stash@{0}
```

Delete all stashes:

```bash
git stash clear
```

`stash clear` is destructive. Stash is for temporary work, not a replacement for meaningful commits.

# 22. Cherry-pick — apply a specific commit

Sometimes you need one commit from another branch without merging the entire branch.

Find a commit:

```bash
git log --oneline feature/example
```

Switch to the branch that should receive it:

```bash
git switch main
```

Apply it:

```bash
git cherry-pick COMMIT_HASH
```

Replace the placeholder with the actual hash. Cherry-pick creates a new commit on the current branch with the selected change. If the same change is later merged through its original branch, history can become confusing. Use deliberately and coordinate with the team.

If conflicts occur:

```bash
git status
# Resolve the files
git add .
git cherry-pick --continue
```

Cancel:

```bash
git cherry-pick --abort
```

# 23. Tags and releases

A tag marks a point in repository history. Teams often use tags to identify releases.

Examples:

```text
v1.0.0
v1.1.0
v2.0.0
```

Create a lightweight tag:

```bash
git tag v1.0.0
```

Create an annotated tag:

```bash
git tag -a v1.0.0 -m "ShopEasy version 1.0.0"
```

Annotated tags include metadata and are commonly used for releases.

List and inspect:

```bash
git tag
git show v1.0.0
```

Push one tag:

```bash
git push origin v1.0.0
```

Push all tags:

```bash
git push origin --tags
```

Delete local tag:

```bash
git tag -d v1.0.0
```

Delete remote tag:

```bash
git push origin --delete v1.0.0
```

A release tag should identify a reviewed and tested application state.

# 24. Repository protection and GitHub Actions

## 24.1 Branch protection

GitHub repository rules can help protect important branches. Depending on repository, organisation, and plan, settings may include:

- Require Pull Requests before merging.
- Require one or more approvals.
- Require status checks to pass.
- Require branches to be up to date.
- Restrict who can push.
- Require conversations to be resolved.
- Prevent force pushes.
- Prevent branch deletion.

Suggested learning setup:

1. Open ShopEasy on GitHub.
2. Open **Settings**.
3. Find **Branches** or the ruleset settings.
4. Create a rule for `main`.
5. Where available, require Pull Requests before merging.
6. Require at least one approval for a team exercise.
7. Require CI status checks after configuring them.
8. Prevent force pushes to `main`.
9. Save the rule.

The interface and available settings vary. Some protections depend on repository visibility, plan, and organisation policy.

**Industry principle:** Developers generally create feature branches and use Pull Requests instead of directly changing a protected `main` branch.

## 24.2 First GitHub Actions CI workflow

GitHub Actions can run jobs when events happen, such as a push or Pull Request. This is useful for Continuous Integration (CI), where automated checks run before changes are merged.

Create the workflow directory:

```bash
mkdir -p .github/workflows
```

Create `.github/workflows/basic-check.yml`:

```yaml
name: Basic Repository Check

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  repository-check:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Show repository files
        run: ls -la

      - name: Validate required files
        run: |
          test -f README.md
          test -f src/index.html
          echo "Required files are present."
```

Commit and push:

```bash
git add .github/workflows/basic-check.yml
git commit -m "ci: add basic GitHub Actions check"
git push
```

If working on a feature branch, push that branch and open a PR.

View the result:

1. Open the repository on GitHub.
2. Select **Actions**.
3. Open the workflow run.
4. Inspect job and step logs.
5. Confirm whether it passed or failed.

A failed required status check can prevent a protected branch from accepting a PR.

**Security:** Workflows execute code. Review third-party actions and permissions. Never print secrets in logs. Do not expose write permissions to untrusted Pull Request code.

# 25. How a team works on a feature in industry

## Example team

| Role | Responsibility |
|---|---|
| Developer 1 | Authentication |
| Developer 2 | Product catalogue |
| Developer 3 | Shopping cart |
| Reviewer | Reviews code and provides feedback |
| Maintainer / Tech lead | Ensures quality and merges changes |

## Typical daily workflow

### Step 1: Start from latest main

```bash
git switch main
git pull --no-rebase origin main
```

### Step 2: Create a branch for your ticket

```bash
git switch -c feature/SHOP-101-login
```

Ticket numbers help connect code changes to work tracking.

### Step 3: Develop and test

Edit the required files and run relevant tests locally.

### Step 4: Inspect changes

```bash
git status
git diff
```

### Step 5: Stage and commit

```bash
git add src/auth/
git diff --staged
git commit -m "feat: add login form"
```

### Step 6: Push branch

```bash
git push -u origin feature/SHOP-101-login
```

### Step 7: Open PR

Set base to `main` and compare to `feature/SHOP-101-login`. Describe what changed and how it was tested.

### Step 8: Respond to review

If changes are requested, update the same branch:

```bash
git add .
git commit -m "fix: address login review feedback"
git push
```

The open PR updates automatically when you push to its source branch.

### Step 9: Wait for checks and approval

Review test results and required approvals.

### Step 10: Merge

The maintainer merges using the repository's agreed strategy.

### Step 11: Update local main

```bash
git switch main
git pull --no-rebase origin main
```

### Step 12: Delete completed branch

```bash
git branch -d feature/SHOP-101-login
git push origin --delete feature/SHOP-101-login
```

This is a common feature-branch workflow, not the only workflow. Some teams use trunk-based development with short-lived branches and frequent integration. Follow the conventions of the team you join.

# 26. Troubleshooting common Git problems

## `git: command not found`

Git may not be installed or may not be in `PATH`.

```bash
command -v git
git --version
```

Install Git using Section 4.

## `Author identity unknown`

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

## `fatal: not a git repository`

You are not inside a repository:

```bash
pwd
ls -la
cd /path/to/ShopEasy
```

## `remote origin already exists`

Inspect:

```bash
git remote -v
```

Correct the URL if required:

```bash
git remote set-url origin https://github.com/YOUR-USERNAME/ShopEasy.git
```

## `src refspec main does not match any`

Common causes: no commit exists, the branch has another name, or you are pushing the wrong branch.

```bash
git status
git branch --show-current
git log --oneline
```

Create a commit if needed, then push the correct branch.

## `rejected — non-fast-forward`

The remote branch contains commits that your local branch does not have.

```bash
git fetch origin
git log --oneline --graph --decorate --all
```

If appropriate, integrate the remote work:

```bash
git pull --no-rebase origin main
```

Resolve conflicts if any, then push. Do not immediately force-push; first understand why the remote branch moved.

## Authentication failed

Check the GitHub account, credential manager, token validity and access, or SSH key configuration.

With GitHub CLI installed:

```bash
gh auth login
```

## Merge conflict

```bash
git status
```

Edit conflicted files, remove conflict markers, then:

```bash
git add .
git commit
```

Cancel a merge in progress:

```bash
git merge --abort
```

## Accidentally committed a file

If it is safe to amend the latest commit before pushing:

```bash
git rm --cached path/to/file
git commit --amend
```

If it contained a secret, revoke or rotate it immediately. Removing it from the latest commit does not remove it from earlier history.

## Detached HEAD

Check:

```bash
git status
```

If you made work worth keeping, create a branch at the current commit:

```bash
git switch -c rescue/my-work
```

# 27. Professional Git best practices

1. Fetch or pull the latest changes before starting new work.
2. Create a branch for a feature, bug fix, or task.
3. Keep branches focused and reasonably short-lived.
4. Make small, meaningful commits.
5. Write clear commit messages.
6. Review `git diff` before staging and committing.
7. Never commit passwords, tokens, private keys, or `.env` secrets.
8. Use `.gitignore` for generated and local-only files.
9. Open Pull Requests for review where required.
10. Run tests before requesting review.
11. Respond to review comments on the same PR branch.
12. Avoid force-pushing shared branches.
13. Protect important branches such as `main`.
14. Delete merged feature branches when appropriate.
15. Tag releases and document important changes.
16. Keep the README updated.
17. Use least privilege for repository permissions and automation.
18. Do not use `git reset --hard`, `git clean -fd`, or force push without understanding the impact.

# 28. Final end-to-end team simulation

## Scenario

You are Developer 1 at ShopEasy. Your ticket is:

**SHOP-201 — Add a customer registration page.**

A second developer is responsible for the product catalogue. A reviewer will inspect your Pull Request.

## Part A — Prepare the repository

1. Create a GitHub repository named `ShopEasy-Team-Lab`.
2. Clone it locally.
3. Add a README describing the project.
4. Create `src`, `docs`, and `tests`.
5. Commit the initial structure.
6. Push `main`.

```bash
git clone REPOSITORY_URL
cd ShopEasy-Team-Lab
git status
git add .
git commit -m "chore: initialise team lab"
git push
```

## Part B — Build your feature

Create a branch:

```bash
git switch -c feature/SHOP-201-registration
```

Create `src/auth/register.html` with:

- Full name.
- Email.
- Password.
- Confirm password.
- Submit button.

Review and commit:

```bash
git status
git diff
git add src/auth/register.html
git diff --staged
git commit -m "feat: add customer registration page"
```

Push:

```bash
git push -u origin feature/SHOP-201-registration
```

## Part C — Pull Request

1. Open a PR from `feature/SHOP-201-registration` into `main`.
2. Add a meaningful description.
3. Ask a reviewer to inspect it.
4. Address review feedback with another commit.
5. Merge when approved and checks pass.

## Part D — Parallel development

1. Have a second developer clone the repository.
2. Ask them to create `feature/SHOP-202-products`.
3. Add a product page and push the branch.
4. Open a separate PR.
5. Review and merge it.

## Part E — Conflict simulation

1. Both developers edit the same line in `README.md` on separate branches.
2. Commit each change.
3. Merge one branch.
4. Integrate the second branch.
5. Resolve the conflict.
6. Verify the final README and commit history.

## Part F — Release a version

After changes are reviewed and merged:

```bash
git switch main
git pull --no-rebase origin main
git tag -a v1.0.0 -m "ShopEasy team lab release 1.0.0"
git push origin v1.0.0
```

On GitHub, inspect the tag and optionally create a Release from it.

## Submission checklist

- [ ] Git installed and configured.
- [ ] GitHub repository created.
- [ ] Local repository connected to GitHub.
- [ ] Initial commit pushed.
- [ ] Feature branch created.
- [ ] Feature changes committed.
- [ ] Feature branch pushed.
- [ ] Pull Request created.
- [ ] Code review completed.
- [ ] Review feedback addressed.
- [ ] PR merged.
- [ ] Local main updated.
- [ ] Feature branch cleaned up.
- [ ] Merge conflict created and resolved.
- [ ] Release tag created and pushed.
- [ ] README documents the project and workflow.

# 29. Practice exercises

## Beginner

1. Explain the difference between Git and GitHub.
2. Create a repository called `Git-Practice`.
3. Add a README and commit it.
4. Make a second commit.
5. Display commit history.
6. Create a `.gitignore`.
7. Push the repository to GitHub.

## Intermediate

1. Create `feature/about-page`.
2. Add an About page.
3. Commit and push the branch.
4. Create a Pull Request.
5. Merge it.
6. Delete the feature branch.
7. Clone the repository into another folder.
8. Make a change in the cloned copy and push it.
9. Fetch and pull the change into your original copy.

## Advanced

1. Create two branches that edit the same line.
2. Cause and resolve a merge conflict.
3. Use `git stash` to save unfinished work.
4. Use `git revert` to reverse a test commit.
5. Use `git cherry-pick` to apply one commit to another branch.
6. Create and push an annotated tag.
7. Add a basic GitHub Actions workflow.
8. Configure branch protection where available.
9. Explain the team's Pull Request workflow in your own words.

# 30. Interview questions and answers

**1. What is Git?**  
A distributed version control system used to track file changes and coordinate development.

**2. What is GitHub?**  
A platform for hosting Git repositories and collaborating through Pull Requests, Issues, reviews, and automation.

**3. What is a repository?**  
A project tracked by Git, including its files and version history.

**4. What is a commit?**  
A saved snapshot of staged changes in repository history.

**5. What is a branch?**  
A movable reference to a commit that provides a separate line of development.

**6. What is the staging area?**  
The area where you select changes for the next commit.

**7. Difference between `git add` and `git commit`?**  
`git add` stages changes; `git commit` saves the staged snapshot in local history.

**8. Difference between `git fetch` and `git pull`?**  
Fetch downloads remote information without integrating it into the current branch. Pull fetches and then integrates remote changes.

**9. Difference between `git clone` and `git pull`?**  
Clone creates a local copy of a remote repository. Pull updates the current local branch by fetching and integrating remote changes.

**10. What is a merge conflict?**  
A situation where Git cannot automatically combine changes, often because branches changed the same lines differently.

**11. What is a Pull Request?**  
A GitHub proposal to integrate changes from one branch into another, with discussion, review, and checks.

**12. Difference between merge and rebase?**  
Merge integrates histories and may create a merge commit. Rebase replays commits onto a new base and rewrites commit identities. Teams should agree on rebase rules, especially for shared branches.

**13. What is `.gitignore`?**  
A file containing patterns for files Git should ignore when reporting untracked files. It does not remove already tracked files.

**14. What is `git stash`?**  
A way to temporarily store local changes so you can switch tasks or branches.

**15. What is `git revert`?**  
It creates a new commit that reverses changes from an earlier commit.

**16. What is `git reset`?**  
It moves the current branch reference and can also change the index and working directory depending on its mode.

**17. What is cherry-pick?**  
It applies the changes from a selected commit onto the current branch.

**18. What is a tag?**  
A reference marking a specific point in repository history, often used for releases.

**19. What is a remote?**  
A named reference to another repository, often hosted on GitHub.

**20. Difference between origin and upstream?**  
`origin` commonly points to the repository you cloned or your fork. `upstream` commonly points to the original repository from which a fork was created. These are conventions, not reserved names.

**21. Why use feature branches?**  
They isolate work, reduce interference, and allow review before integration.

**22. Why avoid direct pushes to main?**  
Pull Requests and branch protections help ensure changes are reviewed and tested before reaching a stable branch.

**23. What is CI?**  
Continuous Integration is the practice of integrating changes frequently and automatically running checks such as builds and tests.

**24. What should you do if you commit a secret?**  
Revoke or rotate it immediately. Then remove it and coordinate history cleanup if required. A later deletion does not erase the secret from earlier commits.

# 31. Git command cheat sheet

## Setup

```bash
git --version
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global --list
```

## Repository

```bash
git init
git clone REPOSITORY_URL
git status
```

## Stage and commit

```bash
git add filename
git add .
git diff
git diff --staged
git commit -m "message"
git log --oneline
git show HEAD
```

## Remote operations

```bash
git remote -v
git remote add origin REPOSITORY_URL
git remote set-url origin REPOSITORY_URL
git fetch origin
git pull
git push
git push -u origin main
```

## Branching

```bash
git branch
git branch -a
git switch main
git switch -c feature/example
git branch -m new-name
git merge feature/example
git merge --no-ff feature/example
git branch -d feature/example
git push origin --delete feature/example
```

## Conflict resolution

```bash
git status
# Edit conflicted files and remove conflict markers
git add .
git commit
git merge --abort
```

## Undo and temporary work

```bash
git restore filename
git restore --staged filename
git revert COMMIT_HASH
git reset --soft HEAD~1
git reset --mixed HEAD~1
git stash push -m "work in progress"
git stash list
git stash apply
git stash pop
```

## Advanced

```bash
git cherry-pick COMMIT_HASH
git tag
git tag -a v1.0.0 -m "Release 1.0.0"
git push origin v1.0.0
git log --graph --oneline --decorate --all
```

---

# 32. Final learning advice

Git becomes easier when you practise it as a daily workflow instead of memorising commands.

For every change, ask:

1. What am I changing?
2. Which branch should contain this work?
3. Have I reviewed the diff?
4. Have I staged only the files I intend to commit?
5. Does my commit message explain the change?
6. Have I pushed the correct branch?
7. Has the Pull Request been reviewed and tested?
8. Is the final change safely merged?

**The most important industry habit:** Do not treat Git as just a way to upload code. Git is the history and collaboration system of a software project. Learn to understand history, communicate through commits and Pull Requests, and protect the quality of shared branches.

---

**End of Module — Git and GitHub Industry-Style Practical Lab**
