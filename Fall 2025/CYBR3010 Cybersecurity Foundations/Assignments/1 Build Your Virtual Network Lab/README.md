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

# Introduction

This lab is an interconnected network configuration designed through MS Visio and built within Cisco Modelling Lab (CML) environment. Its purpose is to provide reliable communication/connectivity between different network resources.

    'Software Used'
    - VMware Workstation Pro
    - Cisco Modelling Lab (CML)
    - MS Visio


# Network Diagram

This is the network layout which consists of 4 virtual client machines running on different operating systems (Kali Linux, Windows 10, Windows 11, Windows 7), a network switch, and a firewall connected to the internet via NAT network adapter.

--- screenshot here from visio


# Device Status Proof (refer to video 03 Building Your First Lab Part 1)




# Rebuild Guide

1. Install VMware Workstation Pro:
   - To get the installer, go to https://support.broadcom.com/
   - Click "Register"
   - Verification code will be sent in email. Continue filling-up the form until account is created.
   - Click "Login" to login to your account for the first time. `username` domingo.arrbelrey+broadcomcybersecurityrequirement@gmail.com `password` qwerty01A.
   - Once logged-in, you will see your dashboard.
   - On the left pane, click "My Downloads"
   - You can either search VMware Workstation Pro or where you see "Free Software Downloads", click "HERE"
   - Click "VMware Workstation Pro". You will then see an option to download different versions of Windows and Linux.
   - Click the version depending on the machine you are using. Agree to the terms and conditions.
   - You can now download the installer and use the executable file to install VMware Workstation Pro in your machine.

2. Download the images needed to setup CML and FortiManager 
  > `Student_CML_FortiManager2_OVA` folder is 1.26 GB <br>
  > `CML_Student_OVA` folder is 75.7 GB in total --> in video tut, this is Student_CML_Master

  - For this assignment, there are 2 folders needed to download and unzip. One folder is for FortiManager and one folder is for CML Student.
    - To download and unzip FortiManager folder:
      - Go to Brightspace where FortiManager folder is uploaded.
      - Click "Download".
      - Keep the "File name" and "Save as type", then choose the location where you want to save the unzipped folder.
      - Click "Save"
      - Go to the location where you saved the unzipped folder
      - Right click the folder
      - Click "Extract All"
      - Choose a location where you want to save the extracted/zipped folder
      - Click "Extract"
    - To download and unzip (using 7zip) CML Student folder:
      - Go to Brightspace where CML Student folder is uploaded.
      - Notice that there are 13 OVA (Open Virtual Appliance) files, download those 13 files one-by-one (depending on your internet connection, this would take time).
      - Keep the "File name" and "Save as type", then choose the location where you want to save the OVA files.
      - Click "Save". Repeat until you get all 13 OVA files.
      - If your machine do not have 7zip File Manager, download the app.
      - Open 7zip File Manager app
      - Where you see a folder icon, paste the location of those 13 OVA files
      - Highlight the first (.001) OVA file
      - Click "Extract"
      - Choose a location where you want to extract.
      - Click "OK". Depending on your machine, the extract process takes some time.
        
    - When both FortiManager folder and CML Student folder are extracted, there are images you need to run/click to open it in VMware Workstation Pro. 
      - For CML Student folder (main CML):
        - Click .ovf (Open Virtualization Format) file  
        - Enter a name of the new virtual machine. `CML01`
        - Browse for the storage path of the new virtual machine
        - Click "Import". This process will take some time to finish.
        - Once done, you will see VMware Workstation. Click "Edit virtual machine settings"
        - "Network Adapter" should be set to "NAT" (give the device internet access and IP address thru closed networks through desktop networks which will not exposed to the outside which is good)
        - "Network Adapter 2" should be set to "Custom: VMnet2" (initially set to Bridged Automatic which means its connected to actual network where it acquire an IP address and connect to your real network that is something not desirable. Custom: VMNet2 is a closed environment meaning it will not be interfering with any other network outside.)
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

3. Power on the new VM and explore CML
  - For FortiManger VM environment:
    - Click "Power on this virtual machine"
    - Wait for couple minutes til it reach the "FortiManager login", leave it like that, just keep the image open.
  - For CML VM environment:
    - Click "Power on this virtual machine"
    - You will see an IP address to access CML UI. Note that it is different for everybody depending on your setup `Access the CML UI from https://192.168.202.136/`
    - Open a browser and paste the ip address. If the browser is not allowing it, click the "Advaced" and proceed.
    - On the login page, "`admin`" is the username and "`P@ssw0rd25`" is the password.
    - Right off the bat, you will see your CML dashboard where you can add projects/lab. Below the screen, you will see the health data which includes the CPU, Memory, and Disk, as well as the licensing.
      
4. Building the Lab in CML
  - To add Nodes (network devices)
    - On the CML screen, Click "Add"
    - You will then proceed to the lab dashboard environment where you can see the workbench consisting of different tools to complete the lab.
    - At the top of the screen where you see a dropdown icon, rename the "Lab Title". "Lab Description" is optional.
    - The most important tools in the workbench is the "Add Nodes" where you can add network resources such as firewall, switch, router, desktop machine, etc. Depending on the requirement, drag and drop a node into your lab environment.
  
  - To check and edit the details of the Nodes (network devices)    
    - Click the node to see the details on the right side of the screen.
    - On the right side of the screen, you can see "Settings" tab where you can change the "Node Name", "Connectivity" tab, "Config" tab, and "Interfaces" tab.
    - `TIP: Before starting the nodes/devices, it is recommended to first setup the nodes/devices properly.`

  - To connect each Nodes (network devices)
    - If you drag and drop all the nodes you need and it is time for you to make a connection within each nodes, right click the node and click "Add Link".
    - Choose a port number where you want to connect from "Source Interface" to "Target Interface".
    - Click "Create Link".

  - To start the Node (network devices)
    - When connection is done, right click the node and click "Start" (It is advisable to start the node one at a time to avoid stressing the machine because starting the nodes at the same time requires heavy amount of CPU and RAM. You know the node successfully started if you notice a green check icon in the node)

  - To check the status of nodes/devices you started
    - Right click the node and click "Console" for switches and routers, and click "Open Console".
    - Otherwise, click "VNC" for client desktop machine and click "Open VNC".

  - To delete the nodes/devices that you started
    - Right click the node then click "Stop".
    - Right click the node again and click "Wipe" (Wipe means going back to the original state with no configuration).
    - Click "Confirm"
    - Another right click on the node and click "Delete".

