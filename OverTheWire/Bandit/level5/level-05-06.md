# Bandit Level 5 → Level 6

### 🎯 Objective
The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

human-readable
1033 bytes in size
not executable


### 🔍 Information & Clues
* **Host:** `bandit.labs.overthewire.org`
* **Port:** `2220`
* **Username:** `bandit5`
* **Commands to research:** `find`, `grep`

---

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
Once inside, I ran `ls` to see the files, a single file named `inhere` was listed.

I used the `cd` command to move to the `inhere` directory and used the `ls -al` command to read all its contents: 

```text
bandit5@bandit:~/inhere$ ls -al
total 88
drwxr-x--- 22 root bandit5 4096 Sep 26 21:54 .
drwxr-xr-x  3 root root    4096 Sep 26 21:54 ..
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere00
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere01
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere02
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere03
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere04
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere05
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere06
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere07
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere08
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere09
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere10
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere11
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere12
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere13
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere14
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere15
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere16
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere17
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere18
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 maybehere19
```

I investigated further and chaned to the first directory listed in the current working directory `inhere`. Once inside i listed all the files using the `ls -al` command and the output is as followed:

```text
bandit5@bandit:~/inhere$ cd maybehere01
bandit5@bandit:~/inhere/maybehere01$ ls -al
total 80
-rwxr-x---  1 root bandit5 6028 Sep 26 21:54 -file1
-rw-r-----  1 root bandit5  288 Sep 26 21:54 -file2
-rwxr-x---  1 root bandit5 9641 Sep 26 21:54 -file3
drwxr-x---  2 root bandit5 4096 Sep 26 21:54 .
drwxr-x--- 22 root bandit5 4096 Sep 26 21:54 ..
-rwxr-x---  1 root bandit5 8944 Sep 26 21:54 .file1
-rw-r-----  1 root bandit5 3070 Sep 26 21:54 .file2
-rwxr-x---  1 root bandit5 3792 Sep 26 21:54 .file3
-rwxr-x---  1 root bandit5 4139 Sep 26 21:54 spaces file1
-rw-r-----  1 root bandit5 4543 Sep 26 21:54 spaces file2
-rwxr-x---  1 root bandit5 8834 Sep 26 21:54 spaces file3
bandit5@bandit:~/inhere/maybehere01$
```

With every directory in the `inhere` folder containing its own list of files, it would not be very efficient to read through them manualy. If we remember the clues that were given to us in the beginning of the challenge we could use the `find` command to locate the specific file. The file we are looking for has the following three properties : `human-readable`, `1033 bytes in size`, and it is `not executable`.

To apply the properties given to use we need to specify them using the `-type f`, `-size 1033c`, and `! executable` flags respectively.

We use `-size 1033` to look for the file-size requirement.
We use `-type f` to only look at files.
We use `-exec file '{}' \;`, to execute the file command and get the file data type. After that, we simply need to filter the output for the file type 'ASCII' again.
We use the the `grep` command along with the pipe command `|` to search for specific text in the command.

```bash
find . -type f -size 1033c ! -executable -exec file '{}' \; | grep ASCII
```

The output of the above command is as followed: 

```text
./maybehere07/.file2: ASCII text, with very long lines (1000)
```

We identified the path to the file and the specific file that meets the exact properties we specified in the command


3. **Reading the Flag:**
Inside the `inhere` working directory I used the `cat` command foolowed by the path to the file `./maybehere07` and the file name `/.file2` to read its contents:

```bash
cat ./maybehere07/.file2
```

---

### 🔑 Key Features Learned
* `find` : the find command could be used to search for the path to a specific file or diractory with specified flags to narrow down the output
* `|` : The pipe operator is used to send the output of one command to andother command as input
* `grep` : The grep command is used to search for specific patterns or strings. 

### 🏁 Password/Flag
`💾 pXa26xhMWaC2SvDotA4r9EgZkulOeSBW`