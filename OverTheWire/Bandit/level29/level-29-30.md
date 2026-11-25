# Bandit Level 29 → Level 30

### 🎯 Objective
There is a git repository at `ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo` via port `2220`. The password for the user `bandit29-git` is the same as for the user `bandit29`.
Clone the repository and find the password for the next level.

### 🔍 Information & Clues
• **Host:** bandit.labs.overthewire.org
• **Port:** 2220
• **Username:** bandit29 / bandit29-git
• **Repository Path:** /home/bandit29-git/repo
• **Commands to research:** git

### 💡 Thought Process & Steps

1. **Cleaning Up & Cloning from the Client Machine:**
First, I removed the repo folder left over from Level 28 (rmdir /s /q repo on Windows) so Git would not run into a directory-name conflict. Because OverTheWire blocks SSH connections originating from localhost to conserve server resources, I cloned the repository directly onto my local machine while explicitly specifying port 2220:

```bash
git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo
```

When prompted, I entered the bandit29 password (Em7eGtqaMySwNFjCpwzzHhLhospOcdt0), which downloaded the repository locally.

2. **Initial Enumeration & Checking Commit History:**
I moved into the repo directory and read README.md (type README.md on Windows / cat README.md on Linux):

```bash
cd repo
cat README.md
```

Instead of a password, the file displayed - password: <no passwords in production!>. Checking the commit history on the default master branch with git log -p showed only a username correction and never contained the password—confirming the credential was never committed to the master (production) branch.

3. **Discovering & Switching Branches:**
The phrase "no passwords in production" strongly hinted that the credential was left behind on a separate development branch. To see every local and remote branch tracked by the repository, I ran:

```bash
git branch -a
```

This listed two additional remote branches: remotes/origin/dev and remotes/origin/sploits-dev. I switched over to the dev branch using git checkout:

```bash
git checkout dev
```

Git automatically created a local dev branch tracking origin/dev and updated the working files in my directory to match that branch's state.

4. **Reading the File on the dev Branch:**
Now that my working directory reflected the dev branch, I read README.md again:

```bash
cat README.md
```

This time, the file contained the plaintext password for bandit30.

### 🔑 Key Features Learned
• **git branch -a:** Lists all local branches as well as remote-tracking branches (remotes/origin/...) fetched during a clone.
• **git checkout <branch>:** Switches your working directory to a different branch (and automatically sets up a local tracking branch if a matching remote branch exists).
• **Branch-level secret exposure:** Even when sensitive credentials are kept out of a master or main production branch, attackers with read access to a repository can inspect dev, staging, or feature branches to find hardcoded passwords.

### 🏁 Flag
`jq9Dfg2rXsfYsWMgFuKlXhphjdH7USgX`