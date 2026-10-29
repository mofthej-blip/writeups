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
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
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