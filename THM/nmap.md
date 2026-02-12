IP range using -: If you want to scan all the IP addresses from 192.168.0.1 to 192.168.0.10, you can write 192.168.0.1-10
IP subnet using /: If you want to scan a subnet, you can express it as 192.168.0.1/24, and this would be equivalent to 192.168.0.0-255
Hostname: You can also specify your target by hostname, for example, google.com

 Nmap offers a list scan with the option -sL. This scan only lists the targets to scan without actually scanning them.
  TCP ports 80 and 443, and DNS servers, which typically listen on UDP (and TCP) port 53.

By design, TCP has 65,535 ports, and the same applies to UDP
Nmap offers the option -sU to scan for UDP services. Because UDP is simpler than TCP, we expect the traffic to differ. The screenshot below shows several ICMP destination unreachable (port unreachable) responses as Nmap sends UDP packets to closed UDP ports.

Limiting the Target Ports

Nmap scans the most common 1,000 ports by default. However, this might not be what you are looking for. Therefore, Nmap offers you a few more options.

-F is for Fast mode, which scans the 100 most common ports (instead of the default 1000).
-p[range] allows you to specify a range of ports to scan. For example, -p10-1024 scans from port 10 to port 1024, while -p-25 will scan all the ports between 1 and 25. Note that -p- scans all the ports and is equivalent to -p1-65535 and is the best option if you want to be as thorough as possible.
Tip: The most common services use a port number between 1 and 1024 for either UDP or TCP. These ports are also known as well-known ports. Use -p1-1023 to scan for the well-known ports.

Summary

Option	Explanation
-sT	TCP connect scan – complete three-way handshake
-sS	TCP SYN – only first step of the three-way handshake
-sU	UDP scan
-F	Fast mode – scans the 100 most common ports
-p[range]	Specifies a range of port numbers – -p- scans all the ports


## Nmap Targeting and Port Scanning (Summary)

Nmap allows flexible target specification using IP ranges (`192.168.0.1-10`), CIDR notation (`192.168.0.1/24`), or hostnames (`google.com`). The `-sL` option performs a list scan, which displays targets without scanning ports.

Both TCP and UDP support 65,535 ports (1–65535). Common services include HTTP (TCP 80), HTTPS (TCP 443), and DNS (UDP/TCP 53).

By default, Nmap scans the top 1,000 most common ports. Port scanning can be customized using:
- `-F` to scan the top 100 ports (Fast mode)
- `-p10-1024` to scan a specific range
- `-p-25` to scan ports 1–25
- `-p-` to scan all ports (1–65535)
- `-p1-1023` to scan well-known ports

UDP services can be scanned using `-sU`. Closed UDP ports typically respond with ICMP “destination unreachable (port unreachable)” messages.

Forcing the Scan

When we run our port scan, such as using -sS, there is a possibility that the target host does not reply during the host discovery phase (e.g. a host doesn’t reply to ICMP requests). Consequently, Nmap will mark this host as down and won’t launch a port scan against it. We can ask Nmap to treat all hosts as online and port scan every host, including those that didn’t respond during the host discovery phase. This choice can be triggered by adding the -Pn option.

Summary

Option	Explanation
-O	OS detection
-sV	Service and version detection
-A	OS detection, version detection, and other additions
-Pn	Scan hosts that appear to be down


