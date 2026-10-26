# Bandit Level 1 → Level 2

### 🎯 Objective
The password for the next level is stored in a file called - located in the home directory


### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit1`
* **Commands to research:** 

---

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
Once inside, I ran `ls` to see the files, a single file named `-` was listed.

I prepended the specified the path to the file to extract its contents. Since the file was in the home directory i only prepender `./` to the front of the namefile.

3. **Reading the Flag:**
I used the `cat` command, followed by the path to the file `./` and then the file name `-` to read its contents:

```bash
cat ./-
```

---

### 🔑 Key Features Learned
* ‘-’ is a special symbol in Linux. So it is not recommended to start a file name with this symbol.
* ‘-’ is the standard option character. We have already seen it for adding so-called flags for specific options to a command (Like the -p flag for choosing a port for the ssh command.). standard option. Because of this, files with this symbol as the first character cannot just be referenced as other files.

### 🏁 Password/Flag
`💾 PK8fYLZg2hnHSz83plBL1iEPKdD3QToB`