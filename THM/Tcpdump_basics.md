Capture packets and save them to a file
Set filters on captured packets
Control how captured packets are displayed


saving to a file using -w FILE.
read packets from a file by using -r FILE

Limit the Number of Captured Packets

You can specify the number of packets to capture by specifying the count using -c COUNT. Without specifying a count, the packet capture will continue till you interrupt it, for example, by pressing CTRL-C. Depending on your goal, you only need a limited number of packets.

Don’t Resolve IP Addresses and Port Numbers

Tcpdump will resolve IP addresses and print friendly domain names where possible. To avoid making such DNS lookups, you can use the -n argument. Similarly, if you don’t want port numbers to be resolved, such as 80 being resolved to http, you can use the -nn to stop both DNS and port number lookups. Consider the following example shown in the terminal below. We captured and displayed five packets without resolving the IP addresses.

Produce (More) Verbose Output

If you want to print more details about the packets, you can use -v to produce a slightly more verbose output. According to the Tcpdump manual page (man tcpdump), the addition of -v will print “the time to live, identification, total length and options in an IP packet” among other checks. The -vv will produce more verbose output; the -vvv will provide even more verbosity; check the manual page for details.

Command	Explanation
tcpdump -i INTERFACE	Captures packets on a specific network interface
tcpdump -w FILE	Writes captured packets to a file
tcpdump -r FILE	Reads captured packets from a file
tcpdump -c COUNT	Captures a specific number of packets
tcpdump -n	Don’t resolve IP addresses
tcpdump -nn	Don’t resolve IP addresses and don’t resolve protocol numbers
tcpdump -v	Verbose display; verbosity can be increased with -vv and -vvv

<img width="1680" height="1050" alt="tcpdump1" src="https://github.com/user-attachments/assets/15a4f506-f442-4928-853a-4c3c33d2cf20" />

What I Learned
	•	tcpdump is like a CLI version of Wireshark → great for quick captures and working on remote servers.
	•	I can filter by protocol, port, or host to focus on relevant traffic.
	•	Capturing to .pcap lets me analyze later in Wireshark with a GUI.
	•	Useful for incident response, troubleshooting, or validating suspicious traffic.

Reflection

Using tcpdump gave me more confidence in analyzing network traffic without relying on a GUI. It ties directly into Security+ Domain 5 (Operations & Incident Response) because packet captures are a critical part of monitoring and investigation.

STILL LEARNING TCP DUMP


tcpdump -r traffic.pcap icmp | wc -l 
www.flino.dev
www.flino.dev
Key points:
Command breakdown: tcpdump reads the file (-r traffic.pcap), filters for icmp traffic, and wc -l counts the number of lines (packets).
Context: This question is part of network traffic analysis, often used to identify ICMP tunnelling or mapping active hosts


Using pcap-filter, Tcpdump allows you to refer to the contents of any byte in the header using the following syntax proto[expr:size], where:

proto refers to the protocol. For example, arp, ether, icmp, ip, ip6, tcp, and udp refer to ARP, Ethernet, ICMP, IPv4, IPv6, TCP, and UDP respectively.
expr indicates the byte offset, where 0 refers to the first byte.
size indicates the number of bytes that interest us, which can be one, two, or four. It is optional and is one by default.
To better understand this, consider the following two examples from the pcap-filter manual page (and don’t worry if you find them difficult):

ether[0] & 1 != 0 takes the first byte in the Ethernet header and the decimal number 1 (i.e., 0000 0001 in binary) and applies the & (the And binary operation). It will return true if the result is not equal to the number 0 (i.e., 0000 0000). The purpose of this filter is to show packets sent to a multicast address. A multicast Ethernet address is a particular address that identifies a group of devices intended to receive the same data.
ip[0] & 0xf != 5 takes the first byte in the IP header and compares it with the hexadecimal number F (i.e., 0000 1111 in binary). It will return true if the result is not equal to the (decimal) number 5 (i.e., 0000 0101 in binary). The purpose of this filter is to catch all IP packets with options.
