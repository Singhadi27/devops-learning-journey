# 🚀 DevOps Learning Summary (Classes 1–14)

This document summarizes my foundational journey into DevOps, covering core topics like Linux, Git, AWS, and Maven. Below is a class-wise breakdown of the concepts and commands I've learned.

---

## 🌐 Class 3: Web Server on AWS EC2

- **Objective:** Deploy a live web server.
- **Tool:** AWS EC2 (Elastic Compute Cloud)
- **Steps to launch EC2:**
  - AMI (OS selection)
  - Instance Type (CPU & RAM)
  - Key Pair (Login access)
  - Network (VPC, Security Groups)
  - Storage setup
  - Launch and connect via SSH (port 22)
- **Deployment:** Host application using a startup script, accessible via public IP.
- **Interfaces:** CLI (Command Line) and GUI (Graphical).

---

## 🐧 Class 4–5: Linux Essentials

- **System Info Commands:**
  - `cat /proc/meminfo` – Memory details
  - `fdisk -l`, `lsblk` – Disk info
- **File Management:**
  - `touch file`, `ls`, `ll`, `cat`
  - `touch file{1..100}` – Create 100 files
  - `cat file1 >> file2` – Append file1 to file2
- **Text Editors:**
  - `vi/vim`, `nano`
  - `i` to insert, `esc` to exit insert mode

---

## 🔎 Class 7: Text Filtering & Editing

- **GREP (Global Regular Expression Print):**
  - `grep "text"` – Basic search
  - `grep -i "text"` – Case-insensitive search
- **SED (Stream Editor):**
  - Replace text: `sed 's/old/new/' file1 >> file2`
  - Replace entire lines: Use `cat -n` to get line numbers
  - Vim substitute: `:%s/old/new/g`

---

## 👥 User & Group Management

- Add users: `useradd raham`, `passwd raham`
- Grant sudo: `visudo` → Edit and assign privileges
- Switch users: `su username`, `sudo command`

---

## 🧾 Class 8–11: Git & Version Control

- **Git Basics:**
  - `git init`, `git add`, `git commit`
  - `git log`, `git log -2 --oneline`
  - `git show`, `git show <commit_id>`
- **Branching:**
  - `git branch`, `git checkout`, `git checkout -b`
  - Rename: `git branch -m old new`
- **Merging & Rebase:**
  - `git merge <branch_name>`
  - 📌 *Interview Tip:* Know the difference between `merge` and `rebase`

---

## 🚫 Class 12: `.gitignore`

- Use `.gitignore` to exclude files/folders from Git tracking (e.g., node_modules, .env, build folders).

---

## ☕ Class 13: Maven – Java Build Tool

- **Build Lifecycle:**
  - `.java → .class → .jar → .war`
- **Key Terms:**
  - `.class` – Compiled Java
  - `.jar` – Java Archive (backend)
  - `.war` – Web Archive (full-stack)
- **Project File:** `pom.xml` – Contains dependencies and config
- **Commands:**
  - `mvn compile` – Compiles the code
  - `mvn clean` – Cleans target folder
- **Artifacts:** Final output packages (JAR/WAR)
- 🆚 *Maven vs Ant* – Maven is XML-based and lifecycle-driven

---

## 🛠️ Tools Covered

- ✅ AWS EC2
- ✅ Linux Shell (Ubuntu)
- ✅ Vim & Nano Editors
- ✅ Git & GitHub
- ✅ Maven (Java Build Management)

---

📚 *This marks the foundation of my DevOps learning path — from setting up infrastructure to managing source code and builds.*
