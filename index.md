## Redteamer's cheatsheet

### Enumeration 

#### Nmap - initial one, a full one and an UDP one, all of them in parallel
nmap -sV -O --top-ports 50 --open -oA nmap/initial <ip or cidr>

nmap -sC -sV -O --open -p- -oA nmap/full <ip or cidr>

nmap -sU -p- -oA nmap/udp <ip or cidr>

##### To specifiy the script & port
nmap --scripts vuln,safe,discovery -p 443,80 <ip or cidr>
##### Servers that could be not ping ?then add the flag -Pn
nmap -Pn --top-ports 50 --open -oA nmap/initial <ip or cidr>
