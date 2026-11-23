<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/6f3d8b67-0474-45be-9e54-94af3da63b88" />

# DevOps Series – Module 01: Git Introduction

**From Zero to Production**  
**Day 01 – Introduction to Version Control and Git**  
**Trainer / Notes:** Sahil Shaikh

> These notes introduce Version Control, Git, GitHub, Git architecture, basic workflow, and how Git fits into real-world DevOps work. The diagrams below are image-based so they display in GitHub Markdown without Mermaid support.

![Git Introduction – complete visual overview](images/git-introduction-infographic.png)

---

## Learning Objectives

By the end of this lesson, students should be able to:

1. Explain Version Control and why it is needed.
2. Describe the problems developers face without version control.
3. Identify the three common approaches to version control.
4. Explain what Git is and why it is called distributed.
5. Explain what GitHub is and how it differs from Git.
6. Describe Git's working directory, staging area, local repository, and remote repository.
7. Understand common Git terminology.
8. Explain how Git supports modern DevOps workflows.

---

# Part 1 – Understanding Version Control

## 1. What is Version Control?

Imagine that you are developing a website for a client. You create a file named `index.html`. Later, you change the design, add new features, and fix bugs. Then the new version breaks something that worked earlier.

You want to return to the earlier working version. If you have manually saved copies, your folder may look like this:

```text
index.html
index_old.html
index_final.html
index_final_latest.html
index_final_latest_new.html
index_final_really_final.html
```

This creates several problems:

- Which file is the latest version?
- Which version was working correctly?
- Who changed the file?
- What exactly changed?
- How can you restore an older version?
- How can several developers safely work on the same project?

A **Version Control System (VCS)** is a tool or system that records changes to files over time. It helps people manage different versions of a project, compare changes, restore earlier versions, and collaborate.

**In simple words:** Version Control is like a time machine for your project. It helps you understand what changed, when it changed, and who made the change.

### Why do we need Version Control?

Consider a company where five developers are working on one e-commerce application:

- Developer 1 works on authentication.
- Developer 2 works on the product page.
- Developer 3 works on payment integration.
- Developer 4 works on the shopping cart.
- Developer 5 works on deployment configuration.

Without version control, sharing and combining their work becomes difficult. Version control gives the team a structured way to track, review, and integrate changes.

### Major benefits

| Benefit | Explanation |
|---|---|
| Change tracking | Records modifications made to project files. |
| Version history | Keeps a history of committed project versions. |
| Collaboration | Helps multiple developers contribute to one project. |
| Recovery | Makes it possible to return to an earlier committed state. |
| Branching | Allows work on features or fixes separately. |
| Code review | Supports reviewing proposed changes before integration. |
| Accountability | Records author and commit metadata. |
| Traceability | Helps investigate when a change was introduced. |

### A real-world picture of developers collaborating

![Developers collaborating around computers](https://images.unsplash.com/photo-1521737711867-e3b97375f902?auto=format&fit=crop&w=1400&q=85)

*Image: A team collaborating on a shared software project.*

---

## 2. Types of Version Control Systems

There are three commonly discussed approaches: local, centralized, and distributed version control.

### 2.1 Local Version Control System

In local version control, versions are maintained on one computer. A local database or tool tracks changes.

**How it works:**

1. A developer creates project files.
2. The local version control tool records versions.
3. The developer can inspect or restore earlier versions.
4. Other developers do not automatically share that local history.

**Advantages:**

- Simple to understand.
- Works without a network.
- Useful for an individual working on one machine.

**Limitations:**

- Collaboration is not built in.
- If the machine and its backups are lost, the history may be lost.
- Sharing changes requires another process.

### 2.2 Centralized Version Control System (CVCS)

In centralized version control, a central server stores the repository. Developers connect to that server to obtain files and submit changes.

Examples include Apache Subversion (SVN) and Perforce in centralized workflows.

**How it works:**

1. The organization maintains a central repository server.
2. Developers connect to the server.
3. Developers check out or update files.
4. They make changes in their working copies.
5. They commit changes back to the central server.

**Advantages:**

- Central place to manage repository data.
- Centralized access control.
- A shared source of truth for the team.

**Limitations:**

- Server availability is important.
- Network connectivity is generally needed for server operations.
- A server outage can interrupt team collaboration.

### 2.3 Distributed Version Control System (DVCS)

In distributed version control, each clone can contain a complete copy of the repository and its history. Git is a distributed version control system.

**How it works:**

1. A developer clones a repository.
2. Git downloads the project and its history.
3. The developer works locally.
4. The developer creates local commits.
5. Changes can be pushed to a shared remote repository.
6. Other developers can fetch or pull those changes.

**Advantages:**

- Most everyday operations work offline.
- Developers can commit locally.
- Each clone has repository history.
- Branching and merging are powerful.
- Developers are not required to contact a central server for every operation.

**Important:** Git is distributed, but teams commonly use a shared remote repository on GitHub, GitLab, or Bitbucket as their collaboration point.

### Comparison

| Feature | Local VCS | Centralized VCS | Distributed VCS |
|---|---|---|---|
| Main storage | One computer | Central server | Repository copies across machines |
| Collaboration | Limited/manual | Through central server | Through shared remotes and exchanges |
| Offline commits | Depends on tool | Usually limited | Supported by Git |
| Central server dependency | No | High | Not required for local work |
| Example | Local revision tools | SVN | Git |

---

# Part 2 – Introduction to Git

## 3. What is Git?

Git is a free, open-source, distributed version control system used to track changes in files, especially source code. It helps developers maintain project history, work in branches, compare changes, and collaborate.

Git was created by Linus Torvalds in 2005, initially to support Linux kernel development.

### What problems does Git solve?

- Tracks source code changes.
- Maintains committed project history.
- Allows developers to work on separate branches.
- Combines work from different developers.
- Helps identify changes that introduced bugs.
- Supports local work without an internet connection for most operations.

**Simple definition:** Git is a tool installed on your computer that helps you track and manage changes in a project.

### Key features of Git

| Feature | Meaning |
|---|---|
| Distributed | Each clone can contain the complete repository history. |
| Open source | Git is freely available and open source. |
| Fast | Many operations happen locally. |
| Branching | Supports separate lines of development. |
| Merging | Combines work from different branches. |
| Integrity | Uses object hashes to identify stored objects. |
| Offline support | Most common local operations work without internet access. |
| History | Records committed changes and their relationships. |

### Git in everyday software development

![Developer working on code at a laptop](https://images.unsplash.com/photo-1498050108023-c5249f4df085?auto=format&fit=crop&w=1400&q=85)

*Image: A developer working on a software project that can be managed with Git.*

---

## 4. Git and GitHub: What is the Difference?

Git and GitHub are related, but they are not the same thing.

### What is GitHub?

GitHub is a web-based platform that hosts Git repositories and provides collaboration tools. Teams use it for code review, issue tracking, project management, and automation.

Git is the version control tool. GitHub is one platform where Git repositories can be hosted and shared.

### Git vs GitHub

| Aspect | Git | GitHub |
|---|---|---|
| What it is | Version control system | Repository hosting and collaboration platform |
| Main purpose | Track and manage changes | Host, share, review, and collaborate on code |
| Where it runs | Commonly on a developer's computer | Web platform and connected tools |
| Offline use | Most local operations work offline | Online service; remote operations need connectivity |
| Examples | `git init`, `git add`, `git commit` | Pull Requests, Issues, Actions |
| Dependency | Does not require GitHub | Uses Git repositories |

**Easy way to remember:**

- Git = Tool.
- GitHub = Platform.

Other platforms that host Git repositories include GitLab, Bitbucket, and Azure Repos.

---

# Part 3 – Git Architecture and Workflow

## 5. Understanding the Git Repository

A Git repository is a project directory managed by Git.

When you run `git init` in a directory, Git creates a hidden directory named `.git`. It stores the repository's internal data, including object storage, references, configuration, and index information.

Example:

```text
my-project/
│
├── .git/
│   ├── HEAD
│   ├── config
│   ├── objects/
│   ├── refs/
│   └── index
│
├── index.html
├── style.css
└── README.md
```

**Important:** Do not delete or manually edit `.git` files unless you understand the consequences. Removing `.git` removes the repository's local metadata and history.

## 6. Four Important Areas in the Git Workflow

Git's everyday workflow is commonly explained using four areas:

1. Working Directory
2. Staging Area
3. Local Repository
4. Remote Repository

![Git workflow infographic showing working directory, staging area, local repository and remote repository](images/git-introduction-infographic.png)

### 6.1 Working Directory

The working directory is the project folder where you create and modify files.

Example:

```text
devops-project/
│
├── index.html
├── style.css
└── app.js
```

When you modify `index.html`, Git can detect the change. However, the change is not automatically included in the next commit.

### 6.2 Staging Area

The staging area is where you select changes that should be included in your next commit. It is also called the index.

For example, you modify three files:

```text
index.html
style.css
app.js
```

If you only want to include the first two files in your next commit:

```bash
git add index.html
git add style.css
```

Those changes are now staged. The `app.js` change remains unstaged.

### 6.3 Local Repository

The local repository stores committed project history on your computer.

```bash
git commit -m "Add homepage design"
```

This creates a commit from the staged changes. A commit records information such as author, committer, timestamp, message, a reference to the project snapshot, and parent commit information.

### 6.4 Remote Repository

A remote repository is another repository, usually hosted on a service such as GitHub, GitLab, or Bitbucket.

You can send local commits to a remote repository:

```bash
git push
```

You can download and integrate remote changes:

```bash
git pull
```

### Basic workflow commands

| Command | Purpose |
|---|---|
| `git add` | Stage selected changes. |
| `git commit` | Save staged changes in local history. |
| `git push` | Send local commits to a remote repository. |
| `git pull` | Fetch and integrate remote changes. |

---

## 7. Important Git Terminologies

| Term | Explanation |
|---|---|
| Repository | Project managed by Git. |
| Commit | Saved snapshot of staged changes. |
| Branch | Separate line of development. |
| Merge | Combining changes from branches. |
| Clone | Creating a local copy of a repository and its history. |
| Fork | Creating your own GitHub-side copy of another repository. |
| Push | Sending local commits to a remote repository. |
| Pull | Fetching and integrating remote changes. |
| Fetch | Downloading remote updates without integrating them into the current branch. |
| HEAD | Reference to the currently checked-out commit or branch. |
| Remote | A named reference to another repository. |
| Staging | Preparing changes for a commit. |
| Conflict | A situation where Git cannot automatically combine changes. |
| Tag | A named reference commonly used for releases. |
| Rebase | Replaying commits on top of another base. |
| Stash | Temporarily storing uncommitted changes. |

---

# Part 4 – Git Internals: A Beginner Introduction

## 8. Understanding Git Objects

Git internally stores repository data using four main object types:

1. Blob
2. Tree
3. Commit
4. Tag

### 8.1 Blob Object

A blob stores the contents of a file. It does not store the original filename; the filename is represented through the tree structure.

### 8.2 Tree Object

A tree represents a directory structure. It connects file names to blob objects and directory names to other tree objects.

### 8.3 Commit Object

A commit records a project snapshot and metadata. It refers to a root tree and, where applicable, one or more parent commits. It also records author, committer, timestamps, and a commit message.

### 8.4 Tag Object

An annotated tag object stores information about a named reference to another Git object, commonly a release commit. A lightweight tag is simply a reference to an object.

## 9. Understanding Git Hashing

Git uses cryptographic hashing to identify objects. Historically, Git primarily used SHA-1. Git also supports SHA-256 repositories.

A hash is an identifier derived from the object's type, size, and content.

Example of a SHA-1 object ID:

```text
e69de29bb2d1d6434b8b29ae775ad8c2e48c5391
```

Why is this useful?

- It helps Git identify stored objects.
- It helps Git detect content differences.
- It supports repository data integrity.
- It allows Git to connect commits, trees, and blobs.

---

# Part 5 – Git in Real-World DevOps

## 10. Why is Git Important in DevOps?

DevOps brings development and operations practices together through collaboration, automation, continuous integration, continuous delivery, and reliable software deployment.

Git provides a version history and collaboration foundation for many of these activities.

### Git's role in the DevOps lifecycle

| DevOps activity | How Git helps |
|---|---|
| Plan | Stores project documentation and related files. |
| Code | Tracks source code changes. |
| Build | Code changes can trigger build pipelines. |
| Test | Pull Requests and commits can trigger automated tests. |
| Release | Tags and branches help organize releases. |
| Deploy | Git changes can trigger deployment workflows. |
| Operate | Tracks infrastructure and configuration definitions. |
| Monitor | Tracks monitoring dashboards and alert configuration files. |

### Git with Docker

A project might contain:

```text
my-app/
│
├── src/
├── package.json
├── package-lock.json
├── Dockerfile
├── .dockerignore
└── README.md
```

Git tracks application code and Docker configuration. A CI pipeline can build a Docker image when changes are pushed or merged.

### Git with Terraform

Terraform files describe infrastructure:

```text
aws-infrastructure/
│
├── main.tf
├── variables.tf
├── outputs.tf
├── providers.tf
└── README.md
```

Git tracks changes to infrastructure definitions. Teams can review changes before applying them.

**Security note:** Do not commit Terraform state files, cloud credentials, private keys, or secrets to a public repository.

### Git with Kubernetes

Kubernetes configuration can also be maintained in Git:

```text
kubernetes/
│
├── deployment.yaml
├── service.yaml
├── configmap.yaml
└── ingress.yaml
```

A CI/CD or GitOps workflow can use reviewed Git changes to update Kubernetes environments.

### GitOps – An Important DevOps Concept

GitOps is an approach in which Git stores the desired state of an application or infrastructure. A GitOps controller compares the desired configuration with the actual environment and reconciles differences.

For example, a Kubernetes deployment might specify:

```yaml
spec:
  replicas: 3
```

A GitOps controller can detect if the cluster differs from the desired configuration and reconcile it, depending on the controller's settings.

Tools used in GitOps include Argo CD and Flux.

---

# Part 6 – Best Practices

## 11. Best Practices Every Developer Should Follow

1. **Write meaningful commit messages.**

   Good:

   ```bash
   git commit -m "Add user registration API"
   ```

   Avoid vague messages such as `update` or `changes`.

2. **Commit small logical changes.** Each commit should represent a meaningful unit of work.

3. **Use branches.** Develop features and fixes separately instead of experimenting directly on the main branch.

4. **Synchronize with the remote repository.** Fetch and integrate team changes before starting work, following the team's agreed workflow.

5. **Review code before merging.** Use Pull Requests and code review.

6. **Use `.gitignore`.** Exclude appropriate untracked files such as dependency folders, build outputs, and local environment files.

   Example:

   ```gitignore
   node_modules/
   .env
   dist/
   build/
   *.log
   ```

   Note: `.gitignore` does not automatically stop tracking files that Git already tracks.

7. **Never commit secrets.** Do not commit AWS access keys, database passwords, private SSH keys, or API tokens. Use environment variables and a secrets manager.

8. **Protect important branches.** Require reviews and automated checks before merging where appropriate.

9. **Use tags for releases.**

   ```bash
   git tag -a v1.0.0 -m "Production release 1.0.0"
   git push origin v1.0.0
   ```

10. **Keep README documentation updated.** Explain what the project does and how to install, configure, and run it.

---

# Part 7 – Common Git Problems

## 12. Common Errors and Their Solutions

| Problem | Possible solution |
|---|---|
| `not a git repository` | Navigate into the repository or initialize one. |
| `Author identity unknown` | Configure Git username and email. |
| `remote origin already exists` | Inspect `git remote -v`; update or remove the remote if needed. |
| `rejected non-fast-forward` | Fetch remote changes and integrate them safely. |
| `merge conflict` | Resolve conflicting files and complete or abort the merge. |
| `Permission denied (publickey)` | Check SSH key configuration and repository access. |
| `nothing to commit` | Check whether there are actual changes to stage. |

Useful troubleshooting commands:

```bash
git status
git log --oneline --all --graph
git remote -v
git branch -a
git diff
git diff --staged
```

---

# Part 8 – Interview Preparation

## 13. Important Git Introduction Interview Questions

**Q1. What is Git?**

Git is an open-source distributed version control system used to track changes in files and collaborate on software projects.

**Q2. Who created Git and when?**

Linus Torvalds created Git in 2005.

**Q3. What is Version Control?**

Version Control is a system for recording and managing changes to files over time.

**Q4. Why do developers use Git?**

Developers use Git to track changes, maintain history, work in branches, collaborate, and restore earlier committed versions.

**Q5. What is GitHub?**

GitHub is a platform for hosting Git repositories and collaborating through features such as Pull Requests, Issues, and Actions.

**Q6. What is the difference between Git and GitHub?**

Git is the version control tool; GitHub is a platform for hosting and collaborating on Git repositories.

**Q7. Is Git centralized or distributed?**

Git is a distributed version control system.

**Q8. Can Git work without the internet?**

Yes. Most common local operations, such as committing, branching, and viewing history, work offline.

**Q9. What is a repository?**

A repository is a project managed by Git, including its files and version history.

**Q10. What is a commit?**

A commit is a saved snapshot of staged changes in a repository.

**Q11. What is a branch?**

A branch is a separate line of development.

**Q12. What is a remote repository?**

A remote repository is another repository, often hosted online, used to exchange changes with other developers.

**Q13. What is `.git`?**

It is the hidden directory that stores Git repository metadata, objects, references, and configuration.

**Q14. What are Git objects?**

Git's main object types are blob, tree, commit, and tag.

**Q15. Why is Git important in DevOps?**

Git provides version control for application code and infrastructure definitions and can trigger CI/CD and GitOps workflows.

---

# Part 9 – Summary and Revision

## 14. Key Takeaways

- Version Control helps track and manage changes to files.
- Git is a distributed version control system.
- GitHub is a platform for hosting and collaborating on Git repositories.
- Git can work locally without internet access for most operations.
- The four everyday workflow areas are working directory, staging area, local repository, and remote repository.
- Git stores information using blob, tree, commit, and tag objects.
- Git supports collaboration, code review, CI/CD, infrastructure versioning, and GitOps.
- Never commit credentials or secrets to a repository.

## 15. Classroom Discussion

Ask students:

1. Why do companies use Git instead of manually creating backup files?
2. What happens if a developer accidentally deletes their local project?
3. Why is Git called a distributed version control system?
4. Can Git be used without GitHub?
5. What is the difference between Git and GitHub?
6. How does Git help DevOps engineers?
7. Why should `.env` files not be pushed to public repositories?
8. Why is version history useful when investigating a production bug?

---

# 16. Homework and Learning Activity

### Theory assignment

1. Define Version Control.
2. Explain local, centralized, and distributed version control.
3. Explain Git and its features.
4. Differentiate Git and GitHub.
5. Explain the four areas of the Git workflow.
6. Explain blob, tree, commit, and tag objects.
7. Explain how Git supports Docker, Terraform, and Kubernetes workflows.

### Practical preparation

For the next lesson, students should:

- Install Git on their computer.
- Create a GitHub account.
- Open a terminal or Git Bash.
- Verify the installation using `git --version`.

Do not worry about mastering every command in this lesson. The next practical session will cover repository initialization, configuration, staging, commits, and history step by step.

---

## 17. Learning Resources

- [Git Official Website](https://git-scm.com/)
- [Git Official Documentation](https://git-scm.com/doc)
- [Pro Git Book – Free Online Book](https://git-scm.com/book/en/v2)
- [GitHub Documentation](https://docs.github.com/en)
- [GitHub Skills – Interactive Learning](https://skills.github.com/)
- [Learn Git Branching – Interactive Practice](https://learngitbranching.js.org/)

---

## Next Lesson

**Day 02 – Git Installation, Configuration and Basic Commands**

Topics:
- Installing Git on Windows and Linux.
- Configuring username and email.
- Creating a repository using `git init`.
- Understanding `git status`.
- Working with `git add` and `git commit`.
- Viewing history using `git log`.
- Comparing changes using `git diff`.
- First hands-on Git lab.

> **Trainer's note:** Ask every student to explain the journey of one code change—from editing a file on their laptop to committing it and sharing it with a team. Understanding that journey is more valuable than memorizing commands.

---

**DevOps Series | From Zero to Production**  
**Module 01 – Git Introduction**  
**Notes by Sahil Shaikh**
