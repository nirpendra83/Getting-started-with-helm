+++
title = "Git More Examples "
+++


---

## 📜 Git Log and History

```bash
git log
git log --oneline
git log --oneline --graph --all
```

---

## 🔙 Undoing Things

```bash
git checkout -- file
git revert <commit>
```

---

## 🧠 Understanding `git reset`

```bash
git reset --soft HEAD~1     # Keep changes staged
git reset --mixed HEAD~1    # Keep changes, unstage
git reset --hard HEAD~1     # Discard changes
```

---

## 🔍 Advanced `git log` Examples

```bash
git log                       # Full history
git log --oneline             # Short view
git log --graph --oneline     # Tree view
git log --author="name"
git log --since="7 days ago"
git log -p                    # Show diffs
git log --stat                # File changes
git log -1 --name-only        # Files in last commit
git log --merges              # Only merge commits
```

---

## 🚫 .gitignore File

```
*.log
.env
node_modules/
.idea/
```

---

## 🏷️ Git Tags

```bash
git tag v1.0
git tag -a v1.1 -m "Release v1.1"
git push origin --tags
```

---

## ✅ Conclusion

Use Git daily. Learn branching, logs, reset, and collaboration with remotes. Practice makes perfect!

---

🙌 Happy Coding with Git!
