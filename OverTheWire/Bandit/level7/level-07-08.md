# Bandit Level 7 → Level 8

### 🎯 Objective
The password for the next level is stored in the file data.txt next to the word millionth


### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit7`
* **Commands to research:** 

---

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit7@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
Once inside, I ran `ls` to see the files, a single file named `data.txt` was listed.

I used `cat` to read the file data and as soon as i opened the file my terminal started running across my screen with hundereds of passowrds.

After read ing the instructions again they mentioned that the password we need for the next ssh login could be found next to the word `millionth`. We learned in previous rooms that we can use the pipe `|` operator to send one commands output to another command as input. We will be using the `cat` command in conjunction with the `grep` command to find the specific password.

3. **Reading the Flag:**
I used the `cat` command, followed by the file name `data.txt`, and then piped that output to the `grep` command that searches for a specific string inside the output.  Its contents:

```bash
cat data.txt | grep millionth
```

The output of the command gave an output in the following format:

```text
millionth      ...Password...
```

---

### 🔑 Key Features Learned
* "grep" : The grep command searches for a specified term inside the output it was piped

### 🏁 Password/Flag
`💾 VR1ljMayciFxbnUokuQmJFw6QC9VKtub`