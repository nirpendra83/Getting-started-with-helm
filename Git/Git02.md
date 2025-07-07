
+++
title = "Git Logs"
+++


## 🔍 Advanced `git log` Examples

`git log` is highly customizable. Here are real-world examples:

---

### 📦 Basic Commit Log

```bash
git log
```
Shows full commit history with author, date, and message.

---

### 🧵 One-line Log (Condensed View)

```bash
git log --oneline
```
Shows each commit as a single line: useful for quick scanning.

---

### 🌲 Visual Branch Graph

```bash
git log --oneline --graph --all
```
Displays commit history as a **branch graph** across all branches.

---

### 📆 Log with Dates

```bash
git log --pretty=format:"%h - %an, %ar : %s"
```
Custom format: shows commit hash, author, relative date, and message.

---

### 🔎 Filter by Author

```bash
git log --author="nirpendra"
```
Shows only commits made by a specific author.

---

### 🕐 Commits from the Last 7 Days

```bash
git log --since="7 days ago"
```

Or filter between two dates:

```bash
git log --since="2024-06-01" --until="2024-06-30"
```

---

### 📁 Log for a Specific File

```bash
git log -- <file-path>
```
View changes made to a particular file.

Example:
```bash
git log -- src/main.py
```

---

### 🧾 Show Diff with Each Commit

```bash
git log -p
```
Includes the patch (code changes) with each commit.

---

### 🧰 Combined Format: Pretty + Graph + Oneline

```bash
git log --pretty=format:"%C(yellow)%h%Creset %Cgreen%ad%Creset %C(cyan)%an%Creset - %s" --date=short --graph
```

This gives a colorful, structured view:
- Hash in yellow
- Date in green
- Author in cyan

---

### 🔢 Limit the Number of Commits

```bash
git log -n 5
```
Shows the **last 5 commits** only.

---

### 📊 Stats Per Commit

```bash
git log --stat
```
Displays each commit along with the number of lines added/removed and the file names.

---

### 📜 List Files Modified in Last Commit

```bash
git log -1 --name-only
```

---

### 🔐 View Only Merge Commits

```bash
git log --merges
```

---

> 💡 Tip: You can combine these flags for powerful custom outputs!
Example:
```bash
git log --oneline --author="nirpendra" --since="1 week ago"
```

---
