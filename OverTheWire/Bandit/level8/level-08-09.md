# Bandit Level 8 → Level 9

### 🎯 Objective
The password for the next level is stored in the file data.txt and is the only line of text that occurs only once


### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit8`
* **Commands to research:** `sort`, `uniq`

---

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit8@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
Once inside, I ran `ls` to see the files, a single file named `data.txt` was listed.

The `sort` command displays a file in consecutive order. The `uniq` command is used to display omitted or repeated lines in a file. You need to specify the type of output you would like to receive with the `uniq` command, since there are multiple output possibilities. By adding the `-u, --unique` flag to the `uniq` command, your output wil only display the unique lines in the output.

3. **Reading the Flag:**
I used the `sort` command, followed by the file name `data.txt` to organise the file. I piped `|` the `uniq`, with the -u` flag set, command to the end of the sort command to display only the unique lines in the output. Its contents read:

```bash
sort data.txt | uniq -u
```

The output only displayed a single password for the next level.

---

### 🔑 Key Features Learned
* "sort" :A tool that re-orders lines of text alphabetically, numerically, or chronologically so that matching lines are grouped together.
* "uniq" : A tool that scans adjacent lines of text and deletes any consecutive duplicates it finds.
* "uniq -u" : • A modifier that filters out all duplicates entirely, leaving behind only the lines that were completely unique from the start.

### 🏁 Password/Flag
`💾 EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl`