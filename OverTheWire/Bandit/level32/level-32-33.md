# Bandit Level 32 → Level 33

### 🎯 Objective
After all this git stuff, it's time for another escape. Good luck!

### 🔍 Information & Clues
• **Host:** bandit.labs.overthewire.org
• **Port:** 2220
• **Username:** bandit32
• **Commands to research:** sh, man

### 💡 Thought Process & Steps
1. **Connecting & Identifying the Restricted Shell:**
I logged into bandit32 over SSH using the password retrieved from the previous level (pWuj5jBQ6IgV0NXwiH6g1pXRF8S1YvbT):

```bash
ssh bandit32@bandit.labs.overthewire.org -p 2220
```

Instead of a standard bash prompt, I was dropped into a custom restricted environment titled WELCOME TO THE UPPERCASE SHELL (>>). Running standard lowercase commands like ls, sudo -l, or whoami returned errors such as sh: 1: LS: Permission denied—revealing that the wrapper automatically converts every alphabetical character typed into uppercase before passing it to /bin/sh.

2. **Escaping the Uppercase Shell via $0:**
Because Linux commands are case-sensitive, alphabetical commands fail when converted to uppercase—however, special shell parameters (symbols and numbers) are unaffected by case conversion. In POSIX shells (sh/bash), $0 is a special variable that holds the name or path of the currently running shell or script itself (in this case, sh).
Entering $0 bypassed the uppercase filter and spawned a standard, unrestricted interactive sh prompt ($):

```bash
$0
```

3. **Enumerating Privileges & Reading the Flag:**
Once inside the standard $ shell, I ran ls -la and whoami to check my permissions:

```bash
ls -la
whoami
```

The uppershell binary in the home directory had the SUID (Set User ID) bit set (-rwsr-x---) and was owned by bandit33. Because uppershell executed with bandit33's privileges before spawning $0, my new shell inherited the effective user ID of bandit33.
With bandit33 permissions active, I read the password file directly from /etc/bandit_pass/:

```bash
cat /etc/bandit_pass/bandit33
```

This printed the final password to log into bandit33 and complete the Bandit wargame.

### 🔑 Key Features Learned
• **Special Shell Parameter $0:** Expands to the name of the current shell or shell script; executing $0 inside a restricted wrapper launches a fresh, unencumbered instance of the underlying shell (sh).
• **SUID (Set User ID) Privilege Inheritance:** When a binary with the SUID bit (s in the owner permissions) spawns a child shell without dropping privileges, that child shell retains the elevated permissions of the binary's owner (bandit33).
• **Input Sanitization Bypass:** Filters that only transform alphabetical characters can often be bypassed using non-alphabetic shell variables ($0, $$, $_).

### 🏁 Flag
`u4P2CyPOwPGLe94RdD9Uo2FxFwvnFswM`