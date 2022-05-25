# Wireshark
### Filters 
Filter for speicifc IP addresses
```
ip.addr==172.16.108.174
ip.addr==172.16.108.0/24
```
Filter for source/destinatinon IP
```
ip.src==172.16.108.0/24
ip.dst==172.16.108.0/24
```
Filter by multiple IPs and Range
```
ip.addr == IP and ip.addr == IP
ip.addr >= 10.10.50.1 and ip.addr <= 10.10.50.100
```
Exclude IPs
```
!(ip.addr == IP)
```
Filter port number
```
tcp.port eq 25 or SERVICE
tcp.port == PORT
tcp.dstport == PORT
tcp.srcport == PORT
```
Ports/services:
- 21/20 = FTP (controlling and transferring data)
	- ftp-data or ftp
- 22 SSH/SCP/SFTP
- 23 TELNET
- 25 SMTP
- 53 DNS (UDP)
- 80 HTTP
- 443 HTTPS
- 110 POP3
- ICMP
- ARP

IF IT IS UDP THEN CHANGE TO udp.port...

Filter based on specific string
```
frame contains STRING
```
Filter to view only 3 way handshake
```
tcp.flags.syn==1 or (tcp.seq==1 and tcp.ack==1 and tcp.len==0 and tcp.analysis.initial_rtt)
```
filter base on the method
```
http.request.method== "POST"
http.request.method== "GET"
http.request.method== "PUT"
http.request.method== "DELETE"
```
Filter based on MAC
```
eth.addr == 00:70:f4:23:18:c4
```
Filter based on broadcast filter
```
eth.dst == ff:ff:ff:ff:ff:ff
```
Filter based on time stamp
```
frame.time >= “June 02, 2019 18:04:00”
```
Filter based on URL
```
http.host == "HOST NAME"
```
syn PACKETS
```
tcp.flags.syn == 1
```

### Exporting items
FILE -> EXPORT OBJECTS -> HTTP
### FOLLOW TCP STREAM
- allows u to read tcp connection into an easier to read format
- right click HTTP post packet -> follow -> TCP Stream

### Name resolution
EDIT -> PREFERENCES -> NAME RESOLUTION -> RESOLVE NETOWRK (IP) ADDRESSES

# Linux CMDs
#### Open ports 
```
netstat -tunap
netstat -tunap | grep PORT NUMBER
```
#### FTP
```
ftp IP

# list direcotry
ls

#download file
get

# exit
bye
exit
```
#### Telnet
```
telnet IP

# exit
exit
```
- unencrypted remote access
#### SSH
```
ssh USERNAME@IP
```
- encrypted remote access
#### SFTP
```
sftp USERNAME@IP

# list direcotry
ls

#download file
get

# exit
bye
exit
```
- encrypted 
#### SCP
```
scp USERNAME@IP:/DIRECTORY/file myfile1
```

#### DNS
```
dig URL
dig google.com
```
- Resolves IP addresses
- Get the CNAME or A record of the domain/url
```
dig IP @{DNS IP}
```
- dig using the 2nd IP as the DNS server
```
dig -x {IP}
```
- DNS reverse lookup
```
dig @{DNS IP} IP axfr
```
- DNS zone transfer 
```
dig -t RECORD IP
```
- Get a specific DNS record 
- CNAME, NS, AAAA,  PTR, NS, A, MX, SOA, TXT

#### HOST
```
host URL
host www.yahoo.com
host www/yahoo.com {DNS IP}
```
- Gets the alias and ip address of the yahoo domain 

####  nslookup 
```
nslookup
nslookup IP

#set the DNS server for nslookup
server IP
```
- nslookup is a network administration command-line tool for querying the Domain Name System to obtain the mapping between domain name and IP address, or other DNS records
```
nslookup 
server IP
set type={DNS RECORD}
IP

e.g., 
nslookup
server 192.168.1.1
set type=ptr
192.168.1.1
```
# Ping
#### Ping Sweep
```
hping3 -c 1 -S IP
```
#### Bypass firewall
```
hping3 –c 1 –S –p PORT IP
```
#### Ping sweep to a range of devices
```
fping -g x.x.x.1 x.x.x.10
```
- ping 10 addresses

# Discover lives devices 
```
netdicover -i eth0 -r IP/24
nmap IP/24
nmap -sn IP/24
fping -g x.x.x.1 x.x.x.10
```

# NMAP
#### Discover other live deivces
```
nmap IP/24
```
- 24 is the CIDR subnet (for 192.168.x.x)
- 16 (172.16.x.x)

#### basic NMAP
```
nmap IP
nmap -sS IP
```

#### Recommended NMAP + banner grabbing + port version
```
nmap -sV -sC IP
```

#### nmap all ports
```
nmap -p- -sV -sC IP
```

#### nmap specific ports 
```
nmap -sC -sV -p80 IP
nmap -sC -sV -p22,80 IP
```

####  scan on ports 21,23,25,53,80,110 between 192.168.10.97 to 102
```
nmap –sF –p 21,23,25,53,80,110 192.168.10.97-102
```
- reduce network traffic

#### ACK scan
```
nmap -sA IP
```
- used to find if there is firewall blocking
- FILTERED = block by wirewall 

#### UDP scan + port 21, 53, 80, 161
```
nmap -sU –p 21,53,80,161 IP
```
- scan UDP ports
- takes a long ass time

#### Other scans
```
# UDP scan to get UDP port version 
nmap -sV –sU –p21,53,80,161 192.168.10.100

# UDP scan with verbose 
sudo nmap -sU –vv 192.168.10.100

# scan top 10 ports 
nmap --top-ports 10
```

# Footprinting 
#### whois 
```
whois URL
```
- gives u information of the domain e.g., owner, IP addresss

### Websites
https://ipinfo.io
https://search.arin.net/rdap
#### tracert
```
# linux
traceroute IP

# windows
tracert IP 
```
www.traceroute.org


# Ping spoofer
#### send 2 icmp packets
```
sudo hping3 –c 2 –-icmp IP
```
#### ping spoofer
```
hping3 –c 2 –a {FAKE IP} –-icmp {TARGET IPs}
```
