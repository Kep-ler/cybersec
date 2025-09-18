Room Prerequisites

This room is the last in a group of four rooms about computer networking:

Networking Concepts
Networking Essentials
Networking Core Protocols
Networking Secure Protocols (this room)
We recommend finishing all the previous three rooms before starting this one.

Learning Objectives

Upon finishing this room, you will learn about:

SSL/TLS
How to secure existing plaintext protocols:
HTTP
SMTP
POP3
IMAP
How SSH replaced the plaintext TELNET
How VPN creates a secure network over an insecure one
The first approach is to use TLS, which provides a convenient way to secure many protocols, such as HTTP, SMTP, and POP3. Protocols secured with TLS usually get an S, for Secure, added to their names, such as HTTPS, SMTPS, and POP3S.

The second approach to secure network traffic is to use SSH. Although SSH is mainly used for remote access, it can also transfer files securely and establish secure tunnels. Creating an SSH tunnel is a solid choice if you want to pass the traffic of a plaintext protocol, such as VNC.

The last approach we covered to secure network traffic is using VPN connections. A VPN connection is usually the perfect option for connecting two company branches.

We will finish this room with a hands-on challenge.
