# Day 2 Cybersecurity Learning Journey
**Date:** February 9th, 2026  
**Duration:** 1 Day

---

## [__OverTheWire Bandit - Levels 12-15__](__https://overthewire.org/wargames/bandit/__)

### 1. __ROT13 Cipher Decoding__
- Understanding ROT13 substitution cipher
- Character translation with `tr` command
- Reversible encryption (applying twice returns original)

### 2. __Hexdump Reversal and Multiple Compressions__
- Converting hexdump back to binary data
- Iterative file type identification
- Sequential decompression (gzip, bzip2, tar)
- Working with temporary directories

### 3. __SSH Key Authentication__
- Private/public key pair concepts
- File ownership and permissions (chmod 600)
- Key-based authentication vs password authentication
- Using SCP for secure file transfer
- Nested SSH sessions

### 4. __Network Communication__
- Client-server communication model
- Using netcat for TCP connections
- Port-based service interaction
- Piping data to network services

---

## Core Commands

### File Operations
```bash
find . -name "filename"           # Locate files
cp source destination             # Copy files
mktemp -d                         # Create secure temp directory
chmod 600 file                    # Set file permissions
```

### Data Processing
```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m'       # ROT13 decode
xxd -r hexfile > binary           # Reverse hexdump
file data.bin                     # Identify file type
```

### Compression Tools
```bash
gzip -d file.gz                   # Decompress gzip
bzip2 -d file.bz2                 # Decompress bzip2
tar -xf file.tar                  # Extract tar archive
```

### Network & Authentication
```bash
ssh -i keyfile user@host          # SSH with key
scp -P port user@host:file ./     # Secure copy
chmod 600 keyfile                 # Set strict permissions for SSH key
nc localhost 30000                # Network connection
echo "data" | nc host port        # Pipe to network service
```

---

## Key Concepts Learned

### File Permissions & Ownership
- Owner vs group vs others permissions
- Octal notation (600 = rw-------)
- Copying files changes ownership
- SSH requires strict key permissions

### Network Fundamentals
- localhost = 127.0.0.1 (same machine)
- Ports as communication endpoints
- TCP connections with netcat
- Piping for data transfer

### Security Practices
- Key-based authentication advantages
- Temporary directories for scratch work
- File type verification before operations
- Verbose mode for debugging (`-v`, `-vvv`)

### Data Transformation
- Hexdump representation and reversal
- Compression/decompression cycles
- Character substitution ciphers
- Binary vs text data handling

---

## Progress

✅ Bandit Level 12 - ROT13 Decoding  
✅ Bandit Level 13 - Hexdump & Multiple Compressions  
✅ Bandit Level 14 - SSH Key Authentication  
✅ Bandit Level 15 - Network Communication with Netcat