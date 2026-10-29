# Bandit Level 11 → Level 12

### 🎯 Objective
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions


### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit11`
* **Commands to research:** 

---

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit11@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
Once inside, I ran `ls` to see the files, a single file named `data.txt` was listed.

I used `cat` to read the contents of the file and i got the following output:

```text
Gur cnffjbeq vf TEBbmJCB8DlA0zTewHxVQ0JPLxMvDkeA
```

If we inspect the instructions given to us we can conclude that all the letters have been rotated by 13 possitions. Substitution means replacing one character with another. Substitution ciphers have been known and used for a very long time. Some of the most known are the Caesar and Vigenere ciphers. However, these simple substitution ciphers are not secure anymore! The Linux tr command, which stands for 'translate', allows replacing characters with others. The base command syntax looks like the following `tr <old_chars> <new_chars>`.

3. **Reading the Flag:**
I used the `cat` command, followed by thefile name `data.txt`. I then piped the `tr` command, followed by the old and the new characters, to read its contents:

```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

The command above gave the following output:

```text
The password is <password>
```

---

### 🔑 Key Features Learned
* "tr" : The tr command (short for "translate") is a command-line utility used to translate, delete, or squeeze characters from standard input and write the result to standard output.

### 🏁 Password/Flag
`💾 GROozWPO8QyN0mGrjUkID0WCYkZiQxrN`