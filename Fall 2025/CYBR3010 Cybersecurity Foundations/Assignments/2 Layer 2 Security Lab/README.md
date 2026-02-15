

<h1> Layer 2 Security </h1> 

![packtrav-host-switch-host](https://github.com/user-attachments/assets/b08c62ca-5ea8-4d1b-bdd3-bea7f83f5a06)


<!-- TOC -->

- [1 Introduction](#1-introduction)
- [2 Network Diagram](#2-network-diagram)
- [3 MAC Address Table](#3-mac-address-table)
- [4 Layer 2 vulnerabilities and potential impact](#4-layer-2-vulnerabilities-and-potential-impact)
  - [4.1 MAC Flooding](#41-mac-flooding)
  - [4.2 MAC Spoofing](#42-mac-spoofing)
  - [4.3 ARP Spoofing](#43-arp-spoofing)
- [5 Test results (before and after scenarios)](#5-test-results-before-and-after-scenarios)
  - [5.1 MAC flooding before launching the attack in Kali Linux VM](#51-mac-flooding-before-launching-the-attack-in-kali-linux-vm)
  - [5.2 MAC flooding after launching the attack in Kali Linux VM](#52-mac-flooding-after-launching-the-attack-in-kali-linux-vm)
  - [5.3 MAC spoofing before launching the attack in Kali Linux VM](#53-mac-spoofing-before-launching-the-attack-in-kali-linux-vm)
  - [5.4 MAC spoofing after launching the attack in Kali Linux VM](#54-mac-spoofing-after-launching-the-attack-in-kali-linux-vm)
  - [5.5 ARP Spoofing before launching the attack in Kali Linux VM](#55-arp-spoofing-before-launching-the-attack-in-kali-linux-vm)
  - [5.6 ARP Spoofing after launching the attack in Kali Linux VM](#56-arp-spoofing-after-launching-the-attack-in-kali-linux-vm)
- [6 Prevention and Mitigation](#6-prevention-and-mitigation)
  - [6.1 MAC flooding attack – security measures](#61-mac-flooding-attack--security-measures)
  - [6.2 MAC spoofing attack – security measures](#62-mac-spoofing-attack--security-measures)
  - [6.3 ARP Spoofing attack – security measure](#63-arp-spoofing-attack--security-measure)

<!-- /TOC -->

## 1 Introduction

This document is about `Data Link Layer` (layer 2 of the OSI model). Switch (as well as bridge) is the common device operating here, which maintains an address table called MAC address table to efficiently switch frames between interfaces on the same Local Area Network (LAN). Simply put, it’s like a directory that links devices’ unique hardware addresses to the ports they’re connected to on a switch. When a switch receives a frame, it associates the MAC address of the sending device with the switch port on which it was received. Because switch is responsible for how devices communicate within LAN, data link layer is a common target for network attack. Therefore, this document will also tackle `vulnerabilities in data link layer`, compare results before and after the attack, and apply `security measures to mitigate the vulnerabilities` effectively.

## 2 Network Diagram

This is the network layout which consists of multiple virtual machines (VMs) with MAC address running on different operating systems (Windows, Linux), as well as a virtual cisco switch configured to communicate with other network devices.

![Figure 1. Network Diagram for Layer 2 Security Lab.](./screenshot/lab%202%20network%20diagram.jpg)

*Figure 1. Network Diagram for Layer 2 Security Lab.*

## 3 MAC Address Table

MAC Address Table is a data structure used by network switches to map MAC (Media Access Control) addresses to specific switch ports. To display the switch’s MAC address table, enter the command “show mac address-table” in privileged EXEC mode after connecting to the switch's CLI. This command will show a table of MAC addresses, their associated VLANs, how they were learned (static or dynamic), and the port on which they were learned.

![Figure 2. Network diagram with the associated MAC address table.](./screenshot/mac%20table%20with%20connections.jpg)

*Figure 2. Network diagram with the associated MAC address table.*

In this diagram, switch’s port e0/1 is where Kali Linux is connected with MAC address 5254.0000.85e3, vlan of 1, and of dynamic type. Switch’s port e0/2 is where Windows 10 is connected with MAC address 5254.0086.0FFF, vlan of 1, and of dynamic type. Switch’s port e0/3 is where Windows 11 is connected with MAC address 5254.002C.82E4, vlan of 1, and of dynamic type.

To be more precise, check the table below.

| Virtual Machine | VLAN | MAC Address | Type | Ports |
|----------|----------|----------|----------|----------|
| Kali Linux  | 1  | 5254.0000.85e3  | Dynamic  | E0/1  |
| Windows 10  | 1  | 5254.0086.0FFF  | Dynamic  | E0/2  |
| Windows 11  | 1  | 5254.002C.82E4  | Dynamic | E0/3  |


## 4 Layer 2 vulnerabilities and potential impact

### 4.1 MAC Flooding

It is a cyberattack that targets network switches on a Local Area Network (LAN) and tries to steal user data. Every device has a MAC address, a unique numerical signifier used to identify the device within a network. The attacker uses the command “macof”, which floods the local network with random fake MAC addresses, causing some switches to fail open in repeating mode which in turn facilitate sniffing.

As a result, sensitive data can be intercepted by attackers and can lead to data breaches, financial losses, and damage to an organization’s reputation. By overloading a network switch, attackers can disrupt its functionality and block legitimate traffic. This causes serious issues like efficiency, increased delays, or even a complete denial of service for authorized users. Furthermore, when network traffic is broadcast to all devices, attackers can intercept sensitive information they wouldn’t normally have access to. This includes login credentials, private data, or communications meant for restricted systems. Such unauthorized access can compromise the security of the entire network and lead to further exploitation.

### 4.2 MAC Spoofing

MAC spoofing is modifying the MAC address of a device to imitate another device present on the network. In a MAC spoofing attack, the hacker changes their device’s MAC address to match a legitimate device’s address, connects to the network, and intercepts or redirects data intended for the legitimate device.

Several tools can be used to change the MAC address of network interface card, and for this purpose, it will use MAC Changer. By spoofing a MAC address, attackers can intercept data intended for a legitimate device, leading to risks such as session hijacking and man-in-the-middle attacks. Attackers can also gain unauthorized access to a network which can create unauthorized access points, disrupt network operations and make it difficult for legitimate users to log on and share resources. Finally, attackers can steal network credentials leading to potential identity theft.

### 4.3 ARP Spoofing

ARP (Address Resolution Protocol) spoofing is a type of attack in which a malicious actor sends falsified ARP messages over a local area network. This results in the linking of an attacker’s MAC address with the IP address of a legitimate computer or server on the network. Once the attacker’s MAC address is connected to an authentic IP address, the attacker will begin receiving any data that is intended for that IP address.

Generally speaking, the goal of ARP spoofing is to steal sensitive information by targeting vulnerable websites or stealing cookies. Other than websites, a Man-in-the-Middle (MITM) attack can happen in any form of online communication such as email, DNS lookups, social media and so on. This security breach exploits real-time transactions and conversations by intercepting data that is meant to be secure, and it is usually too late by the time either of the affected party realizes what has transpired.

## 5 Test results (before and after scenarios)

### 5.1 MAC flooding before launching the attack in Kali Linux VM

- In CML, right-click the switch then press “Start”.
- Right-click the switch again then press “Console”.
- Click “Open Console”.
- In the console, type “enable” then press enter.
- Type “configure” then press enter.
- If you see “Configuring from terminal, memory, or network [terminal]”, just press enter.
- Inside the config, type the word “hostname” plus name of the device to enter the device configuration, then press enter. Ex. “hostname SW01”.
- Type “exit”.
- Considering all the virtual machines were turned-on, type “show mac address-table” in switch.
- MAC address table will display MAC addresses that are connected to the switch.

![Figure 3. Before the MAC flooding attack, there are total of 3 MAC addresses connected to the switch.](./screenshot/switch%20mac%20table.jpg)

*Figure 3. Before the MAC flooding attack, there are total of 3 MAC addresses connected to the switch.*

### 5.2 MAC flooding after launching the attack in Kali Linux VM

### 5.3 MAC spoofing before launching the attack in Kali Linux VM

### 5.4 MAC spoofing after launching the attack in Kali Linux VM

### 5.5 ARP Spoofing before launching the attack in Kali Linux VM

### 5.6 ARP Spoofing after launching the attack in Kali Linux VM


## 6 Prevention and Mitigation

### 6.1 MAC flooding attack – security measures

### 6.2 MAC spoofing attack – security measures

### 6.3 ARP Spoofing attack – security measure