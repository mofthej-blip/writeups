# Bandit Level 19 → Level 20

### 🎯 Objective
To gain access to the next level, you should use the setuid binary in the home directory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

### 🔍 Information & Clues
• Host: bandit.labs.overthewire.org
• Port: 2220
• Username: bandit19
• Commands to research: setuid

### 💡 Thought Process & Steps
1. Connecting:
I logged into the level via SSH:

```bash
ssh bandit19@bandit.labs.overthewire.org -p 2220 
```

2. Enumeration:
Once connected, I ran ls to check the directory contents and found a single executable named bandit20-do. Following the objective instructions, I executed it without any arguments to view its usage guidelines:

```bash
./bandit20-do 
```

The output was as follows:

```text
Run a command as another user. Example: ./bandit20-do whoami 
```

I followed the provided example to see which user context the binary executes under:

```bash
./bandit20-do whoami 
```

The output returned bandit20. Because this SUID binary runs with the privileges of the bandit20 user, I could leverage it to read files normally restricted to that user.To confirm the target file path, I listed the contents of the /etc/bandit_pass directory:

```text
bandit0 bandit13 bandit18 bandit22 bandit27 bandit31 bandit6 
bandit1 bandit14 bandit19 bandit23 bandit28 bandit32 bandit7 
bandit10 bandit15 bandit2 bandit24 bandit29 bandit33 bandit8 
bandit11 bandit16 bandit20 bandit25 bandit3 bandit4 bandit9 
bandit12 bandit17 bandit21 bandit26 bandit30 bandit5 
```

3. Reading the Flag:
Using the same principle, I passed the cat command through the wrapper script to read the password file for bandit20:

```bash
./bandit20-do cat /etc/bandit_pass/bandit20 
```

### 🔑 Key Features Learned
* SUID (Set User ID) Binaries: Learned how executables with the SUID bit set allow a user to run a command with the privileges of the file's owner (in this case, bandit20).
* Privilege Elevation: Leveraged a binary wrapper to execute administrative reading commands (cat) on restricted files.

### 🏁 Password/Flag
`💾 4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA`