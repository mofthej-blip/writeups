# Bandit Level 20 → Level 21

### 🎯 Objective
There is a setuid binary in the home directory that does the following: it makes a connection to localhost on the port you specify as a command-line argument. It then reads a line of text from the connection and compares it to the password for the current level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

NOTE: Try connecting to your own network daemon to see if it works as you think.

### 🔍 Information & Clues
• Host: bandit.labs.overthewire.org
• Port: 2220
• Username: bandit20
• Commands to research: nc, echo, suconnect

### 💡 Thought Process & Steps
1. Connecting:
I logged into the level using SSH:

```bash
ssh bandit20@bandit.labs.overthewire.org -p 2220
```

2. Enumeration:
Once inside, I ran `ls` to list the files and found a single executable named suconnect.

I ran the binary without arguments to see how to use it:

```bash
./suconnect
```

The output instructions were:

```text
Usage: ./suconnect <portnumber>
This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.
```

I opened a second terminal window and logged into the bandit20 account again. In this new terminal, I started a Netcat (nc) listener to intercept the connection from suconnect. I also piped my current password into it so it would automatically send it upon connection:

```bash
echo "bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY" | nc -lnvp 1234
```

Back in my first terminal, I executed suconnect and pointed it to my listener port:

```bash
./suconnect 1234
```

The terminal output confirmed a successful match:

```text
Read: bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY
Password matches, sending next password
```

3. Reading the Flag:
To retrieve the new password, I checked my second terminal window where the Netcat listener received the response transmitted by the binary:

```text
Listening on 0.0.0.0 1234
Connection received on 127.0.0.1 48182
4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA
```

### 🔑 Key Features Learned
* Setuid Binaries: Executables that run with the permissions of the file owner rather than the user executing them, allowing privileged actions like reading restricted password files.
* Network Inter-process Communication: Utilizing netcat (nc) to listen on a local port allows different terminal sessions or local binaries to communicate and exchange data seamlessly over TCP.

### 🏁 Password/Flag
`💾 bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY`