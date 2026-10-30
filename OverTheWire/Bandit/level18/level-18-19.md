# Bandit Level 18 → Level 19

### 🎯 Objective
The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

### 🔍 Information & Clues
• Host: bandit.labs.overthewire.org
• Port: 2220
• Username: bandit18
• Commands to research: ssh, ls, cat

### 💡 Thought Process & Steps
1. Connecting:
I attempted to log into the level using standard interactive SSH:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220
```

2. Enumeration:
Upon providing the password, the connection immediately closed. This is because the user's .bashrc profile file was modified to run a logout or exit command automatically upon setting up an interactive login shell.

3. Inspecting the Directory:
To bypass the .bashrc execution, I appended an explicit command to the end of the SSH connection string. This executes the command directly without initializing an interactive terminal session. First, I listed the contents of the home directory to find the target file:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 'ls'
```

Output:

```text
readme
```

4. Reading the Flag:
After verifying the file existed via the non-interactive ls execution, I ran the command again, replacing ls with cat readme to print out the password for Level 19:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 'cat readme'
```

### 🔑 Key Features Learned
* "Bypassing Profile Scripts" : Non-interactive execution can be leveraged to circumvent forced logouts or strict interactive environment constraints hardcoded inside shell initialization files like .bashrc or .profile.

### 🏁 Password/Flag
`KpsOfPkcP7i1FlIExk2QEjyt6dw8dxZI`