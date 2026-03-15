+++
title = "Git Rebase and Merge"
+++

# 🧩 Git Merge Methods Explained

## 🔸 1. Fast-Forward Merge

### 📌 What:
Moves the branch pointer forward because there's no divergent history.

### 🧪 Example:
```bash
git checkout main
git merge feature-branch
```

✅ If `main` is behind `feature-branch` but has no new commits, Git just moves `main` forward — no merge commit is created.

### 📊 Result:
- No extra commit.
- Clean history.
- Only possible when histories are linear (no parallel commits).

---

## 🔸 2. Three-Way Merge (Default)

### 📌 What:
Used when branches diverged — Git creates a merge commit.

### 🧪 Example:
```bash
git checkout main
git merge feature-branch
```

🔄 If both `main` and `feature-branch` have new commits, Git creates a merge commit to tie them.

### 📊 Result:
- Merge commit created.
- Retains full history of both branches.
- Good for preserving commit context.

---

## 🔸 3. Squash Merge

### 📌 What:
Combines all commits from the feature branch into a single commit on the target branch.

### 🧪 Example:
```bash
git checkout main
git merge --squash feature-branch
git commit -m "Add feature X"
```

📝 All feature-branch commits are flattened into one.

### 📊 Result:
- Clean, single commit.
- Good for simplifying history.
- Loses detailed history of feature-branch.

---

## 🔸 4. Rebase + Fast-Forward

### 📌 What:
Rewrites feature branch history onto base branch — keeps history linear and avoids merge commits.

### 🧪 Example:
```bash
git checkout feature-branch
git rebase main
git checkout main
git merge --ff-only feature-branch
```

📈 Keeps history tidy and linear.

### 📊 Result:
- No merge commits.
- Linear history.
- Can be risky if rebasing shared branches (changes commit hashes).

---

## 🔸 5. No-Fast-Forward Merge (`--no-ff`)

### 📌 What:
Forces Git to always create a merge commit, even if a fast-forward is possible.

### 🧪 Example:
```bash
git merge --no-ff feature-branch
```

🔗 Use this to clearly show a branch was merged, even if no divergence.

### 📊 Result:
- Always a merge commit.
- Good for clear separation of features in history.

---

## 🔍 Summary Table

| Merge Method         | Merge Commit | History Type  | Use Case                               |
|----------------------|--------------|---------------|----------------------------------------|
| Fast-Forward         | ❌ No         | Linear        | Simple updates                         |
| Three-Way Merge      | ✅ Yes        | Diverged      | Default for complex merges             |
| Squash Merge         | ❌ One commit | Simplified    | Clean history, single logical change   |
| Rebase + FF          | ❌ No         | Linear        | Clean history before merge             |
| No-FF                | ✅ Yes        | Always visible| Always document a merge explicitly     |

---

## 🧠 Best Practices

- ✅ Use **squash** for feature branches in solo/team projects to simplify history.
- ✅ Use **no-ff** in protected/main branches for clear audit trails.
- ✅ Use **rebase** to clean up local commits before merging.

---


## 🔁 Understanding `git rebase`

### 🔹 What is `git rebase`?

`git rebase` is used to **move or combine a sequence of commits to a new base commit**. It is often used to **maintain a cleaner, linear project history**.

Unlike `merge`, which creates a new commit, `rebase` re-applies commits on top of another branch.

---

### 🔹 Syntax

```bash
git rebase <upstream-branch>
```

Example:

```bash
git checkout feature
git rebase main
```

This reapplies your feature branch commits on top of the latest `main`.

---

### 🔹 Common Use Case: Updating Feature Branch

```bash
# Assume you're on feature branch
git checkout feature/login
git fetch origin
git rebase origin/main
```

✅ This puts your feature branch on top of the latest main, as if it was created from it.

---

### 🔹 Resolving Conflicts During Rebase

If conflicts occur:
```bash
# After editing the file to fix conflicts
git add <resolved-file>
git rebase --continue
```

To skip the current commit:
```bash
git rebase --skip
```

To abort the rebase and return to the previous state:
```bash
git rebase --abort
```

---

### 🔄 Interactive Rebase

Use this to **edit, squash, or reword commits**.

```bash
git rebase -i HEAD~3
```

Example interactive menu:

```
pick 3f5e1f9 Added login form
pick 89d2ac3 Fixed typo in login
squash d14e7a3 Updated form styles
```

Change `pick` to:
- `reword` — change the commit message
- `squash` — combine commits into one
- `edit` — pause to change code during rebase

---

### 🆚 Rebase vs Merge

| Feature        | `git merge`                        | `git rebase`                        |
|----------------|-------------------------------------|--------------------------------------|
| History        | Keeps history with a merge commit   | Creates a linear history             |
| Commit Graph   | May look like a tree                | Looks like a straight line           |
| Use When       | You want to preserve all changes    | You want a clean history             |

---

### 🚫 Don’t Rebase Shared Branches

⚠️ Avoid using `git rebase` on branches that others are working on (i.e., already pushed/shared), unless you coordinate with your team.

---

### 🔧 Example: Clean Up Before Merge Request

```bash
git checkout feature/payment
git rebase -i HEAD~5
# squash into a single commit
```

Then:
```bash
git push -f origin feature/payment
```

---

> 💡 Tip: Use `git log --oneline --graph` before and after rebase to visualize the change.
