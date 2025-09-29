## 🔧 File Operations

```bash
# Identify file type/magic bytes
file target_file

# Locate a file by name
find / -name "config.json"

# Locate files >1000 bytes with specific size/name (c=bytes, +value for minimum)
find /path -size +1000c -name "*.log"

# Remove duplicate lines from sorted text
sort file.txt | uniq

# Generate hexdump of binary data
xxd binary_shellcode

# Convert hexdump back to executable binary
xxd -r hexdump.txt | xxd -r

# Compare differences between files
diff file1 file2

# Recursive directory comparison
diff -r dir1/ dir2/

# Display number of occurrences of each line, sorted by the most frequent
cat example.txt | uniq -c | sort -nr
```

---

## 🐋 Docker Operations

### Kali Linux Quick Start in Docker

#### Image Transfer Between Hosts
```bash
# On source host:
docker pull kalilinux/kali:latest
docker save kalilinux/kali:latest > kali_image.tar
scp -P <Port> kali_image.tar remote_user@remote_ip:/tmp/

# On destination host:
docker load < kali_image.tar

# Verify import success
docker images | grep kali
```

#### Container Life-cycle Management
```bash
# Start interactive container
docker run -d --name kali_shell kalilinux/kali:latest tail -f /dev/null

# Access shell
docker exec -it kali_shell /bin/bash

# Stop container
docker stop kali_shell

# Restart container
docker start kali_shell && docker exec -it kali_shell /bin/bash
```

---

## 🐱 Git Essentials

```bash
# Track all changes
git add .

# Commit with message
git commit -m "Initial commit"

# Push to remote (ensure remote set first)
git push

# Configuring identity
git config --global user.name "jasem"
git config --global user.email "jasem@example.com"
```

### Making a directory into a new repo from terminal
```sh
cd RepoFolder
git init
gh repo create RepoName --public --source=. --remote=origin
git add .
git commit -m "hello"
git branch -M main
git push -u origin main
```

### Git sub-modules
```bash
# How to add a repo as a submodule
cd YourMainRepo
git submodule add https://github.com/Username/YourRepo.git YourRepo
git commit -m "Adding YourRepo as a submodule in YourMainRepo"
git push origin main

# edit .gitmodules so the submodule knows to track the main branch
git config -f .gitmodules submodule.YourRepo.branch main
git add .gitmodules
git commit -m "Set the submodule to track main branch"
git push origin main

# How to upadte the submodule later
git submodule update --remote yoursubmodule
git add yoursubmodule
git commit -m "Bump submodule to latest main"
git push
```


---
## 🐍 Python

### Using virtual environments in python
```bash
# Creating the virtual enviroment
python3 -m venv ~/.venv/tmdev

# Activating the venv
source ~/.venv/tmdev/bin/activate

# Deactivating after usage
deactivate
```

### Argparse example usage
```python
import argparse

def main(a):

    print(f"Hey {a.name}, have a good day.")

if __name__ == "__main__":

    p = argparse.ArgumentParser(description="This guy tells you to have a good day.")
    p.add_argument("-n", "--name", metavar="", default="root", help="Enter your name")
    a = p.parse_args()
    main(a)
```
---

## 🌐 Network Operations

```bash
# Copy over non-standard port
scp -P 2220 user@host:/remote/path/file.txt ./local/

# TLS certificate analysis
openssl s_client -connect host:port

# Encrypted TCP connections
ncat --ssl host port

# Target specific ports for scanning
nmap -sV -p 22,80,443 host
```

### Tcpdump
```sh
# for capturing traffic of a whole subnet
tcpdump -i <interface> -v net 192.168.1.0/24

# capturing the incomign traffic of specific http server
tcpdump -i <intf> -v `src port <portnum> and src <ipadd>`

# capturing and saving the traffic on a specific port
tcpdump -i <intf> -v -n -w sharkfiles/test.pcap port <portnum>

```

---

## ⏳ Process Management

```bash
# Viewer all cron processes
ps aux | grep -i cron

# List root's scheduled tasks (if permissions allow)
crontab -u root -l

# Force process termination (SIGKILL)
kill -9 <PID>

# Monitor threads for a specific process
top -H -p <PID>
```

---

## 🚀 Privilege Escalation

```bash
# Enumerate available sudo permissions
sudo -l

# Locate SUID files (setuid)
find / -type f -perm -4000 2>/dev/null

# Locate SGID files (setgid)
find / -type f -perm -2000 2>/dev/null
```

---

## 🕵️ Web Pen-Testing

### FFUF Tool
#### Fuzzing Username
```bash
# Verbose output (-v) reveals response code differences (e.g., Redirect Location)
ffuf -X POST \
-d "uid=FUZZ&pass=admin&btnSubmit=Login" \
-H "Content-Type: application/x-www-form-urlencoded" \
-u http://target/login.php \
-w usernames.txt -v

# Echoing usernames and redirect locations to a file
ffuf -X POST \
-d "uid=FUZZ&passw=admin&btnSubmit=Login" \
-H "Content-Type: application/x-www-form-urlencoded" \
-u https://demo.testfire.net/doLogin \
-w admin.txt \
-mc 302 \
-v 2>&1 | awk '
/\| --> \|/ {redir=$NF}
/\* FUZZ:/ {user=$NF; print user, redir}
' | sort > test.txt

# Printing unique redirect locations and usernames, by finding out the most frequent redirect path and filtering it out
ffuf -X POST \
-d "uid=FUZZ&passw=admin&btnSubmit=Login" \
-H "Content-Type: application/x-www-form-urlencoded" \
-u https://demo.testfire.net/doLogin \
-w admin.txt \
-mc 302 \
-v 2>&1 | awk '
/\| --> \|/ {redir=$NF; redirects[redir]++; last_redirect=redir}
/\* FUZZ:/ {
    raw=$NF
    sub(/&.*/, "", raw)  # Remove everything after &
    combo[last_redirect] = raw
}
END {
    # Find the most common redirect
    max = 0
    for (r in redirects) {
        if (redirects[r] > max) {
            max = redirects[r]
            default_redirect = r
        }
    }
    # Output only those that are not the default
    for (r in combo) {
        if (r != default_redirect) {
            print r, combo[r]
        }
    }
}'
```

### SQL Injection & HTTP Testing
```bash
# Basic injection detection
sqlmap -u "http://target/page.php?id=1" --batch

# HEAD request for headers
curl -I http://target/admin.php

# Custom header testing
curl -X POST -H 'X-Forwarded-For: 127.0.0.1' target/page.php
```

---

## 🔍 Forensics

### Binary Analysis
```bash
# Find minimum-length strings
strings -n 10 binary.exe | grep "http"

# Extract file metadata
exiftool photo.jpg

# Binary artifact extraction
binwalk -e firmware.bin

# Carve files with major headers from file.bin faster
foremost -i file.bin -o ./extracted/
```
