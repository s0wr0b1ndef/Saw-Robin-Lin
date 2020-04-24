## Redteamer's cheatsheet

### Enumeration 

#### Nmap - initial one, a full one and an UDP one, all of them in parallel
##### nmap -sV -O --top-ports 50 --open -oA nmap/initial <ip or cidr> 
##### nmap -sC -sV -O --open -p- -oA nmap/full <ip or cidr> 
##### nmap -sU -p- -oA nmap/udp <ip or cidr> 
 
