
## 🔧 File Operations
```bash
file target_file                  # Identify file type/magic bytes
find /path -size +1000c -name "*.log"  # Locate files >1000 bytes with specific size/name (c=bytes, +value for minimum)
sort file.txt | uniq              # Remove duplicate lines from sorted text
xxd binary_shellcode             # Generate hexdump of binary data
xxd -r hexdump.txt | xxd -r      # Convert hexdump back to executable binary
diff file1 file2                  # Compare differences between files
diff -r dir1/ dir2/               # Recursive directory comparison
```

---

## 🐋 Docker Operations

### Kali Linux Container Quick Start

#### Image Transfer Between Hosts
```bash
# On source host:
docker pull kalilinux/kali:latest
docker save kalilinux/kali:latest > kali_image.tar
scp -P <Port> kali_image.tar remote_user@remote_ip:/tmp/

# On destination host:
docker load < kali_image.tar
docker images | grep kali         # Verify import success
```

#### Container Lifecycle Management
```bash
# Start interactive container:
docker run -d --name kali_shell kalilinux/kali:latest tail -f /dev/null

# Access shell:
docker exec -it kali_shell /bin/bash

# Stop container:
docker stop kali_shell

# Restart container:
docker start kali_shell && docker exec -it kali_shell /bin/bash
```

---

## 🐱 Git Essentials
```bash
# Track all changes:
git add .

# Commit with message:
git commit -m "Initial commit"

# Push to remote (ensure remote set first):
git push
```

> **Before Pushing:** Configure identity with:  
> `git config --global user.name "Hacker"`  
> `git config --global user.email "hacker@example.com"`

---

## 🌐 Network Operations
```bash
scp -P 2220 user@host:/remote/path/file.txt ./local/  # Copy over non-standard port
openssl s_client -connect host:port                   # TLS certificate analysis
ncat --ssl host port                                  # Encrypted TCP connections
nmap -sV -p 22,80,443 host                            # Target specific ports for scanning
```

---

## ⏳ Process Management
```bash
ps aux | grep -i cron        # Viewer all cron processes
crontab -u root -l           # List root's scheduled tasks (if permissions allow)
kill -9 <PID>                # Force process termination (SIGKILL)
top -H -p <PID>              # Monitor threads for a specific process
```

---

## 🚀 Privilege Escalation
```bash
sudo -l                       # Enumerate available sudo permissions
find / -type f -perm -4000 2>/dev/null  # Locate SUID files (setuid)
find / -type f -perm -2000 2>/dev/null  # Locate SGID files (setgid)
```

> **LinPEAS Usage:**  
> `chmod +x linpeas.sh && ./linpeas.sh`  # Make executable and run  
> [LinPEAS GitHub](https://github.com/carlospolop/PEASS-ng) download source

---

## 🕵️ Web Testing

### FFUF Username Enumeration
```bash
# Verbose output (-v) reveals response code differences(EX: Redirect Location):
ffuf -X POST \
    -d "uid=FUZZ&pass=admin&btnSubmit=Login" \
    -H "Content-Type: application/x-www-form-urlencoded" \
    -u http://target/login.php \
    -w usernames.txt -v
```

### SQL Injection & HTTP Testing
```bash
sqlmap -u "http://target/page.php?id=1" --batch  # Basic injection detection
curl -I http://target/admin.php                   # HEAD request for headers
curl -X POST -H 'X-Forwarded-For: 127.0.0.1' target/page.php  # Custom header testing
```

---

## 🔍 Forensics

### Binary Analysis
```bash
strings -n 10 binary.exe | grep "http"  # Find minimum-length strings
exiftool photo.jpg                     # Extract file metadata
binwalk -e firmware.bin                # Binary artifact extraction
foremost -i file.bin -o ./extracted/   # Fast carving of header-identified files
```

> **Advanced Forensics:**  
> Combine with `grep -a`: `strings binary.exe | grep -a "password"`