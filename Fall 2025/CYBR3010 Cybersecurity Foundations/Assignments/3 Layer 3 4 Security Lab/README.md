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

This document is about Layer 3 and Layer 4 of the OSI Layer, which is the Network Layer (IP and ICMP are the common protocols) and Transport Layer (TCP and UDP are the common protocols). It is in Layer 3 where routing, packet forwarding, and logical addressing happens with the use of an IP address; hence this document will cover on how to configure IP addresses statically and dynamically through enabling Dynamic Host Control Protocol (DHCP) on a firewall. For Layer 4, Transport layer ensures the reliable and efficient data delivery through segmentation; hence this document will cover the configuration of VLAN on the firewall and on the switch to segment the network for better traffic control and security. Moreover, this document will go through the configuration of NAT to allow VMs to access the internet. Finally, network layer and transport layer are prone to cyberattacks. Therefore, this document will tackle the vulnerabilities and potential impact associated with this layer, compare results before and after the attack, and apply security measures to mitigate these vulnerabilities effectively.

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

### 4.2 ICMP Ping Flood Attack

## 5 Test Results (before and after scenarios)

### 5.1a Before the actual cyber-attack, perform network reconnaissance using Zenmap

### 5.1b After the successful network reconnaissance using Zenmap

### 5.2a Before ICMP Ping Flood Attack

### 5.2b After ICMP Ping Flood Attack

## 6 Prevention and Mitigation

### 6.1 Implementation of VLAN as protection to network reconnaissance using Zenmap

#### 6.1.1 VLAN configuration in switch

#### 6.1.2 VLAN configuration in firewall

### 6.2 Implementation to prevent and mitigate ICMP ping flood attack

## References

[What is OSI Model?](https://www.realpars.com/blog/osi)