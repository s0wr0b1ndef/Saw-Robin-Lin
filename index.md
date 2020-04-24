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

##### Without Nmap 
###### nc + bash
If you get in a machine that doesn’t has nmap installed, you can do a basic discovery of (for example), top 20 ports open in 192.168.30 by doing:

top10=(20 21 22 23 25 80 110 139 443 445 3389); for i in "${top10[@]}"; do nc -w 1 192.168.30.253 $i && echo "Port $i is open" || echo "Port $i is closed or filtered"; done
