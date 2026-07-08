# Lab 1: Accessing and Using the Virtual Machines

## 1. Laboratory Overview & Objectives

The objective of this lab is to configure, deploy, and familiarize myself with the virtualized sandbox environment used throughout the Cisco CyberOps curriculum. This involves utilizing Oracle VM VirtualBox to run a specialized Arch Linux environment ("CyberOps Workstation"), verifying system resources, and confirming operational network connectivity.

* **Host System:** Windows 11 
* **Guest System:** Arch Linux/ubuntu 64-bit (CyberOps Workstation)
* **Hypervisor:** Oracle VM VirtualBox

---

## 2. Step-by-Step Task Execution & Evidence

### Part 1: Booting the CyberOps Workstation & Verifying Network Interfaces

1. Booted the Arch Linux system environment within the hypervisor.
2. Authenticated into the workstation using the security analyst credentials (`analyst` / `cyberops`).
3. Opened a terminal emulator and executed the network interface command `ip a` to map the operational network layers.

> **📸 Verification Screenshot 1: Active Terminal and IP Address Mapping**
> ![Terminal Verification Status](./screenshots/IP%20address(NAT).png)

### Part 2: Verifying Web Connectivity

1. Opened the Mozilla Firefox web browser application inside the guest operating system.
2. Successfully navigated to an external web index to verify that the VirtualBox NAT network configurations are accurately translating packets to the internet.

> **📸 Verification Screenshot 2: Web Browser Connectivity Check**
> ![Web Browser Connection Status](./screenshots/Working%20firefox%20%20inside%20cyberops%20VM.png)

---

## 3. Lab Questions & Technical Analysis

**Question 1: What are the advantages of using a virtual machine instead of a physical operating system for cybersecurity analysis and malware testing?**

* **Answer:** Virtual machines provide an isolated, sandboxed environment that prevents malicious code or exploits from leaking onto the host machine. They also feature snapshot and rollback capabilities, allowing an analyst to easily discard any system changes and return to a clean baseline configuration after running tests or analyzing threats.

**Question 2: Why is it important to understand the keyboard shortcuts or 'Host Key' combinations when interacting with a hypervisor terminal?**

* **Answer:** Understanding the Host Key is essential for maintaining control over the workspace, as it releases the mouse cursor and keyboard focus from the isolated guest operating system back to the host system. This ensures seamless navigation between documentation on the host machine and operations inside the VM without interrupting background tasks.

---

## 4. Laboratory Reflection

The installation and initialization of the CyberOps Workstation environment were completed successfully. By investigating the network interfaces via the command line interface and verifying external browsing capabilities, I confirmed that the hypervisor network translation layers are functioning as expected. The environment is now fully stabilized and prepared for localized traffic capture and packet inspection labs.
