# Bandit Level 9 → Level 10

### 🎯 Objective
The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several '=' characters.


### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit9`
* **Commands to research:** `strings`

---

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit9@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
Once inside, I ran `ls` to see the files, a single file named `data.txt` was listed.

I used the `xxd` command followed by the file name `data.txt`, along with the head -n 20 piped afterwards. `xxd` dumps the file in hex and ASCII, `head` reads the top of the output, and the `-n 20` flag restricts it to the first 20 lines.

```bash
xxd data.txt | head -n 20
```

The output of the command obove is as followed:

```text
00000000: 3d65 aeb2 ce5a 8fb5 151b 0fcc 8e16 81e9  =e...Z..........
00000010: 7031 823e 7063 74b9 d764 e359 e1fe 6ff4  p1.>pct..d.Y..o.
00000020: 0a7a 1785 0e1c 3be5 c3d1 3f6d 698f 02ec  .z....;...?mi...
00000030: 673c a9d2 5305 3e79 526f 591b 4903 6497  g<..S.>yRoY.I.d.
00000040: 9c5c 6194 8533 dc17 3b3d 8c17 7de4 e2d2  .\a..3..;=..}...
00000050: 2f86 95e6 b97b 2252 26dc 81da b827 5a60  /....{"R&....'Z`
00000060: c871 bdc3 2c3a 3a0b 095d b5b1 6528 2028  .q..,::..]..e( (
00000070: d8e4 2ec2 5780 3fc7 8ffb fa81 e7f7 f9c4  ....W.?.........
00000080: a242 4314 f84a d319 c780 32d4 9939 925f  .BC..J....2..9._
00000090: 815f 838e 3b48 5385 3393 8006 10c7 5353  ._..;HS.3.....SS
000000a0: 19d9 8dbc 9a98 21a4 b468 4fba ab9e 555c  ......!..hO...U\
000000b0: 6851 820c 2dd7 0f6b cd31 f658 10b4 df22  hQ..-..k.1.X..."
000000c0: 78ba 3fa7 7587 13f2 0a28 5d21 b890 97d5  x.?.u....(]!....
000000d0: 3937 ea89 bd97 c1d3 04b2 5704 82c1 5c11  97........W...\.
000000e0: b089 71c9 9cdf a6c3 a6e3 9271 4fd1 e7e1  ..q........qO...
000000f0: 6ea3 0d62 f2fc 12f4 0739 8d1b d985 0441  n..b.....9.....A
00000100: 0e6b 2f32 083d 91c4 d50e ddb3 ce49 034d  .k/2.=.......I.M
00000110: 2e37 8e6a faa3 c137 123c 9ce3 8b49 65be  .7.j...7.<...Ie.
00000120: c509 c4ae e848 ff63 3a24 5e51 5fa6 b37e  .....H.c:$^Q_..~
00000130: 9574 9757 e70c d308 bacc 16b6 9c75 aaa9  .t.W.........u..
```

So in order for us to read the human-readable contents we need to use the `strings` command. And since the instructions gave us a hint that the password would be precedes by a few equal `=` mark, we can pipe the grep command at the end of the line to get the password more easily. 

3. **Reading the Flag:**
I ran `strings data.txt | grep ====` to extract the hidden password: `strings` scans the binary file and extracts readable text, `grep` filters that text down to matching patterns, and the `====` argument targets the specific prefix surrounding the flag.

```bash
strings data.txt | grep ====
```

---

### 🔑 Key Features Learned
* "strings" : A tool that scans binary or non-text files and extracts any readable text hidden inside them, filtering out all the unreadable computer code. This command allows you to extract human-readable tet in a file
* "xxd" : A command that converts a file into a hex dump, showing its raw binary data as hexadecimal and text columns.
* "head" : A command that outputs the very beginning (the top lines) of a file or text stream.
* "head -n" : An option for head that specifies the exact number of lines to display instead of the default ten.

### 🏁 Password/Flag
`💾 B0s2khmbT9u0geKuOoVGW3JZKhndE3BG`