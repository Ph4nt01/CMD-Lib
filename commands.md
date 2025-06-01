# Commands 


### File ops
```
file                                               #Shows file type.
find -size 1033c -name example.md                  #Find files named example.md with size of 1033 bytes.    
sort file.txt | uniq                               #Output the unique lines from a input or file.
xxd -r                                             #Revert a hexdump text into binary





```

### Network
```
scp -P 2220 user@host:file.txt /home               #Download a file from a host to home dir.
openssl s_client -connect host:port                #Connect to a SSL/TLS connection
ncat --ssl host port                               #Connect to a SSL/TLS connection
nmap -sV host                                      #Scan a host for open ports and services they provide
diff                                               #compare files or directories

```

### Forensics
```
```
