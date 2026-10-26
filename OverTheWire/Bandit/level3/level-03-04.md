# Bandit Level 3 → Level 4

### 🎯 Objective
The password for the next level is stored in a hidden file in the inhere directory.


### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit3`
* **Commands to research:** `cd`, `ls -la`

---

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit3@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
Once inside, I ran `ls` to see the files, a single directory named `inhere` was listed.

I used the `cd` command, which stands for `change directory`, to move to the specific folder:

```bash
cd inhere
```

Once inside the new folder my prompt changed. The prompt now displays my current working directory which is the directory called `inhere`

```text
bandit3@bandit ~/inhere$
```

I used the list command `ls` again to see what is inside the new working directory and nothing came up. In order to read the hidden file located in the current working directory i used the `ls` command with addition flags set, namely the `-l` flag and the `-a` flag.

```bash
ls -al
```

After running the modified command a filed showed up called `...Hiding-From-You`:

```text
total 12
drwxr-xr-x 2 root    root    4096 Sep 26 21:54 .
drwxr-xr-x 3 root    root    4096 Sep 26 21:54 ..
-rw-r----- 1 bandit4 bandit3   33 Sep 26 21:54 ...Hiding-From-You
```

3. **Reading the Flag:**
I used the `cat` command followed by the file name `...Hiding-From-You` to read its contents:

```bash
cat ...Hiding-From-You
```

---

### 🔑 Key Features Learned
* `cd` : In order to change to another directory you need you need to use the `cd` command followed by the directory name
* `ls -al` : When a file name begins with a dot `.`, the file wont be displayed when you use the regular `ls` command. Pairing the `ls` command with the `-l` flag, which lists the contents of the directory, and the `-a` flag, which outputs ALL of the contenst inside the current working directory.
*  

### 🏁 Password/Flag
`💾 xzTXq1rDJQVVAzdv5cHq1TQytTWufAMq`