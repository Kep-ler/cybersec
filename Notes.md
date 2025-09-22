This are for notes and resources

Added Security+ notes for Prof Messer 4.0–4.3  
- Covered vulnerability management, asset management, and security monitoring  
- Included lab ideas, quiz questions, and reflection notes

Symmetric Encryption (Secret Key)
	•	One key for both encryption & decryption.
	•	Fast, good for bulk data (e.g., AES).
	•	Problem: how do you securely share the key?
	•	Asymmetric Encryption (Public/Private Key)
	•	Public key = share with everyone.
	•	Private key = keep secret.
	•	Slower, but solves key distribution. Used in TLS handshake.
	•	Hashing
	•	One-way function → can’t reverse.
	•	Ensures integrity (e.g., SHA-256).
	•	Real-world: checking file downloads against a hash.
	•	Digital Signatures
	•	Combine hashing + asymmetric encryption.
	•	Prove integrity + authenticity (sender verification).
	•	TLS/SSL
	•	Encrypts traffic between client & server.
	•	Uses asymmetric encryption for key exchange, then symmetric encryption for session.
	•	Real-world: the little padlock in your browse


Finished Prof Messer 4.5 through 5.0, closing out Domain 4: Implementation.

What I Learned
	•	How enterprises enhance security capabilities (endpoint protection, segmentation, NAC, continuous monitoring).
	•	Vulnerability scanning in practice:
	•	Found Avahi CVE (2011-1002) with Nmap.
	•	Tested with hping3/netcat → confirmed patched on Ubuntu.
	•	Installed and scanned Apache with Nikto:
	•	Found missing headers (X-Frame-Options, X-Content-Type-Options).
	•	Fixed configs and confirmed remediation.
	•	Key lesson: scans ≠ proof → must validate findings, confirm patches, and apply hardening.

Reflection

This domain gave me real hands-on understanding of how misconfigurations and outdated services impact enterprise security. I also saw how false positives show up in vuln scans and why patching + verification is critical.

Next Steps
	•	Start Domain 5: Operations and Incident Response.
	•	Labs: SSH hardening, log analysis, SIEM basics, and controlled incident response exercises.
What I Learned
	•	How to walk through the incident response lifecycle: preparation → identification → containment → eradication → recovery → lessons learned.
	•	Basics of SIEM/log analysis (journalctl, auth logs, packet captures in Wireshark).
	•	Forensics concepts: chain of custody, evidence handling, and why documentation matters.
	•	Practical containment/mitigation steps like disabling accounts, isolating machines, and validating recovery.

Labs & Practice
	•	Captured and filtered traffic with Wireshark on Kali.
	•	Reviewed system logs on Ubuntu for authentication events.
	•	Simulated a small incident response case around the Avahi DoS vulnerability → detected, responded, and documented actions.

Reflection

Domain 5 tied everything together: the tools are important, but the process is what ensures security teams handle incidents consistently. It was valuable to treat my lab events like “real incidents” and practice documenting the response.

I have officially finished the full Prof Messer Security+ (SY0-701) video course.

What This Means
	•	Covered all 6 domains from start to finish:
	•	Domain 1: General Security Concepts
	•	Domain 2: Threats, Vulnerabilities, and Mitigations
	•	Domain 3: Security Architecture
	•	Domain 4: Implementation
	•	Domain 5: Operations and Incident Response
	•	Domain 6: Governance, Risk, and Compliance
	•	Built a strong foundation that ties into my hands-on labs and book studies.

Key Reflections
	•	Prof Messer’s series gave me clarity on exam topics and how they connect.
	•	Reinforced my practical work: Nmap scans, Avahi CVE test, Apache hardening with Nikto, SSH/log analysis, Wireshark packet captures, and incident response simulation.
	•	Watching the full course kept me consistent and on track.

	neworking
	
Routers
Routers connect multiple networks together. They also connect computers on those networks to the Internet. Routers enable all networked computers to share a single Internet connection, which saves money. 

A router acts a dispatcher. It analyzes data being sent across a network, chooses the best route for data to travel, and sends it on its way. 

Routers connect your business to the world, protect information from security threats, and can even decide which computers receive priority over others. 

Beyond those basic networking functions, routers come with additional features to make networking easier or more secure. Depending on your security needs, for example, you can choose a router with a firewall, a virtual private network (VPN), or an Internet Protocol (IP) communications system.
