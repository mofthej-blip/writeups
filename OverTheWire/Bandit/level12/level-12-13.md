# Bandit Level 12 → Level 13


# 🎯 Objective
The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command "mktemp -d". Then copy the datafile using cp, and rename it using mv (read the manpages!)

# 🔍 Information & Clues
• Host: bandit.labs.overthewire.org
• Port: 2220
• Username: bandit12
• Commands to research: mktemp, cp, mv, xxd, file, gzip, bzip2, tar, cat

# 💡 Thought Process & Steps

1. Connecting:
I logged into the level using SSH:

```bash
ssh bandit12@bandit.labs.overthewire.org -p 2220
```

2. Enumeration & Setup:
Once inside, I verified the presence of data.txt and examined its header. It was confirmed to be a text hexdump.

```bash
cat data.txt | head -n 10
```

```text
00000000: 1f8b 0808 ff05 de6a 0203 6461 7461 322e  .......j..data2.
00000010: 6269 6e00 0149 02b6 fd42 5a68 3931 4159  bin..I...BZh91AY
00000020: 2653 59b8 b8d3 6700 001d 7fff ffde cfce  &SY...g.........
00000030: 98f7 e7f5 b37f fe14 ffff effa d3ef ffa3  ................
00000040: fede b3f7 f3db fddf 7f77 fdfb b001 3b56  .........w....;V
00000050: 0400 6864 0f53 4681 a032 0d19 0006 8032  ..hd.SF..2.....2
00000060: 068d 0034 0000 d034 0018 8188 0000 0c02  ...4...4........
00000070: 6864 68c2 1a68 c353 4d10 68d0 d034 341a  hdh..h.SM.h..44.
00000080: 0323 4340 64d0 681a 1a64 3200 001a 340d  .#C@d.h..d2...4.
00000090: 001a 640d 0068 68d0 68d3 468d 3d41 a69b  ..d..hh.h.F.=A..
```

Because modifying data files requires write access, I moved to /tmp, generated a unique sandbox workspace, and reverted the hexdump using xxd:

```bash
cd /tmp
mktemp -d
# Created: /tmp/tmp.miNJHTL28i
cd /tmp/tmp.miNJHTL28i
cp ~/data.txt .
mv data.txt hexdump_data
xxd -r hexdump_data compressed_data
```

3. Decompression Loop:
With the raw binary file recovered, I began an iterative loop of identifying the file type, appending the correct extension, and extracting the archive.
• Layer 1 (Gzip):

```bash
file compressed_data
# Output: gzip compressed data, was "data2.bin"
mv compressed_data compressed_data.gz
gzip -d compressed_data.gz
```

• Layer 2 (Bzip2):

```bash
file compressed_data
# Output: bzip2 compressed data
mv compressed_data compressed_data.bz2
bzip2 -d compressed_data.bz2
```

• Layer 3 (Gzip):

```bash
file compressed_data
# Output: gzip compressed data, was "data4.bin"
mv compressed_data compressed_data.gz
gzip -d compressed_data.gz
```

• Layer 4 (Tar Archive):

```bash
file compressed_data
# Output: POSIX tar archive (GNU)
mv compressed_data compressed_data.tar
tar -xf compressed_data.tar
# Extracted: data5.bin
```

• Layer 5 (Tar Archive):

```bash
file data5.bin
# Output: POSIX tar archive (GNU)
mv data5.bin data5.bin.tar
tar -xf data5.bin.tar
# Extracted: data6.bin
```

• Layer 6 (Bzip2):

```bash
file data6.bin
# Output: bzip2 compressed data
mv data6.bin data6.bin.bz2
bzip2 -d data6.bin.bz2
# Extracted: data6.bin
```

• Layer 7 (Tar Archive):

```bash
file data6.bin
# Output: POSIX tar archive (GNU)
mv data6.bin data6.bin.tar
tar -xf data6.bin.tar
# Extracted: data8.bin
```

• Layer 8 (Gzip):

```bash
file data8.bin
# Output: gzip compressed data, was "data9.bin"
mv data8.bin data8.bin.gz
gzip -d data8.bin.gz
# Extracted: data8.bin
```

4. Reading the Flag:
Finally, checking data8.bin revealed it was clean ASCII text containing the plain-text password.

```bash
file data8.bin
# Output: ASCII text
cat data8.bin
```

# 🔑 Key Features Learned

• "xxd -r" : Reverses a plain-text hex dump back into its original raw binary layout.
• "file": Probes the magic bytes of an unknown file to accurately determine its file structure, regardless of its current filename extension.
• "Multi-engine Archiving" : Navigated dynamic, multi-layered compressions using gzip -d, bzip2 -d, and tar -xf by manually maintaining corresponding file extension configurations.

# 🏁 Password/Flag

`💾 qQYQiHOBPR8zR61qxYqX45quvihF2uzk`