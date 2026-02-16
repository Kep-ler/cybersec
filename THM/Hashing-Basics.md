A hash value is a fixed-size string or characters that is computed by a hash function. A hash function takes an input of an arbitrary size and returns an output of fixed length, i.e., a hash value.

Learning Objectives

Hash functions and collisions
The role of hashing in authentication systems
Recognizing stored hash values
Cracking hash values
The use of hashing for integrity protection

Task 3 — Insecure Password Storage For Authentication
Question 1: What is the 20th password in rockyou.txt?
sed -n ‘20p’ rockyou.txt
answer: qwerty


Hashcat uses the following basic syntax: hashcat -m <hash_type> -a <attack_mode> hashfile wordlist, where:

-m <hash_type> specifies the hash-type in numeric format. For example, -m 1000 is for NTLM. Check the official documentation (man hashcat) and example page to find the hash type code to use.
-a <attack_mode> specifies the attack-mode. For example, -a 0 is for straight, i.e., trying one password from the wordlist after the other.
hashfile is the file containing the hash you want to crack.
wordlist is the security word list you want to use in your attack.

<img width="1680" height="1050" alt="hashcat1" src="https://github.com/user-attachments/assets/a81feddd-eba8-48c8-925e-220291d65be7" />

This room covered hashing functions and their uses from various perspectives. Before moving on, we should distinguish between hashing, encoding, and encryption.

Hashing, as already stated, is a process that takes input data and produces a hash value, a fixed-size string of characters, also referred to as digest. This hash value uniquely represents the data, and any change in the data, no matter how small, should lead to a change in the hash value. Hashing should not be confused with encryption or encoding; hashing is one-way, and you can’t reverse the process to get the original data.

Encoding converts data from one form to another to make it compatible with a specific system. ASCII, UTF-8, UTF-16, UTF-32, ISO-8859-1, and Windows-1252 are valid encoding methods for the English language. Note that UTF-8, UTF-16, and UTF-32 are Unicode encodings, and they can represent characters from other languages, such as Arabic and Japanese.

Another type of encoding commonly used when sending or saving data is not for any specific language. Examples include Base32 and Base64 encoding. Consider the following example of using base64 to encode and decode.
