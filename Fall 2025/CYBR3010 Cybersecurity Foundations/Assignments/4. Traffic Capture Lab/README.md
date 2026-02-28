<h1>Traffic Capture</h1>

![1_nQ4UJ9xbBr0C8yEm-II7aQ](https://github.com/user-attachments/assets/d95e10f0-981c-4af5-815d-b2a6df5cdc99)

<!-- TOC -->

- [1 Introduction](#1-introduction)
- [2 Network Diagram](#2-network-diagram)
- [3 Wireshark Setup](#3-wireshark-setup)
  - [3.1 Wireshark in Kali Linux](#31-wireshark-in-kali-linux)
  - [3.2 Wireshark in Windows (Client20 VM and Client30 VM)](#32-wireshark-in-windows-client20-vm-and-client30-vm)
- [4 Other Traffic Analyzer](#4-other-traffic-analyzer)
  - [4.1 Packet Capture](#41-packet-capture)
  - [4.2 SPAN (Switch Port ANalyzer)](#42-span-switch-port-analyzer)
    - [4.2.1 Before implementation of SPAN](#421-before-implementation-of-span)
    - [4.2.2 Implementation of SPAN](#422-implementation-of-span)
    - [4.2.3 After implementation of SPAN](#423-after-implementation-of-span)
- [5 VM Setup for Connectivity and Traffic Generation](#5-vm-setup-for-connectivity-and-traffic-generation)
  - [5.1 Inter-VLAN routing for VM 1 (VLAN 10) to VM 2 (VLAN 20)](#51-inter-vlan-routing-for-vm-1-vlan-10-to-vm-2-vlan-20)
    - [5.1.1 Opening Firewall (FW01) UI](#511-opening-firewall-fw01-ui)
    - [5.1.2 Default Firewall Policy](#512-default-firewall-policy)
    - [5.1.3 Create a policy for VLAN10 to VLAN20 communication](#513-create-a-policy-for-vlan10-to-vlan20-communication)
    - [5.1.4 Check connectivity](#514-check-connectivity)
  - [5.2 SPAN Configuration for VM 3 (VLAN 30)](#52-span-configuration-for-vm-3-vlan-30)
- [6 Packet Analysis](#6-packet-analysis)
  - [6.1 TCP three-way handshake](#61-tcp-three-way-handshake)
    - [6.1.a Identify the packets in the complete TCP three-way handshake process.](#61a-identify-the-packets-in-the-complete-tcp-three-way-handshake-process)
    - [6.1.b Provide a brief description of the TCP three-way handshake process and explain how you identified the packets captured as being part of the same TCP conversation.](#61b-provide-a-brief-description-of-the-tcp-three-way-handshake-process-and-explain-how-you-identified-the-packets-captured-as-being-part-of-the-same-tcp-conversation)
    - [6.1.c List and describe the TCP options used in the conversation.](#61c-list-and-describe-the-tcp-options-used-in-the-conversation)
  - [6.2 TCP four-way teardown](#62-tcp-four-way-teardown)
    - [6.2.a Identify the packets in a complete TCP four-way teardown process.](#62a-identify-the-packets-in-a-complete-tcp-four-way-teardown-process)
    - [6.2.b Provide a brief description of the TCP four-way teardown process and explain how you identified the packets captured as being part of the same TCP conversation.](#62b-provide-a-brief-description-of-the-tcp-four-way-teardown-process-and-explain-how-you-identified-the-packets-captured-as-being-part-of-the-same-tcp-conversation)
  - [6.3 TTL (Time to Live)](#63-ttl-time-to-live)
    - [6.3.a Choose an IP packet that is part of communication between the two client VMs.](#63a-choose-an-ip-packet-that-is-part-of-communication-between-the-two-client-vms)
    - [6.3.b Apply a display filter to isolate that same packet in all three capture points: Source | Network (SPAN) | Destination](#63b-apply-a-display-filter-to-isolate-that-same-packet-in-all-three-capture-points-source--network-span--destination)
    - [6.3.c Provide screenshots from each capture showing: TTL value | Source MAC | Destination MAC | The display filter used](#63c-provide-screenshots-from-each-capture-showing-ttl-value--source-mac--destination-mac--the-display-filter-used)
    - [6.3.d Explain the TTL field and the MAC addresses shown, including why they differ between capture points.](#63d-explain-the-ttl-field-and-the-mac-addresses-shown-including-why-they-differ-between-capture-points)
- [References](#references)

<!-- /TOC -->

## 1 Introduction


## 2 Network Diagram 

## 3 Wireshark Setup

### 3.1 Wireshark in Kali Linux


### 3.2 Wireshark in Windows (Client20 VM and Client30 VM)

## 4 Other Traffic Analyzer


### 4.1 Packet Capture


### 4.2 SPAN (Switch Port ANalyzer)

#### 4.2.1 Before implementation of SPAN

#### 4.2.2 Implementation of SPAN


#### 4.2.3 After implementation of SPAN

## 5 VM Setup for Connectivity and Traffic Generation

### 5.1 Inter-VLAN routing for VM 1 (VLAN 10) to VM 2 (VLAN 20) 

#### 5.1.1 Opening Firewall (FW01) UI


#### 5.1.2 Default Firewall Policy


#### 5.1.3 Create a policy for VLAN10 to VLAN20 communication 


#### 5.1.4 Check connectivity


### 5.2 SPAN Configuration for VM 3 (VLAN 30) 

## 6 Packet Analysis 

### 6.1 TCP three-way handshake


#### 6.1.a Identify the packets in the complete TCP three-way handshake process. 

#### 6.1.b Provide a brief description of the TCP three-way handshake process and explain how you identified the packets captured as being part of the same TCP conversation.

#### 6.1.c List and describe the TCP options used in the conversation. 


### 6.2 TCP four-way teardown

#### 6.2.a Identify the packets in a complete TCP four-way teardown process. 

#### 6.2.b Provide a brief description of the TCP four-way teardown process and explain how you identified the packets captured as being part of the same TCP conversation.


### 6.3 TTL (Time to Live)

#### 6.3.a Choose an IP packet that is part of communication between the two client VMs.

 
#### 6.3.b Apply a display filter to isolate that same packet in all three capture points: Source | Network (SPAN) | Destination


#### 6.3.c Provide screenshots from each capture showing: TTL value | Source MAC | Destination MAC | The display filter used


#### 6.3.d Explain the TTL field and the MAC addresses shown, including why they differ between capture points. 

## References