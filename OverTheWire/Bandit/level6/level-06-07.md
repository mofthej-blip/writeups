# Bandit Level 6 → Level 7

### 🎯 Objective
The password for the next level is stored somewhere on the server and has all of the following properties:

owned by user bandit7
owned by group bandit6
33 bytes in size


### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit6`
* **Commands to research:** `None`

---

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
Once inside, I ran `ls -al` to see the files, however not a single file nor directory was listed.

I shifted my focus to what the instructions of the challenge said. The file is stored somewhere on the server with the following properties : `owned by user bandit7`, `owned by group bandit6`, `33 bytes in size`.

If we apply the same concept as the previous level, we could use the `find` command to locatee the path to the specified file, if we apply the correct flag specifications.

We wil use the `-user` flag to locate the owner
We will use the `-group` flag to locate the specific identified group
Lastely we wil use the `-size` flag to specify the size of the file

```bash
find / -user bandit7 -group bandit6 -size 33c 
```

But once we rand the command a bunch of nonsence files started to appear that are not relevant to what we are looking for. To get rid of these file we can use the `2>/dev/null` command to send those files to the `/dev/null` file.

```bash
find / -user bandit7 -group bandit6 -size 33c 2>dev/null
```

The output above gave the following output

```text
/var/lib/dpkg/info/bandit7.password
```

3. **Reading the Flag:**
I used the `cat` command, followed by the path to the file `/var/lib/dpkg/info/` and then the file name `bandit7.password` to read its contents:

```bash
cat /var/lib/dpkg/info/bandit7.password
```

---

### 🔑 Key Features Learned
* `2>/dev/null` : If a file output lists a bunsh of nonsence files, you can get rid of them by sending them to the `/dev/null` directory.

### 🏁 Password/Flag
`💾 Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3`