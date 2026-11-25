# Bandit Level 31 → Level 32

### 🎯 Objective
There is a git repository at ssh://bandit31-git@bandit.labs.overthewire.org/home/bandit31-git/repo via port 2220. The password for the user bandit31-git is the same as for the user bandit31.
Clone the repository and find the password for the next level.

### 🔍 Information & Clues
• **Host:** bandit.labs.overthewire.org
• **Port:** 2220
• **Username:** bandit31 / bandit31-git
• **Repository Path:** /home/bandit31-git/repo
• **Commands to research:** git

### 💡 Thought Process & Steps

1. **Cloning & Reading the Instructions:**
After removing the previous level's repo folder (rmdir /s /q repo), I cloned the Level 31 repository onto my local machine over port 2220 using the bandit31 password (82NkymblpGBYmIXG6ZQ8YldBYstHpfUf):

```bash
git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo
```

Moving into the directory (cd repo) and inspecting the files revealed README.md alongside a hidden .gitignore file. Reading README.md provided the exact task requirements:

```bash
type README.md
```

• File name: key.txt
• Content: 'May I come in?'
• Branch: master

2. **Identifying the .gitignore Restriction:**
Before creating the file, I inspected .gitignore (type .gitignore on Windows / cat .gitignore on Linux), which contained a single rule: *.txt. In Git, a .gitignore file specifies intentionally untracked file patterns that Git should ignore by default—meaning a standard git add key.txt would silently skip the file unless explicitly forced.

3. **Creating the File & Force-Staging:**
To create key.txt without Windows echo adding an unintended trailing space or newline, I used echo|set /p to write the exact string May I come in? into key.txt, and verified it with type key.txt:

```cmd
echo|set /p="May I come in?" > key.txt
type key.txt
```

Next, I staged the ignored file using the -f (--force) flag on git add and committed it to my local master branch:

```bash
git add -f key.txt
git commit -m "add key.txt"
```

4. **Pushing to Trigger the Server Validation Hook:**
Finally, I pushed the commit to the remote master branch:

```bash
git push origin master
```

Upon receiving the push, the remote server executed a Git pre-receive hook—a server-side script triggered automatically when a client pushes commits. The script verified that key.txt existed on master with the exact required content, printed the password for bandit32 directly in the remote terminal output, and then declined the push so the shared challenge repository remained unmodified for other players.

### 🔑 Key Features Learned
• **.gitignore:** A configuration file that tells Git which files or patterns (like *.txt) to exclude from tracking.
• **git add -f (--force):** Overrides .gitignore rules to explicitly stage an ignored file into the Git index.
• **Git Hooks (pre-receive):** Automated scripts stored in .git/hooks that execute before or after events like commits and pushes—frequently used on servers for validation, CI/CD checks, or access control.

### 🏁 Flag
`pWuj5jBQ6IgV0NXwiH6g1pXRF8S1YvbT`