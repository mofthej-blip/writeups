# Bandit Level 22 → Level 23

### 🎯 Objective
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

### 🔍 Information & Clues
• **Host:** bandit.labs.overthewire.org
• **Port:** 2220
• **Username:** bandit22
• **Commands to research:** md5sum, cut, echo, whoami

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH and the password obtained from Level 21:

```bash
ssh bandit22@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
I navigated to the cron configuration directory and read the job file designated for the next level:

```bash
cd /etc/cron.d
cat cronjob_bandit23
```
The file revealed that a script runs every minute as user bandit23: /usr/bin/cronjob_bandit23.sh.
I inspected the script to find out how it writes the password:

```bash
cat /usr/bin/cronjob_bandit23.sh
```

Script Logic:

```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

The script uses the user executing it (myname) to dynamically generate an MD5 hash string, which it then uses as a filename inside the /tmp/ directory.

3. **Reading the Flag:**
When I manually ran ./cronjob_bandit23.sh, it ran under my current privileges (bandit22), which only copied my current level's password into /tmp/8169b67bd894ddbb4412f91573b38db3.
To find where the automated background cron job saves the password for the next level, I needed to mimic the script logic manually as if myname were bandit23. I generated the MD5 hash string using the identical format:

```bash
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

Output Hash: 8ca319486bfbbc3663ea0fbe81326349
Knowing that the system cron task automatically saves the bandit23 password to this exact filename hash, I read the target file:

```bash
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

### 🔑 Key Features Learned
* whoami: Prints the effective user name of the current user logged into the terminal session.
* md5sum: Computes and checks MD5 message digest hashes, frequently used here as a deterministic obfuscation method to generate unique filenames.
* cut: A command-line utility used to extract sections from each line of input. In this level, cut -d ' ' -f 1 splits the output string by space delimiters (-d ' ') and extracts only the first field (-f 1), stripping away the trailing hyphen from the md5sum command output.

### 🏁 Password/Flag
`💾 gKXDTAXnIz3OBxiPjRZ2uqutUlPZrBsw`