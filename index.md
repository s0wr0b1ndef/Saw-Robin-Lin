## Redteamer's cheatsheet

### Enumeration 

#### Nmap - initial one, a full one and an UDP one, all of them in parallel

nmap -sV -O --top-ports 50 --open -oA nmap/initial <ip or cidr>
nmap -sC -sV -O --open -p- -oA nmap/full <ip or cidr>
nmap -sU -p- -oA nmap/udp <ip or cidr>
 
--top-ports only scan the N most common ports
--open only show open ports
-sC use the default scripts
-sV detect versions
-O detect Operating Systems
-p- scan all the ports
-oA save the output in normal format, grepable and xml
-sU scan UDP ports
