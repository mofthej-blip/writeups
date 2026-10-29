# Bandit Level 14 → Level 15

### 🎯 Objective
The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.


### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit14`
* **Commands to research:** `socat`, `nc`, `openssl`, `telnet` 

---

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit14@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
We can immediately connect to our localhost, as instructed by our instructions, since there are no directories or files located on our terminal.

The following tools can used to form a connection to retreive our next password:

```bash
telnet localhost 30000
```

```bash
socat - TCP:localhost:30000
```

```bash
nc localhost 30000
```

```bash
openssl s_client -ign_eof -connect localhost:30000
```

3. **Reading the Flag:**
After you have succesfully connected to the localhost throught port 30000 with one of the provided tools, you can submit the flag we retrieved from `/etc/bandit_pass/bandit14` onto the terminal and the next password will be provided to you.

```bash
aaWecNkG4FhxJQxz07uiwzVP6bJiYS65
```

---

### 🔑 Key Features Learned
* "Telnet" : is a legacy network protocol designed to establish an interactive, bidirectional text communication channel over a raw TCP network connection. The command initiates a TCP three-way handshake with port 30000 on the loopback address, mapping the terminal's standard input and output directly to the unencrypted data stream.
* "Socat (Socket Cat)" : is a powerful, multipurpose relay utility that establishes two independent byte streams and transfers data between them. This command links the terminal's interactive standard input and output (-) directly to a raw TCP socket connection established at localhost:30000.
* "Netcat (nc)" : is a versatile networking utility used for reading from and writing to network connections using the TCP or UDP protocols. The utility opens a raw TCP connection to the specified local port, allowing cleartext data to be piped or typed manually directly into the listening network service.
* "OpenSSL" : is a robust, commercial-grade security toolkit that implements the Transport Layer Security (TLS) and Secure Sockets Layer (SSL) cryptographic protocols. The s_client flag attempts to initiate a secure SSL/TLS handshake with the target port, while the -ign_eof modifier forces the connection to stay open even after the end-of-file marker is reached on standard input.


### 🏁 Password/Flag
`💾 pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7`