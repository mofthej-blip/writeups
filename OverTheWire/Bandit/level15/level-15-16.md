# Bandit Level 15 → Level 16

### 🎯 Objective
The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.

Helpful note: Getting "DONE", "RENEGOTIATING" or "KEYUPDATE"? Read the "CONNECTED COMMANDS" section in the manpage.


### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit15`
* **Commands to research:** 

---

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit15@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
the server on port 30001 uses SSL/TLS encryption. Because of this, raw TCP tools like telnet and standard nc will fail to communicate because they cannot perform the necessary cryptographic handshake

```bash
openssl s_client -ign_eof -connect localhost:30001
```

3. **Reading the Flag:**
After you have succesfully connected to the localhost throught port 30001 with `openssl`, you can submit the flag we retrieved from the previous level onto the terminal and the next password will be provided to you.

```bash
pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7
```

---

### 🔑 Key Features Learned
* ‘-’ is a special symbol in Linux. So it is not recommended to start a file name with this symbol.
* ‘-’ is the standard option character. We have already seen it for adding so-called flags for specific options to a command (Like the -p flag for choosing a port for the ssh command.). standard option. Because of this, files with this symbol as the first character cannot just be referenced as other files.

### 🏁 Password/Flag
`💾 kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V`