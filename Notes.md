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
