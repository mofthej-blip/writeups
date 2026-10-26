# Bandit Level 0 → Level 1

### 🎯 Objective
The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.

The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit0`
* **Commands to research:** `ssh`, `ls`, `cat`

---

### 💡 Thought Process & Steps
1. **Connecting:** 
I logged into the level using SSH:

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:** 
Once inside, I ran `ls` to see the available files/directories. A single file named `readme` appeared.

3. **Reading the File:** 
I used the `cat` command to read the files contents:

```bash
cat readme
```

The file contained the password to log into the next SSH session for the next level.

---

### 🔑 Key Features Learned
* `ssh`: An encrypted shell tunnel.
* `ls`: Lists files in current directory.
* `cat`: Concatenates and displays file content.

### 🏁 Flag
`💾 6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR`
