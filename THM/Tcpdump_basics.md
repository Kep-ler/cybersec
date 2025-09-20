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
