# Bandit Level 27 → Level 28

### 🎯 Objective
There is a git repository at ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo via port 2220. The password for the user bandit27-git is the same as for the user bandit27.
Clone the repository and find the password for the next level.

### 🔍 Information & Clues
• **Host:** bandit.labs.overthewire.org
• **Port:** 2220
• **Username:** bandit27 / bandit27-git
• **Repository Path:** /home/bandit27-git/repo
• **Commands to research:** git

### 💡 Thought Process & Steps
1. **Connecting & Testing Localhost:**
I initially logged into bandit27 over SSH and created a temporary writable directory using `mktemp -d` inside `/tmp` (since the home directory is read-only). However, attempting to clone from localhost failed because OverTheWire blocks SSH connections originating from localhost to conserve server resources.

2. **Cloning from the Client Machine:**

I exited the SSH session and ran git clone directly from my local machine's terminal, making sure to explicitly specify port 2220 in the SSH URL:

```bash
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```

When prompted, I entered the password for bandit27, which successfully downloaded the repo directory locally.

3. **Enumeration & Reading the File:**
Once the repository finished cloning, I navigated into the new repo folder and listed its contents (dir on Windows / ls on Linux), revealing a README file. I read its contents (type README on Windows / cat README on Linux):

```bash
cd repo
cat README
```

The file contained the password for bandit28.

### 🔑 Key Features Learned
• **git clone:** Downloads a copy of a remote Git repository to a local machine.
• **ssh://user@host:port/path:** Custom SSH URI syntax used when a Git server runs on a non-standard port (like 2220 instead of 22).
• **Client-side cloning:** When a server restricts localhost SSH connections, remote Git repositories over SSH can be cloned directly onto a local client machine.

### 🏁 Flag
`y8Yd2ssKcpHpud7UvOSOxwamRMzIGIeQ`