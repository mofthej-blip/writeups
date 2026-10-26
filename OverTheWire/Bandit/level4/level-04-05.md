# Bandit Level 4 → Level 5

### 🎯 Objective
The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the "reset" command.


### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit4`
* **Commands to research:** `file`

---

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
Once inside, I ran `ls` to see the files, a single directory named `inhere` was listed.

I used the `cd` command to change to the `inhere` directory

```bash
cd inhere
```

once inside i ran the `ls` command agian and multiple files were listed:

```bash
ls
```

The output of the command above is as followed:

```text
-file00  -file02  -file04  -file06  -file08
-file01  -file03  -file05  -file07  -file09
```

I looked whats inside the first file using the `cat` command:

```bash
cat ./-file00
```

The output of the command above is s followed:

```text
�F1d�aIڗ�3n̊�����7��@Ԭ��ci^�
```

Because there is not a lost of files you could manually read through every file in order to locate the readable flag. This will not always be the case. You can not maunauly look into every file if there are hundereds of them. That is where the `file` command comes into play.

Since the files follow the same naming scheme of -file* followed by a digit. We could use the `file` command and list the files of the current working directory.

```bash
file ./*
```

The output of the command above is as followed:

```text
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
```

We located the human readable file by identifying `ASCII text` inside -file07

3. **Reading the Flag:**
I used the `cat` command followed by the path to the file `./` and the file name `-file07` to read its contents:

```bash
cat ./-file07
```

---

### 🔑 Key Features Learned
* `file` : The file command gives us the type of data of the file. Some examples would be: 'ELF', 'Perl script', 'ASCII text', 'data'and more.


### 🏁 Password/Flag
`💾 6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG`