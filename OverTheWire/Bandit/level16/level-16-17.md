# Bandit Level 16 → Level 17


### 🎯 Objective
The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
Helpful note: Getting "DONE", "RENEGOTIATING" or "KEYUPDATE"? Read the "CONNECTED COMMANDS" section in the manpage.

### 🔍 Information & Clues
* **Host** : `bandit.labs.overthewire.org`
* **Port**: `2220`
* **Username** : `bandit16`
* **Commands to research** : `nmap`

### 💡 Thought Process & Steps

1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit16@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
Testing 1000 ports individually is not efficient, so we use an automated scanning tool to locate the active ports for us. We will use nmap to scan through the specified list of ports and identify which unique port returns our password:

```bash
nmap -sV -p 31000-32000 localhost
```

The following output was returned:
```text
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
```

We are looking for active services that allow meaningful connections. Ports 31518 and 31790 are the encrypted ones we need to focus on. Since the service running on port 31518 is an ssl/echo server, it will just resend whatever text payload we send to it. Therefore, the target service port we are looking for is port 31790.

```bash
openssl s_client -ign_eof -connect localhost:31790
```

3. **Reading the Flag:**
After you have successfully connected to localhost through port 31790 with openssl, you can submit the password string retrieved from the previous level into the open connection terminal, and the next credentials will be provided to you.

```bash
kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V
```

We were given an OpenSSH private key file to log into the next level.

```text
-----BEGIN OPENSSH PRIVATE KEY-----

<REDACTED>

-----END OPENSSH PRIVATE KEY-----
```

Change the file system permissions locally using this PowerShell one-liner:

```bash
$Path = ".\bandit17_private_key.txt"; $Acl = Get-Acl $Path; $Acl.SetAccessRuleProtection($true, $false); $User = [System.Security.Principal.NTAccount]($env:USERPROFILE -split '\\')[-1]; $AccessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($User, "FullControl", "Allow"); $Acl.SetAccessRule($AccessRule); Set-Acl $Path $Acl
```

Log into bandit17 via SSH utilizing the newly configured private key:

```bash
ssh -i bandit17_private_key.txt bandit17@bandit.labs.overthewire.org -p 2220
```

You are now logged in and you can read the static password saved on the current working directory:

```bash
cat .bandit16.password
```

### 🔑 Key Features Learned

* "nmap -sV -p <port range> TARGET_IP" : Instructs Nmap to scan a restricted range of network ports while actively probing them to perform application banner grabbing, exposing the exact software service types and version configurations (such as identifying whether an open socket uses standard TCP text routing or an encrypted TLS layer).

### 🏁 Password/Flag
`💾 kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V`