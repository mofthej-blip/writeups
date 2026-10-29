# Bandit Level 13 → Level 14

### 🎯 Objective
The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Look at the commands that logged you into previous bandit levels, and find out how to use the key for this level.
If you need help with this level: a hint file can be found in the home directory.
Make sure to read the error messages as they are informative.


### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit13`
* **Commands to research:** 

---

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit13@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
Once inside, I ran `ls` to see the files, two files named `sshkey.private` and `HINT` were listed.

I used `cat` to read the content of the `HINT` file and it contained the following data:

```text
If you have trouble with this level, note the following:

1) As for all other levels, this level has a website with information:
   https://overthewire.org/wargames/bandit/bandit14.html
2) No, the level is not broken. To verify, see:
   https://status.overthewire.org/
3) The current version of OverTheWire prevents logging in from one
   level to another via localhost. Log out, and see 1)
4) If you get errors, read the error message on your screen.
   We mean it!
```

Next i used `cat` to read the contents of the sshkey.private file. Once the content loaded i was presented with the ssh key i would use to log into the next level(level 14).

```text
-----BEGIN OPENSSH PRIVATE KEY-----

<REDACTED>

-----END OPENSSH PRIVATE KEY-----
```

To solve the OverTheWire Bandit Level 13 challenge on Windows, we had to bypass a server-side block on localhost connections by downloading the sshkey.private file text locally, which meant configuring the file to satisfy the strict security compliance rules of the Windows OpenSSH client. I opened a notepad textfile via my terminal to paste the private SSH key for local use:

```bash
notepad bandit13_private_key.txt
```

Because Windows automatically applies inherited folder permissions to new files-allowing background system groups like Everyone or Administrators to read them-the SSH client initially rejected the private key as insecure and defaulted to a password prompt. 

By using a PowerShell script to disable inheritance, strip out all external access groups, and grant exclusive "Full Control" permissions solely to your individual user account, we successfully verified the key's privacy to the system, allowing a direct, passwordless SSH login into Bandit Level 14.

```bash
$Path = ".\bandit13_private_key.txt"; $Acl = Get-Acl $Path; $Acl.SetAccessRuleProtection($true, $false); $User = [System.Security.Principal.NTAccount]($env:USERPROFILE -split '\\')[-1]; $AccessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($User, "FullControl", "Allow"); $Acl.SetAccessRule($AccessRule); Set-Acl $Path $Acl
```

With everything set i can now log into bandit14 using the private SSH key we were given

```bash
ssh -i bandit13_private_key.txt bandit14@bandit.labs.overthewire.org -p 2220
```

I navigated to `/etc/bandit_pass/` where the passowrd to log into bandit14 is located.


3. **Reading the Flag:**
I used the `cat` command, followed by the file name `bandit14` to read the password:

```bash
cat bandit14
```

---

### 🔑 Key Features Learned
* "ssh -i" : A Linux SSH feature that uses a cryptographic key file instead of a password to authenticate users.
* "localhost / 127.0.0.1" : A virtual network interface that allows a Linux system to route network traffic back to itself.
* "/etc/" : The standard Linux directory structure used to isolate host-specific system configurations and protected credentials.

### 🏁 Password/Flag
`💾 aaWecNkG4FhxJQxz07uiwzVP6bJiYS65`