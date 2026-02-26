<h1>Layer 3 and Layer 4 Security</h1>

![alt text](packtrav-encap-decap.gif)

![alt text](65f854814fd223fc3678f041_OSI-Network-Layer.gif)

![alt text](65f854814fd223fc3678f03f_OSI-Transport-Layer.gif)

<!-- TOC -->

- [1 Introduction](#1-introduction)
- [2 Network Diagram](#2-network-diagram)
- [3 IP Address Configuration and NAT Configuration](#3-ip-address-configuration-and-nat-configuration)
  - [3.1 Configure static IP address of Kali Linux](#31-configure-static-ip-address-of-kali-linux)
  - [3.2 Configure static IP address of Client 20 and Client 30 VM (both Windows 11)](#32-configure-static-ip-address-of-client-20-and-client-30-vm-both-windows-11)
  - [3.3 Configure the Firewall with static IP address](#33-configure-the-firewall-with-static-ip-address)
  - [3.4 Configure the Firewall to be a DHCP Server (for dynamic IP addressing)](#34-configure-the-firewall-to-be-a-dhcp-server-for-dynamic-ip-addressing)
  - [3.5 NAT Configuration](#35-nat-configuration)
- [4 Vulnerabilities and potential impact](#4-vulnerabilities-and-potential-impact)
  - [4.1 Network reconnaissance using Zenmap](#41-network-reconnaissance-using-zenmap)
  - [4.2 ICMP Ping Flood Attack](#42-icmp-ping-flood-attack)
- [5 Test Results (before and after scenarios)](#5-test-results-before-and-after-scenarios)
  - [5.1a Before the actual cyber-attack, perform network reconnaissance using Zenmap](#51a-before-the-actual-cyber-attack-perform-network-reconnaissance-using-zenmap)
  - [5.1b After the successful network reconnaissance using Zenmap](#51b-after-the-successful-network-reconnaissance-using-zenmap)
  - [5.2a Before ICMP Ping Flood Attack](#52a-before-icmp-ping-flood-attack)
  - [5.2b After ICMP Ping Flood Attack](#52b-after-icmp-ping-flood-attack)
- [6 Prevention and Mitigation](#6-prevention-and-mitigation)
  - [6.1 Implementation of VLAN as protection to network reconnaissance using Zenmap](#61-implementation-of-vlan-as-protection-to-network-reconnaissance-using-zenmap)
    - [6.1.1 VLAN configuration in switch](#611-vlan-configuration-in-switch)
    - [6.1.2 VLAN configuration in firewall](#612-vlan-configuration-in-firewall)
  - [6.2 Implementation to prevent and mitigate ICMP ping flood attack](#62-implementation-to-prevent-and-mitigate-icmp-ping-flood-attack)
- [References](#references)

<!-- /TOC -->

## 1 Introduction

This document is about Layer 3 and Layer 4 of the OSI (Open Systems Interconnection) Model, which is the Network Layer (IP and ICMP are the common protocols) and Transport Layer (TCP and UDP are the common protocols). It is in Layer 3 where routing, packet forwarding, and logical addressing happens with the use of an IP address; hence this document will cover on how to configure IP addresses statically and dynamically through enabling Dynamic Host Control Protocol (DHCP) on a firewall. For Layer 4, Transport layer ensures the reliable and efficient data delivery through segmentation; hence this document will cover the configuration of VLAN on the firewall and on the switch to segment the network for better traffic control and security. Moreover, this document will go through the configuration of NAT to allow VMs to access the internet. Finally, network layer and transport layer are prone to cyberattacks. Therefore, this document will tackle the vulnerabilities and potential impact associated with this layer, compare results before and after the attack, and apply security measures to mitigate these vulnerabilities effectively.

## 2 Network Diagram

This is the network layout consisting of multiple virtual machines (VMs), switch, and a virtual firewall. For the client virtual machines, there are three configurations made to set IP addresses that is why there are different IP addresses attached to each VM. Those three configurations for IP addresses are labeled respectively such as static, dynamic, and virtual local area network (VLAN).

![Figure 1. Network diagram of Layer 3 and Layer 4 Security.](./screenshots/network%20diagram%20of%20layer%203%20and%20layer%204.jpg)

*Figure 1. Network diagram of Layer 3 and Layer 4 Security.*

## 3 IP Address Configuration and NAT Configuration

An IP (Internet Protocol) Address is a unique numerical label assigned to each device connected to a computer network which serves two main purposes: to identify a device on a network, and to locate the device that enables communication with other devices over the Internet. There are two ways to configure an IP address: static and dynamic. Static IP addresses are manually assigned and remain the same unless changed by an administrator, while dynamic IP addresses allow a device to automatically obtain network configuration information, including an IP address, from a DHCP server on the network.

### 3.1 Configure static IP address of Kali Linux

- Right-click “Network Connections” and click “Edit Connections”.

![Figure 2. Starting point to configure static IP address of Kali Linux.](./screenshots/1%20configure%20ip%20of%20kali/1%20kali%20network%20connections.jpg)

*Figure 2. Starting point to configure static IP address of Kali Linux.*

- Choose “Wired connection 1” and click the gear icon.

![Figure 3. Choosing the connection.](./screenshots/1%20configure%20ip%20of%20kali/2%20kali%20wired%20connection.jpg)

*Figure 3. Choosing the connection.*

- On “Editing Wired connection 1” window, go to “IPv4 Settings” tab. Method is “Manual”. Click “Add” button and put the following details:
  - Address: 192.168.0.11
  - Netmask: 255.255.255.0 (this is full format, can also be 24 format)
  - Gateway: 192.168.0.1
  - DNS servers: 8.8.8.8
- Click “Save”.

![Figure 4. Step to edit the connection and enter necessary details.](./screenshots/1%20configure%20ip%20of%20kali/3%20edit%20connection.jpg)

*Figure 4. Step to edit the connection and enter necessary details.*

- At this point, IP address of Kali Linux is configured. To verify the IP address of Kali Linux, open terminal and type “ifconfig”.

![Figure 5. Static IP address of Kali Linux is 192.168.0.11.](./screenshots/1%20configure%20ip%20of%20kali/4%20verify%20ip.jpg)

*Figure 5. Static IP address of Kali Linux is 192.168.0.11.*

- Considering other client VMs are properly configured, to verify the connection, ping the device using its own IP address and IP address of other devices.

![Figure 6. Successful ping for both self and other devices.](./screenshots/1%20configure%20ip%20of%20kali/5%20verify%20connections%20to%20other%20device.jpg)

*Figure 6. Successful ping for both self and other devices.*

### 3.2 Configure static IP address of Client 20 and Client 30 VM (both Windows 11)

- Right-click the network icon then click “Network and Internet settings”.

![Figure 7. Starting point to configure static IP address of Windows 11.](./screenshots/2%20configure%20ip%20of%20client%2020%20and%2030/1%20network%20of%20win11.jpg)

*Figure 7. Starting point to configure static IP address of Windows 11.*

- Click “Ethernet”.

![Figure 8. Ethernet connection.](./screenshots/2%20configure%20ip%20of%20client%2020%20and%2030/2%20ethernet.jpg)

*Figure 8. Ethernet connection.*

- Go to “Unidentified network”. On Ip assignment, click “Edit”.

![Figure 9 Continuation of Ethernet connection configuration.](./screenshots/2%20configure%20ip%20of%20client%2020%20and%2030/3%20ip%20assignment.jpg)

*Figure 9 Continuation of Ethernet connection configuration.*

- Choose “Manual” on Edit IP settings. Turn on “IPv4”. Enter following details:
  - Ip address: 192.168.0.12 (for Client 20) / 192.168.0.13 (for Client 30)
  - Subnet mask: 255.255.255.0
  - Gateway: 192.168.0.1
  - Preferred DNS: 8.8.8.8
- Click “Save”.

![Figure 10. Step to edit connection of Windows 11 and enter necessary details.](./screenshots/2%20configure%20ip%20of%20client%2020%20and%2030/4%20edit%20ip%20settings.jpg)

*Figure 10. Step to edit connection of Windows 11 and enter necessary details.*

- Windows 11 doesn’t automatically allow ping. So in order to ping Windows 11 device, some configurations are needed on Windows Defender Firewall with Advanced Security.
  - Click Windows icon, type "Windows Defender Firewall with Advanced Security", and press enter.
  - On left pane, click "Inbound Rules".
  - Right-click all "File and Printer Sharing (Echo Request - ICMPv4-In)" and click "Enable Rule".
  
  ![Figure 11. Configuration of firewall inbound rules.](./screenshots/2%20configure%20ip%20of%20client%2020%20and%2030/8%20firewall%20inbound%20rules.jpg)

  *Figure 11. Configuration of firewall inbound rules.*

  - Also, create a “New Rule”. On the right pane, click "New Rule".
  - Choose "Custom" and click "Next".
  - Choose "All programs" and click "Next".
  - On Protocol type, choose "ICMPv4" and click "Next".
  - Keep default and click Next.
  - Check "Allow the connection" and click Next.
  - All options must be checked and click Next.
  - Put name (Allow ICMP) and click "Finish".
  
  ![Figure 12. Firewall configuration to Allow ICMP.](./screenshots/2%20configure%20ip%20of%20client%2020%20and%2030/9%20firewall%20allow%20icmp.jpg)

  *Figure 12. Firewall configuration to Allow ICMP.*

- To verify the IP address, open CMD and type “ipconfig /all”.
  
![Figure 13. Static IP addresses are configured manually. IP address of Client 20 is 192.168.0.12. IP address of Client 30 is 192.168.0.13.](./screenshots/2%20configure%20ip%20of%20client%2020%20and%2030/5%20verify%20ip.jpg)

*Figure 13. Static IP addresses are configured manually. IP address of Client 20 is 192.168.0.12. IP address of Client 30 is 192.168.0.13.*

- Considering other client VMs are properly configured, to verify the connection, ping the device using its own IP address and IP address of another device.

![Figure 14. This is the result of ping from Client20 VM. Note that this ping result is similar for Client30 VM.](./screenshots/2%20configure%20ip%20of%20client%2020%20and%2030/7%20successful%20ping%20with%20devices.jpg)

*Figure 14. This is the result of ping from Client20 VM. Note that this ping result is similar for Client30 VM.*

### 3.3 Configure the Firewall with static IP address

- Start the firewall (FW01) as well as all other devices.
- Right click the firewall (FW01) and choose “Console”. You know firewall is done booting when you are able to see the serial number and the firewall login.
- Type “cisco” in the “Firewall login” and “Password”.
- Wait until you see “Welcome”.

![Figure 15. Initial step to open firewall in CML.](./screenshots/3%20configure%20firewall/1%20open%20firewall.jpg)

*Figure 15. Initial step to open firewall in CML.*

- Type “get system interface physical port1” to get information about port 1 that is connected to the internet. Open the IP address in another browser.

![Figure 16. Information of Port 1.](./screenshots/3%20configure%20firewall/2%20port1.jpg)

*Figure 16. Information of Port 1.*

- On the browser, click “Advanced”.
- Click “Proceed to 192.168.202.146(unsafe)”.
- Type “cisco” as the Username and Password, then click “Login”.
- Click "Login Read-Write".
- Click "Yes".
- Click "Begin".
- In the Dashboard Setup, choose the default which is "Optimal" and press "OK".
- Firewall (FW01) dashboard will open up.

![Figure 17. Firewall (FW01) dashboard.](./screenshots/3%20configure%20firewall/3%20firewall%20dashboard.jpg)

*Figure 17. Firewall (FW01) dashboard.*

- In the firewall (FW01) dashboard, click “Network”.
- Click “Interfaces”.
- Double click “port2” as this is the port connecting firewall to the switch.
- On “Edit Interface” window, enter the following details:
  - Alias: LAN
  - IP/Netmask: 192.168.0.1/24
  - On “IPv4”, check the “PING”
- Click OK.

![Figure 18. Edit the interface of firewall (FW01).](./screenshots/3%20configure%20firewall/4%20edit%20interface.jpg)

*Figure 18. Edit the interface of firewall (FW01).*

- From the client VM, ping the firewall/gateway IP (192.168.0.1) to check the connection.

![Figure 19. Successful ping of client machines to the firewall/gateway IP.](./screenshots/3%20configure%20firewall/7%20successful%20ping%20to%20gateway.jpg)

*Figure 19. Successful ping of client machines to the firewall/gateway IP.*

### 3.4 Configure the Firewall to be a DHCP Server (for dynamic IP addressing)

- With the same static IP address setup on port2, click to enable “DHCP Server”.
- On the “Address range”, leave an IP address for other devices such as printer, fax machine, etc. Therefore, just put 192.168.0.50-192.168.0.200.
- Keep the “Default gateway” and “DNS server” as is.
- Click OK.

![Figure 20. Firewall with DHCP Server enabled.](./screenshots/3%20configure%20firewall/5%20firewall%20with%20dhcp%20server.jpg)

*Figure 20. Firewall with DHCP Server enabled.*

- On the client machines, type the command “ipconfig /renew” to request a new IP address from a DHCP server. This will take some time to process. Or do the other way which is faster. Go to >Control Panel >Network and Internet >Network and Sharing Center >Change Adapter Settings >Right click Ethernet to disable >Right click Ethernet to enable.
- At this moment, client VMs must be taking dynamic IP addresses. For Kali Linux, type “ifconfig” in the terminal. For Client20 and Client30, type “ipconfig” in the CMD.

![Figure 21. Dynamic IP of Kali Linux is 192.168.0.52. Dynamic IP of Client20 is 192.168.0.51. Dynamic IP of Client30 is 192.168.0.50.](./screenshots/3%20configure%20firewall/6%20dhcp%20ip%20address.jpg)

*Figure 21. Dynamic IP of Kali Linux is 192.168.0.52. Dynamic IP of Client20 is 192.168.0.51. Dynamic IP of Client30 is 192.168.0.50.*

### 3.5 NAT Configuration

Network Address Translation (NAT) is a networking technique used to modify network address information in packet headers while in transit through a router or firewall. It helps convert private IP addresses used within a local network into public IP addresses, allowing multiple devices to share a single public IP address when accessing the internet. NAT also works in reverse, translating public IP addresses back to private ones for incoming traffic. This helps save IPv4 addresses and adds a layer of security by hiding internal networks.

There are three types of NAT: static NAT for one-to-one mapping where private IP address maps to a specific public IP address, dynamic NAT for flexible IP allocation which maps multiple private IP addresses to a pool of public IP addresses from a limited range, and PAT (Port Address Translation) or NAT Overload to let multiple private IP devices share one public IP. Proper NAT setup requires defining inside and outside interfaces, configuring address pools, and ensuring traffic flows correctly.

Currently, VMs are not connected to the internet. When VMs are trying to access the internet, it will give a response to an access error.

![Figure 22. VMs can't access any website.](./screenshots/9%20nat%20configuration/1%20unbale%20to%20connect%20google.jpg)

*Figure 22. VMs can't access any website.*

The access error is mainly because NAT is not yet configured. So, for simplicity’s sake, PAT or NAT Overload will be used to configure the NAT in firewall (FW01) GUI. Follow below steps:

- Open firewall (FW01) GUI.
- Go to “Policy and Objects” and click “Firewall Policy”.
- Click “Create new”.
- Name: Provide a descriptive Name (CML - VMs to Internet).
- Set “Incoming interface” to your internal LAN/VLAN interface (VLAN10, VLAN20, VLAN30).
- Set “Outgoing Interface” to your internet-facing WAN interface (port1).
- Set “Source” to the address or address group representing your CML VMs' subnet (all (IPv4)).
- Set “Destination” to “all”.
- Set “Schedule” to “always”.
- Set Service to “ALL”.
- Ensure “Action” is set to “ACCEPT”.
- Enable “NAT”.
- Set “Log allowed traffic” to “All sessions” to monitor traffic and verify the configuration.
- Click “OK” to save the policy.

![Figure 23. This is NAT configuration.](./screenshots/9%20nat%20configuration/2%20nat%20configuration.jpg)

*Figure 23. This is NAT configuration.*

![Figure 24. Lists of Firewall Policy.](./screenshots/9%20nat%20configuration/3%20firewall%20policy.jpg)

*Figure 24. Lists of Firewall Policy.*

- If everything is properly set, VMs are now able to access the internet.

![Figure 25. VLAN10 can access google or any website now after NAT configuration.](./screenshots/9%20nat%20configuration/4%20internet%20accessible%20in%20vlan10.jpg)

*Figure 25. VLAN10 can access google or any website now after NAT configuration.*

![Figure 26. VLAN20 can access google or any website now after NAT configuration.](./screenshots/9%20nat%20configuration/5%20internet%20is%20accessible%20in%20VLAN20.jpg)

*Figure 26. VLAN20 can access google or any website now after NAT configuration.*

![Figure 27. VLAN30 can access google or any website now after NAT configuration.](./screenshots/9%20nat%20configuration/6%20internet%20is%20accessible%20in%20VLAN30.jpg)

*Figure 27. VLAN30 can access google or any website now after NAT configuration.*

## 4 Vulnerabilities and potential impact

### 4.1 Network reconnaissance using Zenmap

Zenmap is the graphical user interface (GUI) for Nmap that provides a user-friendly interface for tasks like discovering active devices/hosts, identifying open ports, and finding vulnerabilities without requiring extensive command-line knowledge. It is a powerful tool in the context of the network and transport layers, as they are reconnaissance techniques used as critical initial steps to a more malicious activity. It also helps in identifying the operating systems and application/service versions that can be exploited in the later stages of an attack.

As a result of the network scan with zenmap, the information gathered will be used to plan and execute more damaging actions like denial-of-service (DOS) attacks, data breaches, etc. For example, an attacker might discover an open port for a specific version of a service and then use a known exploit for that vulnerability.

### 4.2 ICMP Ping Flood Attack

ICMP ping flood attack is a form of Distributed Denial-of-Service (DDoS) attack in which an attacker floods the recipient device by overwhelming it with ICMP echo requests, also known as pings. Essentially, Internet Control Message Protocol (ICMP) ping requests are used to check for connectivity and the health of networking devices. In a legitimate ICMP ping, the recipient device replies to an ICMP echo request. The response indicates the health of the recipient.

The goal of ICMP ping flood attack is to consume the target’s network bandwidth and/or processing capacity so legitimate traffic or services are disrupted or slowed. These attacks can 24
severely disrupt an organization’s online network operations, compromise the security of the cloud or local infrastructure, and make services unavailable to legitimate users. The resulting service disruptions and outages can significantly impact businesses, particularly those that rely heavily on online services.

## 5 Test Results (before and after scenarios)

### 5.1a Before the actual cyber-attack, perform network reconnaissance using Zenmap

- Click Kali Linux icon and type “zenmap”.
- Type “cisco” as the password and click “Authenticate” to open up zenmap.

![Figure 28. Zenmap is a tool used to do network reconnaissance.](./screenshots/4%20layer%203%20attack%20using%20zenmap/1%20zenmap.jpg)

*Figure 28. Zenmap is a tool used to do network reconnaissance.*

- On a real cyber-attack scenario, once you are connected to a network, you will get an idea of what range of IP addresses to scan. For this purpose, put IP address network range which is “192.168.0.0-250” on “Target”.
- On “Profile”, choose “Quick Scan”.
- Click “Scan” to quickly see what is in the network, figure out more information of what hosts are active, and from there, it is possibly the launch of further attack.

![Figure 29. Result of Zenmap scan where 3 active hosts were found.](./screenshots/4%20layer%203%20attack%20using%20zenmap/2%20zenmap%20scan%20result.jpg)

*Figure 29. Result of Zenmap scan where 3 active hosts were found.*

### 5.1b After the successful network reconnaissance using Zenmap

- From those active hosts, choose one IP address to scan in order to get more information. This time, choose “Intense Attack” on Profile and click Scan.

![Figure 30. Result of Zenmap scan for specific host, which is a successful cyber-attack.](./screenshots/4%20layer%203%20attack%20using%20zenmap/3%20zenmap%20scan%20result%20for%20specific%20host.jpg)

*Figure 30. Result of Zenmap scan for specific host, which is a successful cyber-attack.*

- At this point in time, the attacker achieves the ultimate objective of network reconnaissance which is to gather more information about a target’s network and plan an effective strategy for future cyber-attack.

### 5.2a Before ICMP Ping Flood Attack

In this attack, FortiGate firewall (FW01) is the target, and Kali Linux is the attacker. Kali Linux will basically send hundreds of packets to flood FortiGate firewall.

![Figure 31. Visual representation of ICMP Ping flood attack.](./screenshots/7%20icmp%20ping%20flood%20attack/1%20attacker%20and%20target.jpg)

*Figure 31. Visual representation of ICMP Ping flood attack.*

- Open the terminal in Kali Linux.
- Type “arp -a” to display the gateway of VLAN10 in FortiGate firewall (FW01).
- Ping the gateway to check the connection.

![Figure 32. Kali Linux can communicate on 192.168.10.1 which is the gateway of VLAN10 in FortiGate firewall.](./screenshots/7%20icmp%20ping%20flood%20attack/2%20kali%20pinging%20gateway%20of%20firewall.jpg)

*Figure 32. Kali Linux can communicate on 192.168.10.1 which is the gateway of VLAN10 in FortiGate firewall.*

- Verify the gateway of VLAN10 in FortiGate firewall (FW01) GUI.

![Figure 33. This is the gateway of VLAN10 in FortiGate firewall.](./screenshots/7%20icmp%20ping%20flood%20attack/3%20verify%20gateway%20of%20FortiGate%20firewall.jpg)

*Figure 33. This is the gateway of VLAN10 in FortiGate firewall.*

- Still in firewall (FW01) GUI, click “Dashboard” then click “Status” to monitor the current CPU usage.

![Figure 34. Before the ICMP ping flood attack, the CPU usage of the firewall is currently ranging from 5%.](./screenshots/7%20icmp%20ping%20flood%20attack/4%20cpu%20usage%20before%20attack.jpg)

*Figure 34. Before the ICMP ping flood attack, the CPU usage of the firewall is currently ranging from 5%.*

### 5.2b After ICMP Ping Flood Attack

- To perform the ICMP ping flood attack on the firewall, go to Kali Linux terminal and type “sudo ping -f -i 0.01 192.168.10.1” and press enter. Where “-f” is flood, “-i 0.01” is the 10-millisecond time interval, and 192.168.10.1 is the gateway of VLAN10 in FortiGate.

![Figure 35. Command for ICMP ping flood attack.](./screenshots/8%20icmp%20ping%20flood%20after/1%20command%20for%20icmp%20ping%20attack.jpg)

*Figure 35. Command for ICMP ping flood attack.*

- While running the ICMP ping flood attack, monitor the packet capture in firewall (FW01) GUI:
  - Click “Network”.
  - Click “Diagnostics”.
  - Click “New packet capture” to add new one.
  - On “Interface”, choose the VLAN where you want to capture the packets. In this case, choose HR (VLAN10).
  - Put a name.
  - On “Maximum captured packets”, put 1000 or depends on the requirement.
  - Finally, click “Start Capture”.

  ![Figure 36. This is monitoring tool for ICMP ping flood attack. As you can see, it is currently
capturing 590 packets, and it will keep on adding up.](./screenshots/8%20icmp%20ping%20flood%20after/2%20monitor%20vlan10.jpg)

  *Figure 36. This is monitoring tool for ICMP ping flood attack. As you can see, it is currently
capturing 590 packets, and it will keep on adding up.*

- Check the status of CPU current usage.

![Figure 37. While ICM ping flood attack is on-going, there is a spike in CPU usage. As seen here,
the current usage is 41%.](./screenshots/8%20icmp%20ping%20flood%20after/3%20cpu%20usage%20after%20attack.jpg)

*Figure 37. While ICM ping flood attack is on-going, there is a spike in CPU usage. As seen here,
the current usage is 41%.*

- As an alternative in checking the packet capture:
  - Go to CML.
  - Right click on the connection between switch and firewall.
  - Click “Packet Capture”.
  
  ![Figure 38. Another way in checking packet capture while running the ICMP ping flood attack.](./screenshots/8%20icmp%20ping%20flood%20after/4%20monitor%20vlan%20in%20cml%20itself.jpg)

  *Figure 38. Another way in checking packet capture while running the ICMP ping flood attack.*

## 6 Prevention and Mitigation

### 6.1 Implementation of VLAN as protection to network reconnaissance using Zenmap

Virtual Local Area Network (VLAN) is a logical segmentation that enables devices to be grouped together regardless of their physical location, isolating the traffic for each group. By partitioning a single physical network into multiple broadcast domains, VLANs improve security,
performance, flexibility, and manageability.

To prevent and mitigate from getting information of the entire network via Zenmap, the solution is to configure VLAN so that every device will be grouped and cannot be on the same network. VLAN configuration is done both on the switch (6.1.1) and on the firewall (6.1.2).

#### 6.1.1 VLAN configuration in switch

- On the switch console, type “enable”.
- Type “config” to configure the switch.
- Press enter when ask “Configure from terminal?”.
- Inside the switch config, type “vlan 10” and press enter.
- Type “name HR” to put a name on vlan 10 and press enter.
- Add another vlan which is “vlan 20” and press enter.
- Type “name IT” and press enter.
- Another vlan which is “vlan 30” and press enter.
- Type “name Finance” and press enter.

![Figure 39. VLAN configuration in the switch.](./screenshots/5%20mitigation%20via%20vlan/1%20vlan%20setup.jpg)

*Figure 39. VLAN configuration in the switch.*

- Type “exit” and press enter.
- Type “exit” and press enter.
- Type “show vlan” to show lists of existing vlan.

![Figure 40. Trunk port is used to connect switches and routers/firewall, transmitting data from multiple VLANs simultaneously. Access port connects virtual machines to a switch or VLAN, transmitting data within a single VLAN.](./screenshots/5%20mitigation%20via%20vlan/2%20vlan%20trunk%20and%20access%20port.jpg)

*Figure 40. Trunk port is used to connect switches and routers/firewall, transmitting data from
multiple VLANs simultaneously. Access port connects virtual machines to a switch or VLAN,
transmitting data within a single VLAN.*

- Trunk port configuration (e0/0)
  - Type “configure terminal” and press enter.
  - Type “int e0/0” and press enter.
  - Type “switchport trunk encapsulation dot1q” and press enter.
  - Type “switchport mode trunk” and press enter.

  ![Figure 41. Commands in switch console to configure trunk port.](./screenshots/5%20mitigation%20via%20vlan/3%20trunk%20port%20config.jpg)

  *Figure 41. Commands in switch console to configure trunk port.*

- Access port configuration (e0/1, e0/2, e0/3)
  - In the same switch configuration, type “int e0/1” and press enter.
  - Type “switchport mode access” and press enter.
  - Type “switchport access vlan 10” and press enter.

  ![Figure 42. Commands in switch console to configure access port of e0/1.](./screenshots/5%20mitigation%20via%20vlan/4%20e01%20access%20port%20config.jpg)

  *Figure 42. Commands in switch console to configure access port of e0/1.*

  - For e0/2, type “int e0/2” and press enter.
  - Type “switchport mode access” and press enter.
  - Type “switchport access vlan 20” and press enter.

  ![Figure 43. Commands in switch console to configure access port of e0/2.](./screenshots/5%20mitigation%20via%20vlan/5%20e02%20access%20port%20config.jpg)

  *Figure 43. Commands in switch console to configure access port of e0/2.*

  - For e0/3, type “int e0/3” and press enter.
  - Type “switchport mode access” and press enter.
  - Type “switchport access vlan 30” and press enter.

  ![Figure 44. Commands in switch console to configure access port of e0/3.](./screenshots/5%20mitigation%20via%20vlan/6%20e03%20access%20port%20config.jpg)

  *Figure 44. Commands in switch console to configure access port of e0/3.*

  - Type “exit” and press enter.
  - Type “exit” and press enter.
  - Type “show vlan” and press enter to show lists of vlan.

![Figure 45. Show lists of VLAN after the configuration.](./screenshots/5%20mitigation%20via%20vlan/7%20show%20vlan%20after%20config.jpg)

*Figure 45. Show lists of VLAN after the configuration.*

- Finally, type “write” and press enter to save the configuration made.

#### 6.1.2 VLAN configuration in firewall

- Go back to the firewall (FW01) GUI and click “Network” then “Interfaces”. Go specifically on port 2. On “IP/Netmask”, remove the existing IP and replace it with “0.0.0.0/0”. Disable “DHCP Server” as well. Press OK. This is like going back to the basic LAN.
- Next step is to create 3 different virtual interfaces under port 2 that talk to each VLANs created in switch. On the left panel of firewall (FW01) GUI, go to “Network” and click “Interfaces”.
- Click “Create New” and click on “Interface”.
- On “New Interface” window, put “VLAN10” under “Name” (others are VLAN20 and
VLAN30).
- “Alias” is “HR” (IT for VLAN20; Finance for VLAN30).
- “Type” is “VLAN”.
- “VLAN protocol” is “802.1Q”.
- In “Interface”, choose “LAN(port2)”.
- “VLAN ID” is “10” (“20” for VLAN20; "30” for VLAN30).
- Click “Manual” in the “Addressing mode”.
- “IP/Netmask” is “192.168.10.1/24” (192.168.20.1/24 for VLAN20; 192.168.30.1/24 for
VLAN30).
- Check “PING” on IPv4.
- Enable “DHCP Server”.
- Put “192.168.10.50-192.168.10.200” in the “Address range” (192.168.20.50-
192.168.20.200 for VLAN20; 192.168.30.50-192.168.30.200 for VLAN30).
- Click OK.
- Repeat the process for VLAN20 and VLAN30.

![Figure 46. VLAN 10 (HR) configuration.](./screenshots/6%20firewall%20config%20with%20vlan%20switch/1a%20new%20interface%20vlan%2010.jpg)

*Figure 46. VLAN 10 (HR) configuration.*

![Figure 47. VLAN 20 (IT) configuration.](./screenshots/6%20firewall%20config%20with%20vlan%20switch/2%20new%20interface%20vlan%2020.jpg)

*Figure 47. VLAN 20 (IT) configuration.*

![Figure 48. VLAN 30 (Finance) configuration.](./screenshots/6%20firewall%20config%20with%20vlan%20switch/3%20new%20interface%20vlan%2030.jpg)

*Figure 48. VLAN 30 (Finance) configuration.*

![Figure 49. These are the 3 VLANs created.](./screenshots/6%20firewall%20config%20with%20vlan%20switch/4%203%20vlans%20created.jpg)

*Figure 49. These are the 3 VLANs created.*

- Verify if client machines have the VLAN configured.

![Figure 50. The IP address of VLAN30 is 192.168.30.50](./screenshots/6%20firewall%20config%20with%20vlan%20switch/5%20verify%20vlan30%20correct%20IP.jpg)

*Figure 50. The IP address of VLAN30 is 192.168.30.50*

![Figure 51. The IP address of VLAN20 is 192.168.20.50](./screenshots/6%20firewall%20config%20with%20vlan%20switch/6%20verify%20vlan20%20correct%20IP.jpg)

*Figure 51. The IP address of VLAN20 is 192.168.20.50*

![Figure 52. The IP address of VLAN10 is 192.168.10.50](./screenshots/6%20firewall%20config%20with%20vlan%20switch/7%20verify%20vlan10%20correct%20IP.jpg)

*Figure 52. The IP address of VLAN10 is 192.168.10.50*

- At this point, VLAN is now in place. It means VLAN10, VLAN20, and VLAN30 cannot communicate with each other anymore.

![Figure 53. No response from other VLAN means that the network is properly segmented/isolated.](./screenshots/6%20firewall%20config%20with%20vlan%20switch/8%20no%20response%20from%20other%20vlan.jpg)

*Figure 53. No response from other VLAN means that the network is properly segmented/isolated.*

### 6.2 Implementation to prevent and mitigate ICMP ping flood attack

- Go to firewall (FW01) GUI.
- Click “Policy and Objects” and “DoS Policy”.
- Click “Create new”.
- Put a descriptive “Name” (Block ICMP flood VLAN10, etc).
- On “Incoming Interface”, choose the VLAN (VLAN10, VLAN20, VLAN30).
- “Source Address” is “all”.
- “Destination Address” is “all”.
- “Service” is “ALL”.

![Figure 54. Initial steps for ICMP ping flood mitigation](./screenshots/10%20icmp%20mitigation/1%20first%20step%20in%20ICMP%20ping%20flood%20mitigation.jpg)

*Figure 54. Initial steps for ICMP ping flood mitigation*

- Scroll down and look for “icmp_flood”. Click “Block” button.
- Press OK.

![Figure 55. Setting up DoS policy for each VLAN.](./screenshots/10%20icmp%20mitigation/2%20icmp%20flood%20mitigation%20for%20vlan.jpg)

*Figure 55. Setting up DoS policy for each VLAN.*

- Repeat the process for VLAN20 and VLAN30.

![Figure 56. Lists of DoS Policy to block ICMP ping flood for VLAN10, VLAN20, and VLAN30.](./screenshots/10%20icmp%20mitigation/3%20Lists%20of%20DoS%20Policy.jpg)

*Figure 56. Lists of DoS Policy to block ICMP ping flood for VLAN10, VLAN20, and VLAN30.*

- While running the ICMP ping flood attack in Kali Linux terminal (sudo ping -f -i 0.01 192.168.10.1), verify the current CPU Usage by going to the “Dashboard” and “Status”.

![Figure 57. The current CPU usage is 0% after the implementation of Block ICMP flood in DoS Policy. This means that whenever ICMP ping exceed the threshold set in DoS policy, the DoS policy will block it.](./screenshots/10%20icmp%20mitigation/4%20cpu%20usage%20after%20mitigation.jpg)

*Figure 57. The current CPU usage is 0% after the implementation of Block ICMP flood in DoS Policy. This means that whenever ICMP ping exceed the threshold set in DoS policy, the DoS policy will block it.*

## References

[What is OSI Model?](https://www.realpars.com/blog/osi)

Difference Between Trunk Port and Access Port. (2025, July 23). Retrieved from Geeks for Geeks:
https://www.geeksforgeeks.org/computer-networks/difference-between-trunk-port-andaccess-
port/

Etheridge, E. (n.d.). Access Control List (ACL) in cyber security: Beneficial for all, critical for some.
Retrieved from DataGuard: https://www.dataguard.com/blog/acl-access-control-list/

Fortinet. (n.d.). Configure a Firewall Policy for a PC to Access Internet | FortiGate. Retrieved from
Youtube: https://www.youtube.com/watch?v=36wU22YqrGw

Hess, K. (n.d.). Scanning with Zenmap. Retrieved from Linux Magazine: https://www.linuxmagazine.
com/Online/Features/Scanning-with-Zenmap

Lutkevich, B. (2021, October 1). IP spoofing. Retrieved from TechTarget:
https://www.techtarget.com/searchsecurity/definition/IP-spoofing

NAT Explained: What It Is and Why It's Important in Networking. (n.d.). Retrieved from Nitiz Sharma:
https://nitizsharma.com/nat-explained-in-networking/

Operations, N. (n.d.). Deep Packet Analysis Ep4 - ICMP PING FLOOD + FortiGate Firewall Part 1. Retrieved
from YouTube: https://www.youtube.com/watch?v=vegnSkRNMJs

Operations, N. (n.d.). Deep Packet Analysis Ep5 - IPv4 DOS POLICY + FORTIGATE FIREWALL PART 2.
Retrieved from YouTube: https://www.youtube.com/watch?v=bYM3lWzqPwI&list=PL12zYfEwh5q_yDEcQyzSa87pxwZq2rrw&index=14

Raza, M. (2025, June 18). Deep Packet Inspection (DPI) Explained: OSI Layers, Real-World Applications &
Ethical Considerations. Retrieved from Splunk:
https://www.splunk.com/en_us/blog/learn/deep-packet-inspectiondpi.html#:~:text=What%20is%20deep%20packet%20inspection,might%20violate%20specific%20filtering%20rules.

Saroyan, K. (n.d.). What Is Access Control List (ACL) & Importance? Retrieved from Impanix:
https://impanix.com/security/access-control-listacl/#:~:text=and%20security%20requirements.-,What%20Are%20The%20Disadvantages,access%20control%20across%20interconnected%20systems.

What Is A Ping (ICMP) Flood DDOS Attack? (n.d.). Retrieved from radware:
https://www.radware.com/security/ddos-knowledge-center/ddospedia/icmpflood/#WhatIsAPingFloodAttack

What is a Ping Flood Attack? (n.d.). Retrieved from Akamai: https://www.akamai.com/glossary/what-isa-ping-flood-attack

What is a SYN flood attack? (n.d.). Retrieved from fastly: https://www.fastly.com/learning/bots/what-isa-syn-floodattack#:~:text=A%20SYN%20flood%20attack%20overwhelms,struggles%20with%20pending%2C%20incomplete%20connections.

What is an IP Address? (2025, October 7). Retrieved from GeeksforGeeks:
https://www.geeksforgeeks.org/computer-science-fundamentals/what-is-an-ip-address/
