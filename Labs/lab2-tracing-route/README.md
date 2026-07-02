\# Lab 2: Tracing a Route to a Remote Server



\## 1. Laboratory Overview \& Objectives



The objective of this laboratory is to utilize connection-testing tools (ICMP `ping`) and route-tracing utilities (`traceroute`) to diagnose network paths,

measure round-trip times, and analyze path infrastructure. This allows for an in-depth understanding of how data routing operates across local and public 

network boundaries.



\* \*\*Host System:\*\* Windows 11

\* \*\*Guest System:\*\* Arch Linux (CyberOps Workstation)

\* \*\*Primary Tools:\*\* `ping`, `traceroute`



\---



\## 2. Step-by-Step Task Execution \& Evidence



\### Part 1: Testing Connectivity with ICMP Ping



1\. Opened a terminal instance inside the CyberOps Workstation.



2\. Initiated a 4-packet echo request loop to a public DNS destination node (`8.8.8.8`) to confirm active latency baselines. 

The output verified a stable round-trip time baseline with 0% packet loss.



> \*\*📸 Verification Screenshot 1: Successful ICMP Echo Reply Loop\*\*

> !\[ICMP Ping Diagnostics](./screenshots/01\_ping\_diagnostic.png)



\### Part 2: Dissecting the Network Path with Traceroute



1\. Executed the `traceroute` utility targeting an external web resource (`google.com`).

2\. Captured the hop traversal output to observe intermediate gateway tracking.



> \*\*📸 Verification Screenshot 2: Route Trace Hops\*\*

> !\[Traceroute Hop Sequence](./screenshots/02\_traceroute\_hops.png)



\---



\## 3. Lab Questions \& Technical Analysis



\*\*Question 1: What happens when a router along the traceroute path does not respond to the probe requests? How is this represented in the terminal output?\*\*



\*\*\*Answer:\*\* When a router or gateway fails to respond to probe packets, the terminal prints asterisks (`\* \* \*`) for that specific hop row. This indicates an ICMP/UDP timeout condition, showing that the device at that stage of the path did not reply within the expected window frequently because security configurations or firewalls drop unrequested trace packets.



\*\*Question 2: In your traceroute output (`02\_traceroute\_hops.png`), why did only the first hop return an IP address while all subsequent hops timed out?\*\*



\* \*\*Answer:\*\* By default, Linux implementations of `traceroute` send high-port UDP probe packets rather than ICMP requests. While the local hypervisor gateway (`10.0.2.2`) processed and responded to the packet's expiring Time-to-Live (TTL), the host network's physical router or edge firewall is configured to drop or filter outbound/inbound UDP diagnostic probes for security hardening, masking the rest of the public transit topology.



\---



\## 4. Laboratory Reflection



This laboratory successfully demonstrated the fundamental differences between active connection verification (`ping`) and route tracking (`traceroute`). While the ICMP echo requests bypassed perimeter restrictions flawlessly to confirm internet access, the traceroute attempt highlighted a real-world security baseline: modern network perimeters purposefully drop unexpected UDP probes to prevent reconnaissance mapping by potential adversaries.







