# Bandit Level 2 → Level 3

### 🎯 Objective
The password for the next level is stored in a file called --spaces in this filename-- located in the home directory


### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit2`
* **Commands to research:** `None`

---

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit2@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
Once inside, I ran `ls` to see the files, a single file named `--spaces in this filename--` was listed.

I used the same consept as the previous room to prepend the path to the file infront of the file. But once i ran the command there was an error. The command did not understand the spaces in the file name and read them all as individual files.

```bash
cat ./--spaces in this filename--
```

The output of the command above gave the following error message:

```text
cat: ./--spaces: No such file or directory
cat: in: No such file or directory
cat: this: No such file or directory
cat: filename--: No such file or directory
```

To bypass this error you have to enclose any file that has spaces in quotations `"`:

3. **Reading the Flag:**
To read the flag i used the `cat` command, along with the specified path and quotationmarks:

```bash
cat ./"--spaces in this filename--"
```

---

### 🔑 Key Features Learned
* `"` : When a file has spaces in its name the spaces would indicate the tart of a new filename.
* `"` : Using the quotationmarks to enclose the filename would bypass this error and concatenate the contents of the file

### 🏁 Password/Flag
`💾 7ZZ2LFrykP2zEyvBl4m3clcL7tGYJPME`
