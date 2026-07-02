\# Lab 4: Using Wireshark to Examine Ethernet Frames



\## 1. Laboratory Overview \& Objectives



The objective of this laboratory exercise is to analyze Layer 2 Ethernet II frames using the Wireshark packet analyzer framework. 

By isolating local network traffic captures, this module explores the specific structural fields of an Ethernet frame—including 

physical MAC addressing and protocol type flags—while observing the operational behavior of the Address Resolution Protocol (ARP).



\* \*\*Host System:\*\* Windows 11 

\* \*\*Guest System:\*\* Arch Linux (CyberOps Workstation)

\* \*\*Primary Tools:\*\* Wireshark Packet Analyzer, Terminal Engine



\---



\## 2. Step-by-Step Task Execution \& Evidence



\### Part 1: Initializing the Capture Interface \& Filtering ARP Broadcasts



1\. Launched Wireshark within the graphical user desktop of the CyberOps Workstation.

2\. Initiated a live traffic capture on the active interface loop (`enp0s3`).

3\. Executed localized ping sweeps to trigger address resolution queries, then applied a strict display filter to isolate ARP communication sequences.



> \*\*📸 Verification Screenshot 1: Isolated ARP Traffic Stream\*\*

> !\[Wireshark ARP Capture](./screenshots/01\_wireshark\_arp\_capture.png)



\### Part 2: Dissecting the Structure of an Ethernet II Frame



1\. Selected an active network packet from the capture list panel to inspect its protocol stack.

2\. Expanded the Layer 2 header metadata under the "Ethernet II" details window to inspect physical hardware addresses.



> \*\*📸 Verification Screenshot 2: Ethernet II Header Field Expansion\*\*

> !\[Ethernet II Frame Header Details](./screenshots/02\_ethernet\_frame\_details.png)



\---



\## 3. Lab Questions \& Technical Analysis



\*\*Question 1: What is the purpose of the 'Type' field inside an Ethernet II frame header, and what are two common hex values found there?\*\*



\* \*\*Answer:\*\* The Type field (2 bytes) specifies which upper-layer protocol is encapsulated inside the frame's payload data area, telling the receiving network interface card exactly how to parse the incoming frame. Two highly common hex values are `0x0800` (which designates an IPv4 packet) and `0x0806` (which designates an Address Resolution Protocol frame).



\*\*Question 2: What unique destination MAC address is used during an initial ARP request when a device does not yet know the destination's physical hardware address?\*\*



\* \*\*Answer:\*\* An initial ARP request utilizes a Layer 2 broadcast address consisting entirely of binary 1s, which is represented in hexadecimal as `ff:ff:ff:ff:ff:ff`. This ensures that every network node connected to the local switch receives and evaluates the query to see if it owns the target IP address.



\---



\## 4. Laboratory Reflection



This laboratory offered excellent clarity on how Layer 2 addressing functions beneath the IP layer. Seeing a broadcast ARP request query (`ff:ff:ff:ff:ff:ff`) transform dynamically into a point-to-point unicast reply showed exactly how devices construct local hardware tables. Verifying the explicit Preamble, MAC mapping structures, and Hexadecimal Type indicators bridged theoretical knowledge directly with live packet inspection.





