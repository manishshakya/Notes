#security
#pen-testing

# Steps for Penetration Testing

- Target Scoping
- Information Gathering
- Target Discovery
- Enumerating Target
- Vulnerability Mapping
- Social Engineering
- Target Exploitation
- Privilege Escalation
- Maintaining Access
- Documentation and Reporting

## Target Scoping

Target Scoping is defined as an empirical process to gather target assessment requirements and characterize each of its parameters in order to generate a test plan, its limitations, business objectives, and time schedule.

## Information Gathering

### querying the domain registration information

whois - client for the whois directory service
`dmitry -iwnse targethost` - Deepmagic Information Gathering Tool

### DNS record types

SOA - This is the start of authority record.
NS - This is the name server record.
A - This is the IPv4 address record.
MX - This is the mail exchange record.
PTR - This is the pointer record.
AAAA - This is the IPv6 address record.
CNAME - This is the abbreviation for canonical name. It is used as an alias name for another canonical domain name.

### DNS Information

host - DNS lookup utility
dig - DNS lookup utility. It is flexibility and clarity of output.
dnsenum -- multithread script to enumerate information on a domain and to discover non-contiguous IP blocks
fierce - DNS scanner that helps locate non-contiguous IP space and hostnames against specified domains.

### Getting network routing information

traceroute - print the route packets trace to network host
tcptraceroute - print the route packets trace to network host
The traceroute command sends a UDP or ICMP echo request packet with a Time To Live (TTL) of one and increments the TTL until the packet reaches the target, while the tcptraceroute tool uses TCP SYN to send out the packet to the target.

tctrace - It works by sending a TCP SYN packet to the target.
theharvester - The theharvester tool is an e-mail accounts, username, and hostname/subdomains gathering tool. It collects information from various public sources.
Metagoofil - It is a tool that utilizes the GoogleV search engine to get metadata from the documents available in the target domain.

## Target Discovery

### Identifying the target machine

ping - famous tool that is used to check whether a particualar host is available.
fping - send ICMP ECHO_REQUEST packets to network hosts
hping3 - send (almost) arbitrary TCP/IP packets to network hosts
nping - Network packet generation tool / ping utility
nbtscan - scan networks for NetBIOS name information

### Os fingerprinting

`nmap -O host` - to check what os the device is using

## Enumerating Target

- Emumerationg target is a process that is used to find and collect information about ports, operationg systems, and services avaiable on the target machines.

### Understating the TCP/IP protocol

IP provides addressing, datagram routing, and other functions for connecting one machine to another, while TCP is responsible for managing connections and provides reliable data transport between processes on two machines.

TCP is a connection-oriented protocol.
Before TCP can be used for sending data, the client and the server that want to communicate must establish a TCP connection using a three-way handshake mechanism as follows:

1. The client initiates the connection by sending a packet containing a SYN (synchronize) flag to the server. The client also sends the initial sequence number (ISN) in the Sequence number field of the SYN segment. This ISN is chosen randomly.
2. The server replies with its own SYN segment containing its ISN. The server acknowledges the client's SYN by sending an ACK (acknowledgment) flag containing the client's ISN + 1 value.
3. The client acknowledges the server by sending an ACK flag containing the server ISN + 1. At this point, the client and the server can exchange data

To terminiate the connection, TCP must follow the given mechanism:

1. The client sends a packet containing a FIN (finish) flag set.
2. The server sends an ACK (acknowledgment) packet to inform the client that the server has received the FIN packet.
3. After the application server is ready to close, the server sends a FIN packet.
4. The client then sends the ACK packet to acknowledge receiving the server's FIN packet. In a normal case, each side (client or server) can terminate its end of communication independently by sending the FIN packet.

Ports:

- Well-known port numbers (0 to 1023): Port numbers in this range are reserved port numbers and are usually used by the server processes that are run by a system administrator or privileged user. The examples of the port numbers used by an application server are SSH (port 22), HTTP (port 80), HTTPS (port 443), and so on.
- Registered port numbers (1024 to 49151): Users can send a request to the Internet Assigned Number Authority (IANA) to reserve one of these port numbers for their client-server application.
- Private or dynamic port numbers (49152 to 65535): Anyone can use port numbers in this range without registering themselves to IANA.

TCP Header Format
0 1 2 3  
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Source Port | Destination Port |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Sequence Number |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Acknowledgment Number |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Data | |U|A|P|R|S|F| |
| Offset| Reserved |R|C|S|S|Y|I| Window |
| | |G|K|H|T|N|N| |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Checksum | Urgent Pointer |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Options | Padding |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| data |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

SYN: This flag synchronizes the sequence numbers. This bit is used during session establishment.
ACK: This flag indicates that the Acknowledgment field in the TCP header is significant. If a packet contains this flag, it means that it is an acknowledgement to the previously received packet.
RST: This flag resets the connection.
FIN: This flag indicates that the party has no more data to send. It is used to tear down a connection gracefully.
PSH: This flag indicates that the buffered data should be pushed immediately to the application rather than waiting for more data.
URG: This flag indicates that the Urgent Pointer field in the TCP header is significant. The urgent pointer refers to important data sequence numbers.

0 7 8 15 16 23 24 31  
+--------+--------+--------+--------+
| Source | Destination |
| Port | Port |
+--------+--------+--------+--------+
| | |
| Length | Checksum |
+--------+--------+--------+--------+
|  
| data octets ...  
+---------------- ...

Port detection:
nmap host

Service version detection:
nmap -sV 192.168.200.2 -p 22

Operating system detection:
nmap -O 192.168.200.2

If you use the -A option, it will enable the following probe:

- Service version detection (-sV)
- Operating system detection (-O)
- Script scanning (-sC)
- Traceroute (--traceroute)

nmap -A 192.168.200.2
amap - Amap is a tool that can be used to check the application running on a specific port. Amap works by sending a trigger packet to the port and comparing the response with its database

## Vulnerability Mapping

Vulnerability mapping is the process of identifying and analyzing the critical security flaws in a target environment.

### Types of Vulnerability

- Design vulnerabilities: These are discovered due to the weaknesses found in the software specifications
- Implementation vulnerabilities: These are the technical security glitches found in the code of a system
- Operational vulnerabilities: These are the vulnerabilities that may arise due to the improper configuration and deployment of a system in a specific environment

Tools
Cisco Auditing Tool (CAT) is a mini security auditing tool. It scans the Cisco routers for common vulnerabilities such as default passwords, SNMP community strings, and some old IOS bugs

Web server vulnerabilities
Burp Suite (burpsuite) is a combination of powerful web application security tools.
Nikto2 is a basic web server security scanner.
Paros proxy is a valuable and intensive vulnerability assessment tool.
W3AF is a feature-rich web application attack and audit framework that aims to detect and exploit the web vulnerabilities.
WafW00f is a very useful python script, capable of detecting the web application firewall (WAF).
WebScarab is a powerful web application security assessment tool.

Password Cracking
