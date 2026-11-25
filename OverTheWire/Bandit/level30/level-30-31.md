# Bandit Level 30 → Level 31

### 🎯 Objective
There is a git repository at ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo via port 2220. The password for the user bandit30-git is the same as for the user bandit30.
Clone the repository and find the password for the next level.

### 🔍 Information & Clues
• **Host:** bandit.labs.overthewire.org
• **Port:** 2220
• **Username:** bandit30 / bandit30-git
• **Repository Path:** /home/bandit30-git/repo
• **Commands to research:** git

### 💡 Thought Process & Steps

1. **Cloning the Repository & Noticing the Object Count:**
After removing the old repo folder from the previous level (rmdir /s /q repo), I cloned the Level 30 repository directly onto my local machine using port 2220:

```bash
git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo
```

When prompted, I entered the bandit30 password (jq9Dfg2rXsfYsWMgFuKlXhphjdH7USgX). During the download, Git reported Enumerating objects: 4, done.—an immediate clue that the repository contained four Git objects.

2. **Initial File Enumeration & Commit History Check:**
I moved into the cloned directory (cd repo), listed the files (dir on Windows / ls on Linux), and read the only visible file, README.md:

```bash
cd repo
type README.md
```

The file only contained a decoy message: just an epmty file... muahaha. Next, I inspected the commit history using git log -p to see if anything had been deleted or altered in an earlier commit:

```bash
git log -p
```

There was only a single initial commit. In Git's internal data model, one standard commit with one file accounts for three objects (one commit object, one tree object representing the directory, and one blob object representing README.md). Because the clone output showed four objects, one hidden object remained unaccounted for—pointing directly to a Git tag.

3. **Discovering & Inspecting the Git Tag:**
In Git, a tag is a human-readable reference (like a bookmark) attached to a specific point or object in a repository's history, commonly used to mark release versions. Unlike normal commits, tags can point directly to standalone blob objects containing raw text rather than files in your working directory.

First, I ran git tag to list all tag names stored in the repository, which revealed a single tag named secret:

```bash
git tag
```

Next, I used git show—a command that displays metadata and raw contents for any Git object (commits, trees, blobs, or tags)—to inspect the secret tag:

```bash
git show secret
```

Instead of pointing to a standard commit, the secret tag pointed directly to a standalone blob object containing the plaintext password for bandit31.

### 🔑 Key Features Learned
• **Git Objects (blob, tree, commit, tag):** Git stores everything in its .git/objects database as four core object types: blobs (file contents), trees (directory structures), commits (snapshots with history metadata), and tags (named references or annotated pointers).
• **git tag:** Lists all tags present in a repository.
• **git show <object>:** Inspects and prints the contents of a specified Git object, such as a commit hash, branch, or tag name.

### 🏁 Flag
`82NkymblpGBYmIXG6ZQ8YldBYstHpfUf`