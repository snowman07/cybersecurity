

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
- [References](#references)

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

*Table 1. Tabular representation of Virtual Machines with the corresponding MAC Address Table.*
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

- To launch an attack, type “sudo macof -1 eth0” in Kali Linux terminal and press enter.
- Type the password and press enter.
- Mac flooding will take effect at this moment of time. Several frames with different MAC addresses will be displayed continuously.

![Figure 4. After the MAC flooding attack, notice in Kali Linux terminal that all frames with different MAC addresses will keep showing up.](./screenshot/mac%20flooding%20after%20the%20attack%20in%20kali%20terminal.jpg)

*Figure 4. After the MAC flooding attack, notice in Kali Linux terminal that all frames with different MAC addresses will keep showing up.*

- In switch console, type “show mac address-table” then press enter.
- This will display the total entries of MAC addresses after the MAC flooding attack.

![Figure 5. After the MAC flooding attack, there are total of 313 MAC addresses in the MAC address table as opposed to 3 MAC addresses before the MAC flooding attack.](./screenshot/mac%20flooding%20after%20the%20attack%20in%20switch%20console.jpg)

*Figure 5. After the MAC flooding attack, there are total of 313 MAC addresses in the MAC address table as opposed to 3 MAC addresses before the MAC flooding attack.*

### 5.3 MAC spoofing before launching the attack in Kali Linux VM

- In CML, start both the switch and Kali Linux VM.
- Open the terminal in Kali Linux and run the command “ifconfig”. This command will check the MAC address of Kali Linux VM.

![Figure 6. Before MAC spoofing attack, MAC address of Kali Linux VM is 52:54:00:00:85:e3, which is the original MAC address.](./screenshot/mac%20spoofing%20before%20attack%20mac%20add%20in%20kali.jpg)

*Figure 6. Before MAC spoofing attack, MAC address of Kali Linux VM is 52:54:00:00:85:e3, which is the original MAC address.*

- On the switch, open the console and run the command “show mac address-table” to display the MAC address connected to the switch.

![Figure 7. The MAC address in switch is 5254.0000.85e3, which is the same and the original MAC address of Kali Linux VM.](./screenshot/mac%20spoofing%20before%20attack%20mac%20add%20in%20switch.jpg)

*Figure 7. The MAC address in switch is 5254.0000.85e3, which is the same and the original MAC address of Kali Linux VM.*

### 5.4 MAC spoofing after launching the attack in Kali Linux VM

- To launch the MAC spoofing attack, type “macchanger -h” in Kali Linux terminal and press enter. This is a command for help and to see all the available options.
- Type “sudo macchanger -s eth0”, press enter and enter the password. This command is to show the current and permanent MAC address of Kali Linux VM in interface eth0.

![Figure 8. Current MAC and Permanent MAC is the same which is 52:54:00:00:85:e3.](./screenshot/mac%20spoofing%20after%20attack1.jpg)

*Figure 8. Current MAC and Permanent MAC is the same which is 52:54:00:00:85:e3.*

- Type “sudo macchanger -r eth0” and press enter. This command is to set random MAC address.

![Figure 9. Random new MAC address (92:8b:cd:d2:0b:e9) has been generated after running the command for MAC spoofing attack.](./screenshot/mac%20spoofing%20after%20attack2.jpg)

*Figure 9. Random new MAC address (92:8b:cd:d2:0b:e9) has been generated after running the command for MAC spoofing attack.*

- Type “sudo macchanger -s eth0” and press enter. This command is to verify the new MAC address.

![Figure 10. The current MAC address is the new random MAC address 92:8b:cd:d2:0b:e9.](./screenshot/mac%20spoofing%20after%20attack3.jpg)

*Figure 10. The current MAC address is the new random MAC address 92:8b:cd:d2:0b:e9.*

- Another way to verify the new MAC address is by typing “ifconfig”.

![Figure 11. Command "ifconfig" is also showing the new random MAC address 92:8b:cd:d2:0b:e9 after MAC spoofing attack.](./screenshot/mac%20spoofing%20after%20attack5.jpg)

*Figure 11. Command "ifconfig" is also showing the new random MAC address 92:8b:cd:d2:0b:e9 after MAC spoofing attack.*

- Verify the MAC address table in the switch console. Type “show mac address-table”.

![Figure 12. In the switch, MAC address table is updated with the new random MAC address 92:8b:cd:d2:0b:e9 after MAC spoofing attack.](./screenshot/mac%20spoofing%20after%20attack4.jpg)

*Figure 12. In the switch, MAC address table is updated with the new random MAC address 92:8b:cd:d2:0b:e9 after MAC spoofing attack.*

Side Note: It is also possible to set a specific MAC address instead of random MAC address. Type the command “sudo macchanger –mac=11.00.22.00.33.00 eth0”.
Where:
- 11.00.22.00.33.00 is the specific MAC address and can change depending on preference.
- eth0 is the interface.

### 5.5 ARP Spoofing before launching the attack in Kali Linux VM

In this ARP Spoofing, Windows 10 VM and Windows 11 VM are communicating with each other. However, Windows 11 VM will be the target by telling that the MAC address of Windows 10 VM has changed, when in fact it is the MAC address of the attacker in Kali Linux VM.

![Figure 13. Visual diagram of how ARP spoofing is in action.](./screenshot/arp%20spoofing%202.jpg)

*Figure 13. Visual diagram of how ARP spoofing is in action.*

- In CML, turn on all the VM’s and the switch.
- Since Windows 11 will be the target, open Windows Powershell and run as admin.
- In the Windows Powershell, type “arp -a” and press enter.

![Figure 14. Before the ARP spoofing attack, Kali Linux VM and Windows 10 VM has its own MAC address. Kali Linux VM MAC address is 52-54-00-00-85-e3, while Windows 10 VM MAC address is 52-54-00-86-0f-ff.](./screenshot/arp%20spoofing%201.jpg)

*Figure 14. Before the ARP spoofing attack, Kali Linux VM and Windows 10 VM has its own MAC address. Kali Linux VM MAC address is 52-54-00-00-85-e3, while Windows 10 VM MAC address is 52-54-00-86-0f-ff.*

### 5.6 ARP Spoofing after launching the attack in Kali Linux VM

- To launch the ARP Spoofing attack in Kali Linux terminal, check the port forwarding first. Type “sysctl net.ipv4.ip_forward”.
- If you see 0, it means it is disabled. To enable port forwarding, type “sudo sysctl -w net.ipv4.ip_forward=1”, enter the root password, then press enter.
- To check port forwarding, type again “sysctl net.ipv4.ip_forward” and press enter. By this time, the value should be 1.

![Figure 15. In order to launch ARP Spoofing attack, it is recommended to check and set port forwarding first. In here, port forwarding is set to 1 and is now enabled.](./screenshot/arp%20spoofing%203.jpg)

*Figure 15. In order to launch ARP Spoofing attack, it is recommended to check and set port forwarding first. In here, port forwarding is set to 1 and is now enabled.*

- Please note that ARP spoofing is done by superuser. So, type “sudo -s”, type the password, and press enter.
- Type “arpspoof -i eth0 -t 192.168.202.142 -r 192.168.202.144” and press enter. By this time, Kali Linux VM will start sending ARP replies to the target Windows 11 VM telling that the MAC address of Windows 10 VM’s IP address 192.168.202.144 has changed.

![Figure 16. Attacker is in control.](./screenshot/arp%20spoofing%204a.jpg)

*Figure 16. Attacker is in control.*

- Go to the target Windows 11 VM. Type “arp -a” and press enter. Notice that the MAC address of Windows 10 VM has change to that of Kali’s MAC address.

![Figure 17. MAC address of Windows 10 VM is now similar with the attacker's MAC address.](./screenshot/arp%20spoofing%205.jpg)

*Figure 17. MAC address of Windows 10 VM is now similar with the attacker's MAC address.*

- From the target Windows 11 VM, try to ping 8.8.8.8. After ARP spoofing attack, check the traffic connection from Kali Linux VM to switch. Notice that connection is getting ARP responses.

![Figure 18. Checking the traffic of Kali Linux VM connected to switch.](./screenshot/arp%20spoofing%206.jpg)

*Figure 18. Checking the traffic of Kali Linux VM connected to switch.*

## 6 Prevention and Mitigation

### 6.1 MAC flooding attack – security measures

To prevent MAC flooding attack, use Port Security. Below are the main commands that would limit MAC addresses and block unknown devices in MAC flooding attack:
- Make sure you are in SW01#, otherwise type “enable” and press enter.
- Type “configure terminal” and press enter.
- In the switch configuration, type “interface range e0/1-3”and press enter. This command means going to interfaces range from e0/1, e0/2, and e0/3.
- Type “switchport mode access” and press enter.
- Type “switchport port-security” and press enter.
- Type “switchport port-security mac-address sticky” and press enter.
- Type “switchport port-security maximum 2” and press enter.
- For violation, we will use the default option which is to shutdown. As an alternative, type “switchport port-security violation restrict”.

![Figure 19. Main commands to configure security measures of Mac flooding attack.](./screenshot/mac%20flooding%20security%20measure%20main%20command.jpg)

*Figure 19. Main commands to configure security measures of Mac flooding attack.*

Below are additional helpful commands:
- If you type “switchport port-security ?”, this will display available configuration options.

![Figure 20. This command is to control which MAC addresses are allowed on a port and what happens when an unauthorized device attempts to connect.](./screenshot/mac%20flooding%20security%20measure%20helpful%20command%201.jpg)

*Figure 20. This command is to control which MAC addresses are allowed on a port and what happens when an unauthorized device attempts to connect.*

> Aging is an option to set the time for how long a secure MAC address remains in the switch's table before it's removed.

> Mac-address allows you to statically define specific MAC addresses that are permitted on the port.

> Maximum sets the highest number of unique MAC addresses allowed on that specific port.

> Violation determines the switch's response when the maximum number of secure MAC addresses is exceeded, or a violation occurs.

- If you type “switchport port-security mac-address ?”, you will see configuration options for mac-address.

![Figure 21. For MAC flooding security measures, we are only concerned about “sticky”. Sticky feature automatically learns and saves these MAC addresses.](./screenshot/mac%20flooding%20security%20measure%20helpful%20command%202.jpg)

*Figure 21. For MAC flooding security measures, we are only concerned about “sticky”. Sticky feature automatically learns and saves these MAC addresses.*

- If you type “switchport port-security violation ?”, you will see configuration options for violation.

![Figure 22. A switchport port-security violation is an event where a network switch's port security feature detects an unauthorized device or action, such as a MAC address that exceeds the configured limit for a port.](./screenshot/mac%20flooding%20security%20measure%20helpful%20command%203.jpg)

*Figure 22. A switchport port-security violation is an event where a network switch's port security feature detects an unauthorized device or action, such as a MAC address that exceeds the configured limit for a port.*

When a violation occurs, the switch takes a specific action based on the configured violation mode, which can be to disable the port and requires network admin to manually re-enable it (shutdown, this is the default option), block the violating traffic while logging the event (restrict), or silently drop the violating traffic without logging (protect).

- To check what is going on after the implementation of port security, exit the configuration and type “show port-security”.

![Figure 23. Checking after Port Security implementation.](./screenshot/mac%20flooding%20security%20measure%20helpful%20command%204.jpg)

*Figure 23. Checking after Port Security implementation.*

- To show the status of all the interfaces, type “show ip interface brief”.

![Figure 24. Right now, the status of all the interfaces is up because MAC flood is not launched yet.](./screenshot/mac%20flooding%20security%20measure%20helpful%20command%205.jpg)

*Figure 24. Right now, the status of all the interfaces is up because MAC flood is not launched yet.*

![Figure 25. In this screenshot, MAC flooding is in place. Since port security is implemented, status of interface e0/1 is set to “down”. Therefore, MAC flooding attack is unsuccessful](./screenshot/mac%20flooding%20security%20measure%20helpful%20command%206.jpg)

*Figure 25. In this screenshot, MAC flooding is in place. Since port security is implemented, status of interface e0/1 is set to “down”. Therefore, MAC flooding attack is unsuccessful*

- To return the affected interface (e0/1) from “down” status to “up” status
  - Type “config terminal” and press enter.
  - Type “int e0/1” and press enter.
  - Type “shutdown” and press enter.
  - Type “no shutdown” and press enter.
  - Type “exit” and press enter (or ctrl+Z).
  - Type “exit” and press enter (or ctrl+Z) again.
  - Lastly, type “show ip interface brief” and press enter.

![Figure 26. Affected interface e0/1 is now back up after running the command.](./screenshot/mac%20flooding%20security%20measure%20helpful%20command%207.jpg)

*Figure 26. Affected interface e0/1 is now back up after running the command.*

### 6.2 MAC spoofing attack – security measures

The security measure for MAC spoofing is by using port-security, which is the same security measure for MAC flooding.

- Make sure you are in SW01#, otherwise type “enable” and press enter.
- Type “configure terminal” and press enter.
- In the switch configuration, type “interface range e0/1-3”and press enter. This command means going to interfaces range from e0/1, e0/2, and e0/3.
- Type “switchport mode access” and press enter.
- Type “switchport port-security” and press enter.
- Type “switchport port-security mac-address sticky” and press enter.
- Type “switchport port-security maximum 2” and press enter.
- For violation, we will use the default option which is to shutdown. As an alternative, type “switchport port-security violation restrict”.

![Figure 27. Commands to configure security measures of Mac spoofing attack.](./screenshot/mac%20flooding%20security%20measure%20main%20command.jpg)

*Figure 27. Commands to configure security measures of Mac spoofing attack.*

- Revert to the permanent (hardware) MAC address:
  - Type “sudo ip link set dev eth0 down” and press enter.
  - Type “sudo macchanger -p eth0” and press enter.
  - Type “sudo ip link set dev eth0 up” and press enter.
  - To verify, type “sudo macchanger -s eth0” and press enter.
  - Another way to verify is to type “ifconfig”.

![Figure 28. MAC address of Kali Linux VM is back from its original MAC address which is 52:54:00:00:85:e3.](./screenshot/mac%20spoofing%20after%20attack6.jpg)

*Figure 28. MAC address of Kali Linux VM is back from its original MAC address which is 52:54:00:00:85:e3.*

### 6.3 ARP Spoofing attack – security measure

To prevent ARP spoofing attack, use Dynamic ARP Inspection (DAI) together with DHCP Snooping. This security measure ensures that only legitimate ARP replies are accepted.
- Go to switch console configuration.
- Type “ip dhcp snooping” and press enter. This is the command to turn on DHCP globally.
- Type “ip dhcp snooping vlan 1” and press enter. This is the command to turn DHCP on specific Vlan.
- Type “ip arp inspection vlan 1” and press enter.

![Figure 29. Implementation of DHCP Snooping to prevent ARP spoofing attack.](./screenshot/arp%20spoofing%207.jpg)

*Figure 29. Implementation of DHCP Snooping to prevent ARP spoofing attack.*

- Type “interface E0/1” and press enter to go to the interface configuration.
- Type “ip dhcp snooping trust” and press enter.
- To verify, type “show ip arp inspection statistics” and press enter.

![Figure 30. After the implementation of Dynamic ARP Inspection (DAI) and DHCP Snooping to prevent ARP spoofing, statistics show that it dropped 2827 invalid ARPs.](./screenshot/arp%20spoofing%208%20-%20arp%20stat.jpg)

*Figure 30. After the implementation of Dynamic ARP Inspection (DAI) and DHCP Snooping to prevent ARP spoofing, statistics show that it dropped 2827 invalid ARPs.*


## References

Application Security Knowledge Base. (n.d.). Retrieved from Veracode:
https://www.veracode.com/security/arp-spoofing/

CCNADailyTIPS. (2019, June 23). Kali Linux ARP Poisoning/Spoofing Attack. Retrieved from Youtube:
https://www.youtube.com/watch?v=mchrDyBdMmc

Introduction to Spanning Tree Protocol | CCNA 200-301. (n.d.). Retrieved from Youtube:
https://www.youtube.com/watch?v=EFw_ZNdj-w8

Kali Linux ARP Spoofing. (n.d.). Retrieved from Notes:
https://notes.shanakadesoysa.com/Cyber_Security/ARP_Spoofing/#kali-linux-arp-spoofing

MAC Spoofing Attacks Explained: A Technical Overview. (2024, September 26). Retrieved from
SecureW2: https://www.securew2.com/blog/how-do-mac-spoofing-attacks-work

macof(8) - Linux man page. (n.d.). Retrieved from Linux Die.Net: https://linux.die.net/man/8/macof

Man-in-the-middle attack | ARP Spoofing & 07 step Procedure! (n.d.). Retrieved from Cybervie:
https://cybervie.com/man-in-the-middle-attack/

Port Security – Lesson and Lab. (n.d.). Retrieved from How To Network Making IT Easy:
https://www.howtonetwork.com/technical/security-technical/port-security/

Šlekytė, I. (2024, July 3). What is a MAC spoofing attack? Learn how it works and how to detect and
prevent it. Retrieved from NordVPN: https://nordvpn.com/blog/macspoofing/?
srsltid=AfmBOorLEvGNJgogN--

Spanning Tree Protocol Attacks: 3 Attacks. (n.d.). Retrieved from ProSec: https://www.prosecnetworks.
com/en/blog/spanning-tree-protokoll-angriffe-3-attacks-undschutzmassnahmen/#:~:
text=Penetration%20Tester%20course-
,Spanning%20Tree%20Protocol%20Attack%202:%20STP%20Denial%20of%20Service,process%2
0we%20choose%20option%202.

What Is a MAC Address Table? (2025, February 14). Retrieved from JumpCloud:
https://jumpcloud.com/it-index/what-is-a-mac-address-table

What Is a MAC Flooding Attack? (2025, May 21). Retrieved from JumpCloud: https://jumpcloud.com/itindex/
what-is-a-mac-flooding-attack

What is MAC Address Table ? (n.d.). Retrieved from GeekforGeek:
https://www.geeksforgeeks.org/computer-networks/what-is-mac-address-table/

What is MAC Spoofing? How It Works & Examples. (2024, August 1). Retrieved from Twingate:
https://www.twingate.com/blog/glossary/mac%20spoofing

What is STP (Spanning Tree Protocol)? (n.d.). Retrieved from Youtube:
https://www.youtube.com/watch?v=i_q-kIgz9Wk
