# Bandit Level 28 → Level 29

### 🎯 Objective
There is a git repository at `ssh://bandit28-git@bandit.labs.overthewire.org/home/bandit28-git/repo` via port `2220`. The password for the user `bandit28-git` is the same as for the user `bandit28`.
Clone the repository and find the password for the next level.

### 🔍 Information & Clues
• **Host:** bandit.labs.overthewire.org
• **Port:** 2220
• **Username:** bandit28 / bandit28-git
• **Repository Path:** /home/bandit28-git/repo
• **Commands to research:** git

### 💡 Thought Process & Steps
1. **Cleaning Up & Cloning from the Client Machine:**
First, I deleted the repo directory left over from Level 27 (rmdir /s /q repo on Windows) so Git wouldn't throw a folder-name conflict. Because OverTheWire blocks SSH connections originating from localhost, I cloned the repository directly onto my local machine using port 2220:

```bash
git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo
```

When prompted, I entered the bandit28 password (y8Yd2ssKcpHpud7UvOSOxwamRMzIGIeQ), which downloaded the new repo folder.

2. **Enumeration & Reading the Current File:**
I moved into the repo directory and inspected the README.md file:

```bash
cd repo
cat README.md
```

Unlike the previous level, the password inside README.md had been replaced with xxxxxxxxxx.
3. **Inspecting the Git Commit History:**
Because Git saves every historical snapshot and modification made to a repository, censoring a password in a newer commit does not erase it from earlier commits. I ran git log -p to inspect the commit history along with the exact line-by-line diffs:

```bash
git log -p
```

Scrolling down through the pager output (Spacebar / Enter, then q to quit) revealed a commit titled "fix info leak", where the original password for bandit29 had been removed and overwritten with xxxxxxxxxx.

### 🔑 Key Features Learned
• **git log -p (or --patch):** Displays the full commit history alongside the exact line-by-line changes (diffs) introduced in each commit.
• **Git history persistence:** Deleting or overwriting sensitive data (like passwords or API keys) in a new commit does not remove it from the repository's history—anyone with access to the .git directory can inspect past commits to recover leaked credentials.
• **rmdir /s /q (Windows):** Forcefully and quietly removes a directory along with all hidden subdirectories (such as .git).

### 🏁 Flag
`Em7eGtqaMySwNFjCpwzzHhLhospOcdt0`