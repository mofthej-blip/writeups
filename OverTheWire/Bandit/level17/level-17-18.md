# Bandit Level 17 → Level 18

### 🎯 Objective
There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

NOTE: if you have solved this level and see 'Byebye!' when trying to log into bandit18, this is related to the next level, bandit19


### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit17`
* **Commands to research:** `diff`

---

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh -i bandit17_private_key.txt bandit17@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
The diff command compares two files line-by-line and highlights the differences between them. In OverTheWire's Bandit Level 17 to 18, your goal is to find the single line that changed between two files.

When you run diff, it compares a source file to a target file. It outputs the specific lines that are different, along with symbols indicating what needs to change to make the first file match the second:

- "<" means a line is only in the first file.
- ">" means a line is only in the second file.
- "---" acts as a divider between the changes of the two files.

Because these two files are identical except for one password, your output will look something like this:

```text
42c42
< old_password_here
---
> new_password_here
```

3. **Reading the Flag:**
To find the password for the next level, run the following command in your terminal:

```bash
diff passwords.old passwords.new
```

---

### 🔑 Key Features Learned
* "diff" : It takes two versions of a document, compares them side-by-side, and immediately points out exactly what was added, removed, or changed between them.

### 🏁 Password/Flag
`💾 OQxXZjELndr90zuhOTDYBEomI0SZITXI`