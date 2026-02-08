# OverTheWire Bandit - Levels 0-11 Walkthrough

A comprehensive guide to solving Bandit wargame levels with detailed explanations of Linux commands and techniques.

---

## Table of Contents
- [Level 0 → 1: Basic File Operations](#level-0--1)
- [Level 1 → 2: Files with Special Characters (-)](#level-1--2)
- [Level 2 → 3: Files with Spaces](#level-2--3)
- [Level 3 → 4: Hidden Files](#level-3--4)
- [Level 4 → 5: Identifying Human-Readable Files](#level-4--5)
- [Level 5 → 6: Finding Files by Properties](#level-5--6)
- [Level 6 → 7: Filesystem-Wide Search](#level-6--7)
- [Level 7 → 8: Text Searching with grep/awk](#level-7--8)
- [Level 8 → 9: Finding Unique Lines](#level-8--9)
- [Level 9 → 10: Extracting Strings from Binaries](#level-9--10)
- [Level 10 → 11: Base64 Encoding/Decoding](#level-10--11)
- [Summary of Commands](#summary-of-commands-learned)
- [Tips & Resources](#tips-for-future-levels)

---

## Level 0 → 1

**Challenge:** Basic file operations - listing and reading files

### Concept
Basic file listing and reading commands in Linux.

### Commands Used
```bash
ls                    # List files in directory
cat <filename>        # Display file contents
```

### Key Learning
- `ls` lists files and directories with various options (`-l` for details, `-a` for hidden files)
- `cat` (concatenate) displays, combines, or creates file contents

### Solution
```bash
ls
cat readme
```

### Password
```
ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

---

## Level 1 → 2

**Challenge:** Reading files with special characters (dash `-`)

### Concept
Reading files with special characters (dash `-`) in their names.

### Challenge
The dash (`-`) is normally used for command options, causing issues when it's a filename.

### Solution Methods
```bash
cat ./-                    # Method 1: Relative path
cat /home/bandit1/-        # Method 2: Absolute path
cat < -                    # Method 3: Input redirection
cat -- -                   # Method 4: Using -- to stop option parsing
```

### Password
```
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

---

## Level 2 → 3

**Challenge:** Reading files with spaces in their names

### Concept
Reading files with spaces in their names.

### Challenge
Shells treat spaces as argument separators, requiring special handling.

### Solution Methods
```bash
cat './spaces in this filename'     # Method 1: Using quotes
cat ./spaces\ in\ this\ filename    # Method 2: Escaping spaces with \
```

### Password
```
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

---

## Level 3 → 4

**Challenge:** Finding and reading hidden files

### Concept
Working with hidden files in Linux.

### Key Learning
- Hidden files start with a dot (`.`)
- Not shown by default with `ls`
- `.` = current directory, `..` = parent directory

### Commands Used
```bash
cd inhere              # Change directory
ls -a                  # List all files including hidden
cat .hidden            # Read hidden file
```

### Password
```
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

---

## Level 4 → 5

**Challenge:** Identifying the only human-readable file

### Concept
Identifying human-readable files among multiple files.

### Key Learning
The `file` command analyzes file content (not just extension) to determine type.

### Commands Used
```bash
file ./-file*          # Check file types
cat ./-file07          # Read the human-readable file
```

### Password
```
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

---

## Level 5 → 6

**Challenge:** Finding a file with specific properties (size, readable, not executable)

### Concept
Using the `find` command to search files by specific criteria.

### Command Breakdown
```bash
find ~/inhere -type f -readable -size 1033c ! -executable
```

**Parameters:**
- `~/inhere` - Search starting directory
- `-type f` - Search for regular files
- `-readable` - Human-readable files only
- `-size 1033c` - Exactly 1033 bytes (c = bytes)
- `! -executable` - NOT executable

### Find Command Syntax
```bash
find <path> <options> <conditions> 2>/dev/null

# Examples:
-name "*.txt"          # Match .txt files
-size +1M              # Files larger than 1MB
-type d -name "dir"    # Find directories by name
2>/dev/null            # Suppress error messages
```

### Password
```
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

---

## Level 6 → 7

**Challenge:** Finding a file anywhere on the server with specific owner, group, and size

### Concept
Searching the entire filesystem for a file with specific properties.

### Command Breakdown
```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

**Parameters:**
- `/` - Search from root directory
- `-user bandit7` - Owned by user bandit7
- `-group bandit6` - Owned by group bandit6
- `-size 33c` - Exactly 33 bytes
- `2>/dev/null` - Hide permission errors

### Password
```
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

---

## Level 7 → 8

**Challenge:** Finding a password next to the word "millionth" in a large file

### Concept
Text searching and processing with `grep` and `awk`.

### Commands Reference

**grep - Search for patterns:**
```bash
grep "hello" file.txt              # Basic search
grep -i "error" log.txt            # Case-insensitive
grep -r "config" /etc              # Recursive search
grep -n "main" code.c              # Show line numbers
```

**awk - Text processing:**
```bash
awk '{print $1}' file.txt          # Print first column
awk '/error/ {print $0}' log.txt   # Print lines with "error"
awk -F ':' '{print $1}' /etc/passwd # Custom field separator
```

### Solution Options
```bash
grep 'millionth' data.txt
awk '/millionth/ {print $2}' data.txt
grep "millionth" data.txt | awk '{print $2}'
```

### Password
```
dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

---

## Level 8 → 9

**Challenge:** Finding the only unique line in a file

### Concept
Finding unique lines in a file using `sort` and `uniq`.

### Key Learning
- `sort` arranges lines alphabetically/numerically
- `uniq` removes consecutive duplicate lines (requires sorted input)

### Solution
```bash
cat data.txt | sort | uniq -u
```

**Command Breakdown:**
1. `cat data.txt` - Read file contents
2. `sort` - Sort lines (required for uniq to work properly)
3. `uniq -u` - Output only unique lines (appearing once)

### Password
```
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

---

## Level 9 → 10

**Challenge:** Finding human-readable strings in a binary file

### Concept
Extracting readable text from binary files.

### Key Learning
The `strings` command extracts human-readable text from binary files, useful for:
- Analyzing unknown files
- Debugging
- Finding hidden information in CTF challenges
- Reverse engineering

### Solution
```bash
strings data.txt | grep "==="
```

### Password
```
FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

---

## Level 10 → 11

**Challenge:** Decoding Base64 encoded data

### Concept
Base64 encoding and decoding.

### Key Learning
- Base64 is an encoding method (NOT encryption)
- Converts binary data to ASCII text
- Commonly used in emails, URLs, JSON for safe data transmission

### Commands
```bash
base64 file.txt        # Encode to Base64
base64 -d encoded.txt  # Decode from Base64
```

### Solution
```bash
cat data.txt | base64 -d
```

### Password
```
dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

---

## Summary of Commands Learned

| Command | Purpose |
|---------|---------|
| `ls` | List directory contents |
| `cat` | Display file contents |
| `cd` | Change directory |
| `file` | Determine file type |
| `find` | Search for files by criteria |
| `grep` | Search for patterns in text |
| `awk` | Text processing and formatting |
| `sort` | Sort lines of text |
| `uniq` | Filter duplicate lines |
| `strings` | Extract readable text from binaries |
| `base64` | Encode/decode Base64 data |

---

## Tips for Future Levels

1. **Always read the level description carefully** - It tells you exactly what you need to find
2. **Use `man <command>`** - View detailed manual pages for any command
3. **Combine commands with pipes (`|`)** - Chain commands for powerful text processing
4. **Redirect errors with `2>/dev/null`** - Clean up output by hiding error messages
5. **Tab completion is your friend** - Use Tab to autocomplete file and directory names

---

## Resources

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux Command Reference](https://linux.die.net/man/)
- [Explain Shell](https://explainshell.com/) - Break down complex commands

---

**Progress:** Levels 0-11 Complete ✓

**Next Challenge:** Continue to Level 12 and beyond!