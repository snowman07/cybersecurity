<h1 style="color: skyblue; font-size: 55px;">Traffic Capture</h1>

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
This document is about the concepts of network traffic analysis using different options with an emphasis on using Wireshark, a widely used network protocol analyzer. The purpose of Traffic Capture Lab is to understand how data moves within a switched network environment, how network switches handle traffic between connected hosts, and how to use network protocol analyzer to capture traffic at multiple area in the network. By capturing and analyzing different types of network traffic (such as ARP, ICMP, TCP, UDP, HTTP, etc.), this will give an insight on how communication occurs at various layers of the OSI model.

## 2 Network Diagram 
This is the network layout which consists of one network switch, one firewall to serve as the gateway for all the virtual machines, and three virtual machines. All client virtual machines have Wireshark installed. For VLAN 10, the routing is configured so it can communicate VLAN 20 as well as the internet. VLAN 20 is in another VLAN and can access internet only. VLAN 30 is connected to a span port and captures traffic on trunk link. Implementation of Switch Port Analyzer (SPAN) was configured in the switch with interface E0/0 as the source and interface E0/3 as the destination.

![Figure 1. Network diagram of Traffic Capture Lab.](./screenshots/network%20diagram%20-%20traffic%20capture%204.jpg)

*Figure 1. Network diagram of Traffic Capture Lab*

## 3 Wireshark Setup
Wireshark is the world’s leading network protocol analyzer, used by network administrators, security professionals, and developers to capture, inspect, and troubleshoot network traffic in real time. It is particularly useful for troubleshooting network issues, analyzing network protocols and ensuring network security.

### 3.1 Wireshark in Kali Linux
By default, Wireshark in Kali Linux is automatically installed. To open Wireshark:

•	Click Kali Linux icon and type “Wireshark

![Figure 2. Opening Wireshark in Kali Linux.](./screenshots/1%20wireshark%20in%20kali/1%20wireshark%20in%20kali.jpg)

*Figure 2. Opening Wireshark in Kali Linux.*

•	This is how Wireshark looks like. It would show list of interfaces available in the system.

![Figure 3. Wireshark UI.](./screenshots/1%20wireshark%20in%20kali/2%20wireshark%20UI.jpg)

*Figure 3. Wireshark UI.*

•	On the list of available interfaces, double-click “eth0”. This will show everything interface “eth0” sees in the network.

![Figure 4. Interface eth0 in action.](./screenshots/1%20wireshark%20in%20kali/3%20eth0%20in%20action.jpg)

*Figure 4. Interface eth0 in action.*

•	To initiate traffic, try to ping the VLAN10 gateway (192.168.10.1) from Kali Linux VM.

![Figure 5. Ping the gateway to initiate traffic in Wireshark.](./screenshots/1%20wireshark%20in%20kali/4%20initiate%20traffic.jpg)

*Figure 5. Ping the gateway to initiate traffic in Wireshark.*

•	As the ping continues running, go back to Wireshark and filter out the protocol. For example, try to filter ICMP protocol to check the traffic specifically for ICMP.

![Figure 6. IMCP traffic in Wireshark.](./screenshots/1%20wireshark%20in%20kali/5%20icmp%20traffic%20in%20wireshark.jpg)

*Figure 6. IMCP traffic in Wireshark.*

### 3.2 Wireshark in Windows (Client20 VM and Client30 VM)

Unlike Kali Linux, Wireshark does not come pre-installed in Windows OS. To install Wireshark:

•	Go to www.wireshark.org/download.html and choose the stable release for Windows.

![Figure 7. Download Wireshark in Windows 11.](./screenshots/2%20wireshark%20in%20windows/1%20dl%20wireshark.jpg)

*Figure 7. Download Wireshark in Windows 11.*

•	Once downloaded, click “Run”. Follow the installation steps and just keep the default options and click “Install”.

•	Try to ping VLAN20 gateway (1962.168.20.1) and open the Wireshark application to see the traffic.

![Figure 8. These are the traffic while pinging VLAN20 gateway.](./screenshots/2%20wireshark%20in%20windows/2%20icmp%20traffic%20for%20vlan20.jpg)

*Figure 8. These are the traffic while pinging VLAN20 gateway.*


## 4 Other Traffic Analyzer

### 4.1 Packet Capture
Packet Capture is helpful in doing this lab in CML. However, please note that Packet Capture is not a tool used in the real-world industry. So, to capture the traffic using Packet Capture, follow these steps:

•	In the CML diagram, right-click on any links where you want to capture the traffic and click “Packet Capture”. 

![Figure 9. Another way to capture the traffic in CML.](./screenshots/3%20packet%20capture/6%20packet%20capture.jpg)

*Figure 9. Another way to capture the traffic in CML.*

•	Packet Capture window will appear and click “Start”.

![Figure 10. Starting the packet capture.](./screenshots/3%20packet%20capture/7%20start%20packet%20capture.jpg)

*Figure 10. Starting the packet capture.*

•	Once started, Packet Capture will show all the traffic on that link. You can also search for specific protocols in the search bar.

![Figure 11. Lists of all the traffic in Packet Capture.](./screenshots/3%20packet%20capture/8%20list%20of%20traffic%20in%20Packet%20Capture.jpg)

*Figure 11. Lists of all the traffic in Packet Capture.*

•	Try to ping VLAN10 gateway (192.168.10.1) and search for specific traffic in Packet Capture (ICMP for example).

![Figure 12. ICMP traffic in Packet Capture.](./screenshots/3%20packet%20capture/9%20icmp%20traffic%20in%20packet%20capture.jpg)

*Figure 12. ICMP traffic in Packet Capture.*


### 4.2 SPAN (Switch Port ANalyzer)
It is a network monitoring tool commonly used for network traffic or troubleshoot issues. What it does is it copy and forward traffic from one or more switch ports to another port (called the destination port) for analysis. It is sometimes called “port mirroring” and think of it as a "watchtower" in your network that serve as a second set on your network to ensure everything flows smoothly and helping spot problems before they escalate.

#### 4.2.1 Before implementation of SPAN
Kali Linux VM can only see traffic within its own network, and not able to see any traffic from other network.

![Figure 13. Before implementation of SPAN, Wireshark is taking traffic only on 10 networks.](./screenshots/4%20span/1%20before%20implementation%20of%20span.jpg)

*Figure 13. Before implementation of SPAN, Wireshark is taking traffic only on 10 networks.*

#### 4.2.2 Implementation of SPAN
The goal of SPAN is to copy and forward the traffic from one network to another network. 

![Figure 14. Diagram of SPAN implementation.](./screenshots/4%20span/2%20implementation%20of%20span%20diagram.jpg)

*Figure 14. Diagram of SPAN implementation.*

In order to do that, it needs a configuration in the switch:

•	Type “monitor session 1 source interface e0/0” and press enter.

•	Type “monitor session 1 destination interface e0/1” and press enter.

![Figure 15. Switch command to implement SPAN.](./screenshots/4%20span/3%20implementation%20of%20span%20console.jpg)

*Figure 15. Switch command to implement SPAN.*

•	To cancel sending the traffic of one network, type “no monitor session 1” and press enter. 

#### 4.2.3 After implementation of SPAN

•	To verify if Kali Linux VM can see other network traffic (20 network), ping VLAN20 gateway (192.168.20.1) from Client20 VM to initiate traffic in interface e0/0.

![Figure 16. Initiating traffic from Client20 VM.](./screenshots/4%20span/4%20after%20implementation%20of%20span%20client20.jpg)

*Figure 16. Initiating traffic from Client20 VM.*

•	Go back to Wireshark in Kali Linux VM. Notice that aside from capturing traffic from 10 network, it also captures traffic from 20 network.

![Figure 17. Successful implementation of SPAN in Kali Linux VM and Client20 VM.](./screenshots/4%20span/5%20after%20implementation%20of%20span%20verify%20in%20kali.jpg)

*Figure 17. Successful implementation of SPAN in Kali Linux VM and Client20 VM.*

## 5 VM Setup for Connectivity and Traffic Generation

### 5.1 Inter-VLAN routing for VM 1 (VLAN 10) to VM 2 (VLAN 20) 

Per requirement, Client VM 1 (VLAN 10) has access to Client VM 2 (VLAN 20) and the internet. However, Client VM 2 (VLAN 20) has NO ACCESS to Client VM 1 (VLAN 10). So, to enable communication between VLAN 10 and VLAN 20, configuration is needed in the firewall.

#### 5.1.1 Opening Firewall (FW01) UI
•	Start the firewall (FW01) as well as all other devices.

•	Right click the firewall (FW01) and choose “Console”. You know firewall is done booting when you are able to see the serial number and the firewall login.

•	Type “cisco” in the “Firewall login” and “Password”.

•	Wait until you see “Welcome”.

•	Type “get system interface physical port1” to get information about port 1 that is connected to the internet. Open the IP address in another browser.

![Figure 18. Opening the firewall to configure inter-vlan communication.](./screenshots/5b%20inter%20vlan%20vm1%20and%20vm2%20-%20firewall%20-%20v2/1%20opening%20the%20firewall%20to%20configure%20inter-vlan%20communication.jpg)

*Figure 18. Opening the firewall to configure inter-vlan communication.*

•	On the browser, click “Advanced”.

•	Click “Proceed to 192.168.202.146(unsafe)”.

•	Type “cisco” as the Username and Password, then click “Login”.

•	Click "Login Read-Write".

•	Click "Yes".

•	Click "Begin".

•	In the Dashboard Setup, choose the default which is "Optimal" and press "OK".

•	Firewall (FW01) dashboard will open up.

#### 5.1.2 Default Firewall Policy
•	To control access between different VLAN (VLAN 10 to VLAN 20), go to “Policy & Objects” then click “Firewall Policy”. 

•	By default, there is “Implicit” policy that deny an access to everything (it is like a door lock which by default is automatically locked and someone will allow to do anything).  

![Figure 19. Implicit policy is the default "deny all" rule at the end of the firewall policy list that blocks any traffic not explicitly matched by a previous "allow" policy.](./screenshots/5b%20inter%20vlan%20vm1%20and%20vm2%20-%20firewall%20-%20v2/2%20default%20policy.jpg)

*Figure 19. Implicit policy is the default "deny all" rule at the end of the firewall policy list that blocks any traffic not explicitly matched by a previous "allow" policy.*

#### 5.1.3 Create a policy for VLAN10 to VLAN20 communication 
•	On the same screen, click “Create new”.

•	Enter a meaningful “Name” (Allow_VLAN10_VLAN20_Communication).

•	Set the “Schedule” to “always”.

•	Make sure “Action” is set to “ACCEPT”.

•	On the “Incoming interface”, choose “VLAN10”.

•	On the “Outgoing interface”, choose “VLAN20”.

•	Be specific on the “Source”, choose “VLAN10 address”.

•	Be specific on the “Destination”, choose “VLAN20 address”.

•	On “Service”, choose "ALL_ICMP” (VLAN10 can ping VLAN20).

•	Disable “NAT” as it is not required between VLAN communication and it will cause slow performance (NAT is applicable only for internet connection).

•	Click OK.

![Figure 20. Policy for VLAN10 to communicate to VLAN20.](./screenshots/5b%20inter%20vlan%20vm1%20and%20vm2%20-%20firewall%20-%20v2/3%20policy%20for%20vlan10%20to%20vlan20.jpg)

*Figure 20. Policy for VLAN10 to communicate to VLAN20.*

#### 5.1.4 Check connectivity
Remember that Client VM 1 (VLAN 10) has access to Client VM 2 (VLAN 20) and the internet. However, Client VM 2 (VLAN 20) has NO ACCESS to Client VM 1 (VLAN 10).

•	In VLAN10, it should be able to:
  -	ping 8.8.8.8 (internet)
  -	ping 192.168.10.1 (VLAN10 own default gateway)
  -	ping 192.168.20.1 (VLAN20 default gateway)
  -	ping 192.168.20.51 (VLAN 20 IP)

![Figure 21. Successful connection from Client VM 1 (VLAN10).](./screenshots/5b%20inter%20vlan%20vm1%20and%20vm2%20-%20firewall%20-%20v2/4%20check%20connectivity%20vlan10.jpg)

*Figure 21. Successful connection from Client VM 1 (VLAN10).*

•	In VLAN20, it should be able to:
  -	ping 8.8.8.8 (internet)
  -	ping 192.168.20.1 (VLAN20 own default gateway)
  And NOT be able to ping:
  -	ping 192.168.10.1 (VLAN10 default gateway)
  -	ping 192.168.10.51 (VLAN 10 IP)

![Figure 22. Connection status from VLAN20.](./screenshots/5b%20inter%20vlan%20vm1%20and%20vm2%20-%20firewall%20-%20v2/5%20check%20connectivity%20vlan20.jpg)

*Figure 22. Connection status from VLAN20.*

### 5.2 SPAN Configuration for VM 3 (VLAN 30) 

Per requirement, Client VM 3 (VLAN 30) is connected to a SPAN port, capturing traffic on a trunk link. To do that, it needs configuration to the switch:

•	Go to switch (SW01) console.

•	Type “enable” and press enter.

•	Type “config terminal” and press enter to go to configuration mode.

•	Type “monitor session 1 source interface e0/0” and press enter.

•	Type “monitor session 1 destination interface e0/3” and press enter.

•	Exit the configuration mode by pressing Ctrl+Z.

![Figure 23. Configuration of SPAN to VLAN 30.](./screenshots/6%20vm3%20configuration/1%20configure%20SPAN%20to%20vm3.jpg)

*Figure 23. Configuration of SPAN to VLAN 30.*

•	Type “show run” and press enter to double check the configuration. Scroll down until you see below image.

![Figure 24. After SPAN configuration in switch, run the command “show run” and the result should be like this.](./screenshots/6%20vm3%20configuration/2%20show%20run.jpg)

*Figure 24. After SPAN configuration in switch, run the command “show run” and the result should be like this.*

•	To cancel sending the traffic of one network, type “no monitor session 1” and press enter. 

## 6 Packet Analysis 

This section will cover different traffic capture points in the network such as TCP three-way handshake, TCP four-way teardown, and TTL.
	To initiate traffic:

•	Start the three VM (VLAN10, VLAN20, VLAN30) simultaneously.

•	Open the Wireshark for all the virtual machines and start to capture packets.

•	On VLAN10, ping the internet (8.8.8.8), ping the default gateway of both VLAN10 and VLAN20, ping the other VM in VLAN20, as well as open Mozilla Firefox and search for anything on the internet.

•	On VLAN20, ping the internet (8.8.8.8) and ping the default gateway VLAN20.

•	On VLAN30, just start Wireshark as this is a dedicated SPAN destination port. This VM is solely for monitoring devices and not to be used for any regular network operations.

•	Below are the screenshots after traffic capture for all virtual machines:

![Figure 25. Wireshark in VLAN10 generated 22,852 packets.](./screenshots/7%20packet%20analysis/1%20kali%20wireshark.jpg)

*Figure 25. Wireshark in VLAN10 generated 22,852 packets.*


![Figure 26. Wireshark in VLAN20 generated 59,006 packets.](./screenshots/7%20packet%20analysis/2%20vlan20%20wireshark.jpg)

*Figure 26. Wireshark in VLAN20 generated 59,006 packets.*

![Figure 27. Wireshark in VLAN30 generated 91,546 packets.](./screenshots/7%20packet%20analysis/3%20vlan30%20wireshark.jpg)

*Figure 27. Wireshark in VLAN30 generated 91,546 packets.*

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