# Lab 08: Using Wireshark to Examine HTTP and HTTPS Traffic

## 1. Executive Summary & Lab Information

This laboratory module focuses on analyzing the structural and security differences between unencrypted Hypertext Transfer Protocol (HTTP) traffic and encrypted Hypertext Transfer Protocol Secure (HTTPS) traffic. Using command-line utilities (`tcpdump`) and graphical packet analyzers (Wireshark), network traffic is inspected to demonstrate how HTTP exposes sensitive credentials in plaintext while HTTPS enforces encryption via SSL/TLS.


* **Host Operating System:** Windows 11
* **Guest Virtual Machine:** Arch Linux (CyberOps Workstation VM)
* **Target Network Interface:** `enp0s3`
* **Analysis Tools Used:** `tcpdump`, Wireshark, Mozilla Firefox Browser

---

## 2. Environment Verification & Preliminary Commands

Before initiating packet captures, the workstation interfaces were identified and verified using the command line:

```bash
[analyst@secOps ~]$ ip address
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:55:44:07 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 metric 100 brd 10.0.2.255 scope global dynamic enp0s3
       valid_lft 82091sec preferred_lft 82091sec
    inet6 fd17:625c:f037:2:a00:27ff:fe55:4407/64 scope global dynamic mngtmpaddr noprefixroute 
       valid_lft 86399sec preferred_lft 14399sec
    inet6 fe80::a00:27ff:fe55:4407/64 scope link 
       valid_lft forever preferred_lft forever


Active Network Interfaces Summary
Interface ID	Interface Type	IPv4 Address / Subnet	IPv6 Address	Interface State	MAC Address
lo	Virtual Loopback	127.0.0.1/8	::1/128	UNKNOWN	00:00:00:00:00:00
enp0s3	Ethernet Connection	10.0.2.15/24	fd17:625c:f037:2:a00:27ff:fe55:4407/64	UP	08:00:27:55:44:07

3. Step-by-Step Task Execution
Part 1: Capturing and Inspecting Unencrypted HTTP Traffic
Opened the terminal on the CyberOps Workstation.

Executed tcpdump to write captured packets to httpdump.pcap:

Bash


sudo tcpdump -i enp0s3 -s 0 -w httpdump.pcap
Opened Mozilla Firefox and accessed the unencrypted portal: http://www.altoromutual.com/login.jsp.

Entered authentication parameters (Admin / Admin) into the login prompt and submitted the form.

Stopped tcpdump using CTRL+C and launched the capture file in Wireshark:

Bash


wireshark httpdump.pcap &
Applied display filter http and isolated the authentication frame (POST /doLogin).

Captured HTTP Parameter Breakdown
Protocol Parameter	Value Captured in Payload	Security Implications
Request Method	POST	Transmits payload data inside frame body.
Header Type	application/x-www-form-urlencoded	Transmits form keys and values without layer encryption.
Username (uid)	Admin	Fully visible in cleartext to network eavesdroppers.
Password (passw)	Admin	Fully visible in cleartext to network eavesdroppers.

📸 Verification Asset 1: ./screenshots/01_http_login_capture.png

Part 2: Capturing and Inspecting Encrypted HTTPS Traffic
Re-initiated command-line packet capture targeting HTTPS sessions:

Bash


sudo tcpdump -i enp0s3 -s 0 -w httpsdump.pcap
Opened Firefox and navigated to the encrypted web service: https://www.netacad.com.

Clicked the login portal link and submitted test entries into the secure input fields.

Terminated tcpdump and loaded httpsdump.pcap in Wireshark.

Applied the port display filter to isolate SSL/TLS records:

Plaintext


tcp.port == 443
Captured HTTPS Parameter Breakdown
Protocol Parameter	Value Captured in Payload	Security Implications
Transport Protocol	TCP (Port 443)	Ensures reliable transport for cryptographic exchanges.
Security Layer	Transport Layer Security (TLSv1.2 / TLSv1.3)	Encrypts application payloads end-to-end.
Content Type	Application Data (23)	Transmits obfuscated HTTP headers and form payloads.
Payload Format	Ciphertext Hex Stream	Protects sensitive credentials from unauthorized extraction.

📸 Verification Asset 2: ./screenshots/02_https_encrypted_data.png

4. Technical Analysis & Lab Questions
Part 1, Step 2b: List the interfaces and their IP addresses displayed in the ip address output.

lo (Loopback interface): 127.0.0.1/8

enp0s3 (Ethernet interface): 10.0.2.15/24

Part 1, Step 3d: Expand the HTML Form URL Encoded section. What two pieces of information are displayed?

Answer: Expanding the HTML Form URL Encoded application tree displays the cleartext form submission parameters:

uid: Admin

passw: Admin

Part 2, Step 1b: What do you notice about the website URL?

Answer: The address line begins with the https:// protocol scheme and renders a closed padlock icon, verifying that traffic is encrypted over an SSL/TLS session.

Part 2, Step 2d: What has replaced the HTTP section that was in the previous capture file?

Answer: The HTTP header structure is replaced by the Transport Layer Security (TLS) record layer, displaying encrypted Application Data payloads.

Part 2, Step 2f: Is the application data in a plaintext or readable format?

Answer: No. The entire payload is rendered as an unreadable hex string (ciphertext), preventing eavesdroppers from reading transmitted data without private encryption keys.

5. Analytical Reflections & Conclusions
Advantages of Deploying HTTPS over HTTP
Confidentiality: Encrypts session data to protect user credentials, financial transactions, and session tokens from packet sniffing.

Integrity: Uses cryptographic hashes to verify that transmitted frames are not altered or tampered with by middleboxes or malicious actors.

Authentication: Uses Public Key Infrastructure (PKI) digital certificates to confirm web server identities to client endpoints.

Security Considerations Regarding HTTPS Trustworthiness
While HTTPS guarantees channel encryption, it does not validate whether a site is safe or trustworthy. Threat actors frequently obtain valid SSL/TLS certificates for phishing sites, typosquatting domains, or malware hosting environments. Modern security operations center (SOC) analysts must inspect domain reputations, certificates, and payload behaviors rather than relying solely on encryption indicators.

