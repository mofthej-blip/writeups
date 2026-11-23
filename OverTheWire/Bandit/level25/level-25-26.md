# Bandit Level 25 → Level 26

### 🎯 Objective
Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.

NOTE: if you’re a Windows user and typically use Powershell to ssh into bandit: Powershell is known to cause issues with the intended solution to this level. You should use command prompt instead.

### 🔍 Information & Clues
• **Host:** bandit.labs.overthewire.org
• **Port:** 2220
• **Username:** bandit25
• **Commands to research:** ssh, scp, icacls, more, vi

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH via Windows Command Prompt (CMD) rather than PowerShell so terminal resizing would work properly:

```bash
ssh bandit25@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration & Preparing the Key (Windows):**
Inside the bandit25 home directory, there is a private SSH key named bandit26.sshkey. Checking `/etc/passwd` reveals that bandit26's login shell is `/usr/bin/showtext`, a script that uses the more command to print ASCII art and immediately terminates the session. Because OverTheWire blocks localhost SSH connections, I transferred the key to my Windows machine and locked down its permissions:

• Download the key locally using scp:

```cmd
scp -P 2220 bandit25@bandit.labs.overthewire.org:bandit26.sshkey .
```

• Fix permissions on Windows using icacls:

```cmd
icacls bandit26.sshkey /inheritance:r
icacls bandit26.sshkey /grant:r "%username%":R
```

3. **Breaking Out & Reading the Flag:**
• Shrink the terminal window: I dragged the border of my Command Prompt window until it was only 4–5 lines tall so the more command would pause instead of exiting.
• Connect as bandit26:

```cmd
ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
```

• Escape more via vi: Once the `--More--` prompt appeared, I pressed v to open the file in the vi editor, maximized the window, and spawned an interactive bash shell:

```vim
:set shell=/bin/bash
:shell
```

• Read the password:

```bash
cat /etc/bandit_pass/bandit26
```

### 🔑 Key Features Learned
• **Restricted Shells (more):** The more pager pauses output when text exceeds the terminal window height. Pressing v inside more launches the vi text editor, which can then execute system commands and spawn an interactive `/bin/bash` shell.
• **Windows SSH Key Permissions (icacls):** Windows OpenSSH rejects private keys that are accessible to other users or groups. Using `icacls with /inheritance:r and /grant:r "%username%":R` mimics Linux's chmod 400 permission restriction.
• **Secure File Transfer (scp):** Using scp avoids formatting or encoding corruption that commonly occurs when copy-pasting private keys through Windows Notepad.

### 🏁 Password/Flag
`💾 jHdv2ELQhT22BkprMNDjybZDAkw1zeBJ`