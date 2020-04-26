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
~~~
top10=(20 21 22 23 25 80 110 139 443 445 3389); for i in "${top10[@]}"; do nc -w 1 192.168.30.253 $i && echo "Port $i is open" || echo "Port $i is closed or filtered"; done 
~~~

/dev/tcp/ip/port or /dev/udp/ip/port
Alternatively, is possible to do the same than above but by using the special dev files /dev/tcp/ip/port or /dev/udp/ip/port (for example nc is not found):
~~~
top10=(20 21 22 23 25 80 110 139 443 445 3389); for i in "${top10[@]}"; do (echo > /dev/tcp/192.168.30.253/"$i") > /dev/null 2>&1 && echo "Port $i is open" || echo "Port $i is closed"; done
~~~
Taking these last examples, is straightforward to create a dummy script for scan a hole /24 net (for example):

```
#!/bin/bash
subnet="192.168.30"
top10=(20 21 22 23 25 80 110 139 443 445 3389)
for host in range {1..255}; do
    for port in "${top10[@]}"; do
        (echo > /dev/tcp/"${subnet}.${host}/${port}") > /dev/null 2>& && echo "Host ${subnet}.${host} has ${port} open" || "Host ${subnet}.${host} has ${port} closed"
    done
done
```
##### Banner grabbing (without nmap)
If nmap didn’t grab banners (or is not installed), you can do it with /dev/tcp/ip/port /dev/udp/ip/port or by using telnet.
~~~
/dev/tcp/ip/port or /dev/udp/ip/port
cat < /dev/tcp/192.168.30.253/22
SSH-2.0-OpenSSH_6.2p2 Debian-6
^C pressed here
~~~
For doing it with udp ports is the same, but changing tcp for udp

###### telnet
~~~
telnet 192.168.30.253 22
SSH-2.0-OpenSSH_6.2p2 Debian-6
^C pressed here
~~~
