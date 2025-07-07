
+++
title = "Git"
+++


## 🚀 Git Tutorial from Start — With Real-Time Examples

## 📘 Table of Contents

- [🚀 Git Tutorial from Start — With Real-Time Examples](#-git-tutorial-from-start--with-real-time-examples)
- [📘 Table of Contents](#-table-of-contents)
- [📌 What is Git?](#-what-is-git)
- [🧰 Installing Git](#-installing-git)
  - [For Linux:](#for-linux)
  - [For macOS:](#for-macos)
  - [For Windows:](#for-windows)
- [⚙️ Git Configuration](#️-git-configuration)
- [🔄 Basic Git Workflow](#-basic-git-workflow)
- [🧬 Cloning a Repository](#-cloning-a-repository)
- [✍️ Making Changes \& Committing](#️-making-changes--committing)
- [🌿 Branching and Merging](#-branching-and-merging)
- [🔄 Real-Time Example: Feature Branch Workflow](#-real-time-example-feature-branch-workflow)
- [📦 Stashing Changes](#-stashing-changes)
- [🌐 Working with Remotes](#-working-with-remotes)
- [📜 Git Log and History](#-git-log-and-history)
- [🔙 Undoing Things](#-undoing-things)
- [🚫 .gitignore File](#-gitignore-file)
- [🏷️ Git Tags](#️-git-tags)
- [✅ Conclusion](#-conclusion)

---

## 📌 What is Git?

- Git is a **distributed version control system**.
- Tracks changes in source code during software development.
- Helps teams collaborate on code.

---

## 🧰 Installing Git

### For Linux:
```bash
sudo apt update
sudo apt install git
```

### For macOS:
```bash
brew install git
```

### For Windows:
- Download from: https://git-scm.com/downloads

---

## ⚙️ Git Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --list  # verify settings
```

---

## 🔄 Basic Git Workflow

```bash
git init                     # Initialize a Git repository
git status                   # Check current status
git add file.txt             # Stage a file
git commit -m "Added file"   # Commit with message
```

---

## 🧬 Cloning a Repository

```bash
git clone https://github.com/yourusername/project.git
```

---

## ✍️ Making Changes & Committing

```bash
echo "Hello Git" > hello.txt
git add hello.txt
git commit -m "Added hello.txt file"
```

---

## 🌿 Branching and Merging

```bash
git branch                  # List branches
git branch feature-x        # Create a new branch
git checkout feature-x      # Switch to branch
# Make changes and commit
git checkout main
git merge feature-x         # Merge branch into main
```

---

## 🔄 Real-Time Example: Feature Branch Workflow

**Scenario**: You are adding a login feature.

```bash
git checkout -b feature/login
# Add login logic to login.js
git add login.js
git commit -m "Add login feature"
git push origin feature/login
# Open a Merge Request (MR) on GitLab/GitHub
```

After MR approval:

```bash
git checkout main
git pull origin main
git merge feature/login
git push origin main
```

---

## 📦 Stashing Changes

```bash
git stash           # Save uncommitted changes
git pull origin main
git stash pop       # Apply stashed changes
```

---

## 🌐 Working with Remotes

```bash
git remote -v                          # Show configured remotes
git remote add origin <repo-url>      # Add remote repository
git push -u origin main               # Push code to remote
git pull origin main                  # Pull latest changes
```

---

## 📜 Git Log and History

```bash
git log                                # View commit logs
git log --oneline --graph --all        # Pretty graph format
```

---

## 🔙 Undoing Things

```bash
git checkout -- file.txt       # Discard uncommitted changes
git reset HEAD~1               # Undo last commit (keep changes)
git revert <commit-id>         # Revert a specific commit
```

---

## 🚫 .gitignore File

Create a `.gitignore` file in the project root to ignore files/folders.

```text
# .gitignore example
*.log
node_modules/
.env
.idea/
.DS_Store
```

---

## 🏷️ Git Tags

```bash
git tag v1.0                         # Lightweight tag
git tag -a v1.1 -m "Release v1.1"    # Annotated tag
git push origin --tags               # Push tags to remote
```

---

## ✅ Conclusion

- Git is essential for modern development workflows.
- Use branches and MRs for cleaner collaboration.
- Practice using `add`, `commit`, `push`, and `pull` regularly.
- Use `git status` and `git log` to understand project state.

---

> 🙌 *Happy Coding with Git! Keep committing and stay versioned!*
