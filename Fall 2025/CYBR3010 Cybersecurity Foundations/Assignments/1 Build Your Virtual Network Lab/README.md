# Virtual Lab Build - CML (Cisco Modelling Lab)

## Objectives
Virtual labs refer to labs that are based on virtual machines and are designed to provide hands-on cybersecurity training through simulated real-world environments. They offer students the opportunity to practice different aspects of cybersecurity and network building, with the added benefit of being able to save their work on a portal drive that can be accessed from the classroom or at home.

## Background
In this lab, students will work within a Cisco Modeling Lab (CML) environment.
Your task is to design, build, configure, and document a virtual lab network. This exercise will help you become familiar with the lab environment you’ll be using for the remainder of the course.
Include screen captures as evidence of your work.

## Instructions
Using a virtual firewall, a virtual switch, and virtual client machines, create and configure a virtual network with the following requirements:
<br>
1. `Network Design`
<ul> 
  <li>Use MS Visio (or any other diagramming tool) to design an interconnected network containing:</li>
  
    1 - 1x Virtual Firewall
    2 - 1x Virtual Network Switch<
    3 - 4× Virtual Machines, each running a different operating system (Kali Linux, Windows 10, Windows 11, Windows 7, Linux)
    

  <li>Build and interconnect your network on a virtual lab environment (CML)</li>

  <li>Turn on all devices and ensure all devices are up and running and ready for configuration</li>
</ul>

2 - `Lab Setup in CML`
<ul>
  <li>Build and interconnect your network in the CML virtual lab environment. </li>
  <li>Power on all devices and verify that they are running and ready for configuration. </li>
</ul>

## Deliverables
Submit a PDF document containing screenshots and explanations, including:
- `Network Diagram`: A Visio (or equivalent) diagram showing the final network layout.
- `Introduction`: A brief overview of your network build and its purpose.
- `Device Status Proof`: Screenshots confirming that each virtual device is operational.
- `Rebuild Guide`: A clear, step-by-step set of instructions that could be followed to recreate the system from scratch.


------

# Rebuild Guide

'Software Used'
- VMware Workstation Pro
- Cisco Modelling Lab (CML)
-


1. Install VMware Workstation Pro:
   - To get the installer, go to https://support.broadcom.com/
   - Click "Register"
   - Verification code will be sent in email. Continue filling-up the form until account is created.
   - Click "Login" to login to your account for the first time. `username` domingo.arrbelrey+broadcomcybersecurityrequirement@gmail.com `password` qwerty01A.
   - Once logged-in, you will see your dashboard.
   - On the left pane, click "My Downloads"
   - You can either search VMware Workstation Pro or where you see "Free Software Downloads", click "HERE"
   - Click "VMware Workstation Pro". You will then see an option to download different versions of Windows and Linux.
   - Click the version. Agree to the terms and conditon.
   - You can now download the installer and use the executable file to install VMware Workstation Pro in your machine.

2. Setup for CML and FortiManager 
  > `Student_CML_FortiManager2_OVA` folder is 1.26 GB <br>
  > `CML_Student_OVA` folder is 75.7 GB in total --> in video tut, this is Student_CML_Master

  - For this assignment, there are 2 folders needed. One for FortiManager and one for CML Student. Download both folders and unzip using 7zip.
  - Inside the folder, there are images where you need to run/click to open it in VMware Workstation Pro. 
    - For CML Student (main CML):
      - Click .ovf file (Open Virtualization Format) 
      - Enter a name of the new virtual machine. `CML01`
      - Browse for the storage path of the new virtual machine
      - Click "Import". This process will take some time to finish.
      - Once done, you will see VMware Workstation. Click "Edit virtual machine settings"
      - "Network Adapter" should be set to "NAT" (give the device internet access and IP address thru closed networks through desktop networks which will not exposed to the outside which is good)
      - "Network Adapter 2" should be set to "Custom: VMnet2" (initially set to Bridged Automatic which means its connected to actual network. Custom: VMNet2 is a closed environment meaning it will not be interfering with any other network outside.)
         ![CML01 - Network Adapter](C:\GitHub\cybersecurity\Fall 2025\CYBR3010 Cybersecurity Foundations\Assignments\1 Build Your Virtual Network Lab\screenshots)
      - Click OK
    - For CML FortiManager (required for firewall licensing):
      - Click .ovf file (Open Virtualization Format)
      - Enter a name of the new virtual machine. `FortiManager01`
      - Browse for the storage path of the new virtual machine
      - Click "Import"
      - Once done, you will see VMware Workstation. Click "Edit virtual machine settings"
      - "Network Adapter" should be set to "Custom: VMnet2". Also check "Connect at power on" under Device status.
         ![CML01 - Network Adapter](C:\GitHub\cybersecurity\Fall 2025\CYBR3010 Cybersecurity Foundations\Assignments\1 Build Your Virtual Network Lab\screenshots)
      - Click OK


3. Power on the new VM
   - For FortiManger VM environment:
       - Click "Power on this virtual machine"
       - Wait for couple minutes til it reach the "FortiManager login", leave it like that, just keep the image open.
   - For CML VM environment:
       - Click "Power on this virtual machine"
       - You will see an IP address to access CML UI. Note that it is different for everybody depending on your setup `Access the CML UI from https://192.168.202.136/`
       - Open a browser and paste the ip address. If the browser is not allowing it, click the "Advaced" and proceed.
       - On the login page, "`admin`" is the username and "`P@ssw0rd25`" is the password.
       - 
         
     

