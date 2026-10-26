# Bandit Level 10 → Level 11

### 🎯 Objective
The password for the next level is stored in the file data.txt, which contains base64 encoded data


### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit10`
* **Commands to research:** 

---

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit10@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
Once inside, I ran `ls` to see the files, a single file named `data.txt` was listed.

I ran the `cat data.txt` command and the following output was given:

```text
VGhlIHBhc3N3b3JkIGlzIHBZZk9ZNkh3VXNEajVyTDlVdnloVTdNQ212OHZONVJvCg==
```

This is a base64 encoded string. This can easily be identified by the two equal signs at the end of the string `==`. For us to get the passowrd we need to decode the string. This can be achieved by using the `base64` command with the `-d, --decode` flag set.

3. **Reading the Flag:**
I used the `base` command, followed by the file name `data.txt` and the `-d`flag to read its contents:

```bash
base64 data.txt -d
```

The output of the command above is as followed : 

```text
The password is <REDACTED>
```

---

### 🔑 Key Features Learned
* "" : 

### 🏁 Password/Flag
`💾 pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro`