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
