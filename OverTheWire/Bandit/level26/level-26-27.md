# Bandit Level 26 → Level 27

### 🎯 Objective
Good job getting a shell! Now hurry and grab the password for bandit27!

### 🔍 Information & Clues
• **Host:** bandit.labs.overthewire.org
• **Port:** 2220
• **Username:** bandit26
• **Commands to research:** ls, id, ./bandit27-do, cat

### 💡 Thought Process & Steps
1. **Connecting:**
Since I already broke out of the restricted more shell into /bin/bash at the end of Level 25 → 26, I remained directly inside the bandit26 session:

```bash
bandit26@bandit:~$
```

2. **Enumeration:**
I listed the files in the bandit26 home directory using `ls -la` and found an executable binary named `bandit27-do` alongside `text.txt`. Running `./bandit27-do` without arguments showed its usage:

```bash
./bandit27-do
# Output: Run a command as another user. Example: ./bandit27-do id
```

Testing it with id confirmed that the binary has the SUID bit set and runs commands with the effective user ID (euid) of bandit27:

```bash
./bandit27-do id
# Output: uid=11026(bandit26) gid=11026(bandit26) euid=11027(bandit27) groups=11026(bandit26)
```

3. **Reading the Flag:**
I used `./bandit27-do` to execute cat with bandit27's privileges and read the password file:

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

### 🔑 Key Features Learned
• **SUID (Set User ID) Binaries:** In Linux, when an executable file has the SUID permission bit set, it runs with the privileges of the file's owner (bandit27) rather than the user executing it (bandit26).
• **Privilege Escalation via Wrapper Binaries:** If a SUID binary executes user-supplied commands without sanitizing inputs, it can be leveraged to read restricted files or spawn a privileged shell.
• **UID vs. EUID:** Checking id shows both your real User ID (uid) and your Effective User ID (euid), confirming which user's permissions are active during execution.

###🏁 Password/Flag
`💾 STJLJBRRphMxKB392CT4iOr5CbzPU9ER`