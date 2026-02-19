<h1> Cyberthreat Intelligence (CTI) Threat Actor and MITRE ATT&CK Framework </h1>

![alt text](1_qSIVN9fWM_DrD_sOhFlJQA.gif)

<!-- TOC -->

- [1 Introduction](#1-introduction)
  - [1.1 What is CTI?](#11-what-is-cti)
  - [1.2 What is Threat Actor](#12-what-is-threat-actor)
  - [1.3 What is MITRE ATT\&CK?](#13-what-is-mitre-attck)
- [2 Threat Actor - APT32 aka OceanLotus](#2-threat-actor---apt32-aka-oceanlotus)
  - [2.1 Threat Actor Introduction](#21-threat-actor-introduction)
  - [2.2 TTP Used by APT32](#22-ttp-used-by-apt32)
  - [2.3 Motivations Attributed to APT32](#23-motivations-attributed-to-apt32)
  - [2.4 Target Industries by APT32](#24-target-industries-by-apt32)
  - [2.5 Attributed Attacks by APT32](#25-attributed-attacks-by-apt32)
    - [2.5.1 Operation Cobalt Kitty](#251-operation-cobalt-kitty)
    - [2.5.2 Covid-19 Espionage](#252-covid-19-espionage)
  - [2.6 MITRE ATT\&CK mapping](#26-mitre-attck-mapping)
- [3 Mapping several adversaries (APT28, APT29, APT39) on MITRE ATT\&CK Navigator](#3-mapping-several-adversaries-apt28-apt29-apt39-on-mitre-attck-navigator)
  - [3.1 APT28 - create layer, set color coding, and insert scoring](#31-apt28---create-layer-set-color-coding-and-insert-scoring)
  - [3.2 Combining all APT group into single layer](#32-combining-all-apt-group-into-single-layer)
- [Useful Links](#useful-links)
- [References](#references)

<!-- /TOC -->

## 1 Introduction
  > This document is about the MITRE ATT&CK Framework and the process to map TTP (Tactic, Technique, Procedure) of threat actors to the MITRE ATT&CK navigator.
  > - TTP mapping for two attributed attacks of APT32 (aka OceanLotus) which are `Operation Cobaly Kitty` and `Covid-19 Espionage`.
  > - TTP mapping for three threat actors which are `APT28`, `APT29`, and `APT39`.

### 1.1 What is CTI?
  Cyber threat intelligence (CTI) refers to information and insights gathered, analyzed, and shared to understand and defend against current and future cyber threats. It provides organizations with actionable insights about ongoing and emerging threats, adversary tactics, techniques, and procedures (TTPs), and vulnerabilities in their systems. (https://www.bluevoyant.com/knowledge-center/cyber-threat-intelligence-cti-definition-types-process)

  To give you an image, I will reinforce my front door to which I will add locks and cameras. These extra locks allow me to deal with actors trying to force here every day. The CTI is used to detect when this takes place, it is a means of anticipation allowing me to see that people are trying to enter my home. -- François Deruty, COO Sekoia.io (https://www.sekoia.io/en/glossary/what-is-cti/)

  > Threat - any potential damage that can cause negative impacts to the operations, assets, etc

  > Intelligence - corporate capability to forecast changes.

  > In short, cyberthreat intelligence is making sure that we are on top of the things. We're gathering data from our systems and analyzing this data to make sure if there are threats that could harm the organization, it could be prevented from happening.

### 1.2 What is Threat Actor
  Threat actors, also known as cyberthreat actors or malicious actors, are individuals or groups that intentionally cause harm to digital devices or systems. Threat actors exploit vulnerabilities in computer systems, networks and software to perpetuate various cyberattacks, including phishing, ransomware and malware attacks. (https://www.ibm.com/think/topics/threat-actor)

  > Threat Actor - refer to individual of group of organization that seeks to compromise the Confidentiality, Integrity, and Availability of computer systems, networks, or data for malicious purposes.

### 1.3 What is MITRE ATT&CK?

  MITRE ATT&CK refers to a group of tactics organized in a matrix, outlining various techniques that threat hunters, defenders, and red teamers use to assess the risk to an organization and classify attacks. Threat hunters identify, assess, and address threats, and red teamers act like threat actors to challenge the IT security system. (https://www.fortinet.com/resources/cyberglossary/mitre-attck#:~:text=MITRE%20ATT%26CK%20refers%20to%20a,an%20organization%20and%20classify%20attacks.)

  > It is a framework developed by the MITRE corporation. It provides structured and comprehensive understanding of threat actors or adversaries behavior in different stages of a cyber attacks. (https://www.youtube.com/watch?v=iOkkAfVAFyc)

  ATTA&CK - Adversarial, Tactics, Techniques & Common Knowledge
  > Adversarial - these are the threat actos or the attackers

  > Tactics - high level objectives that adversaries aim to achieve during the cyber attack. Represents “why” or the reason an adversary is performing an action.

  > Techniques - specific methods or actions that adversaries use to accomplish their objectives within each tactics. Represents “how” adversaries achieve tactical goals by performing an action.

  > Common Knowledge - collective understanding and awareness of the tactics techniques and procedures (TTP)

  > Sub-techniques, a more specific or lower-level description of adversarial behavior.

  > Procedures, specific implementation or in-the-wild use the adversary uses for techniques or sub-technique

## 2 Threat Actor - APT32 aka OceanLotus

### 2.1 Threat Actor Introduction

  APT32 is a suspected Vietnam-based threat group that has been active since at least 2014. The group has targeted multiple private sector industries as well as foreign governments, dissidents, and journalists with a strong focus on Southeast Asian countries like Vietnam, the Philippines, Laos, and Cambodia. They have extensively used strategic web compromises to compromise victims  (https://attack.mitre.org/groups/G0050/)

### 2.2 TTP Used by APT32

### 2.3 Motivations Attributed to APT32

### 2.4 Target Industries by APT32

### 2.5 Attributed Attacks by APT32

#### 2.5.1 Operation Cobalt Kitty

#### 2.5.2 Covid-19 Espionage

### 2.6 MITRE ATT&CK mapping


## 3 Mapping several adversaries (APT28, APT29, APT39) on MITRE ATT&CK Navigator 

  >When planning an adversarial emulation campaign or a comprehensive red team operation, we use the TTP (Tactic, Technique, Procedure) of several adversaries or APT (Advance Persistent Threat) groups and combine them in order to come up with a campaign that is hollistic and encompassing all of the TTPs of more than one APT group.

### 3.1 APT28 - create layer, set color coding, and insert scoring

  -  Go to https://attack.mitre.org/
  -  Go to Resources and click "ATT&CK Data and Tools".
  -  Under ATT&CK Navigator, click "Open Application".
  -  Click "Create New Layer".
  -  Click "Enterprise ATT&CK".
  ![Create new layer for APT group.](./screenshot/3.1%20create%20layer.jpg)

    *Create new layer for APT group.*

  -  Once inside the Navigator tool, click "Selection Layer", click search icon, and search APT 28.
    ![Search APT groups.](./screenshot/3.2%20Search%20APT%20group..jpg)

      *Search APT groups.*
      
  -  Under "Threat Groups", click "select" beside APT28. Notice that the techniques will be highlighted.
    ![Select APT group.](./screenshot/3.3%20Select%20APT%20group.jpg)
    
      *Select APT group.*

  - To rename the APT group, go to "Layer Controls", click "layer settings".
    ![Naming the layer.](./screenshot/3.7%20Naming%20the%20layer..jpg)
    
      *Naming the layer.*

  -  To set the color coding, go to "Layer Controls", click "color setup".
    ![Color setup.](./screenshot/3.4%20Color%20setup.jpg)
    
      *Color setup.*

  - Set the "Low Value" to 1 and "High Value" to 3. 
      
    ![](./screenshot/3.5%20Set%20color%20value.jpg)
      
      *Set color value.*
  
    >`Note:` In terms of putting score to the color coding, since we are comparing 3 APT groups; `1 = there is only one TTP present in all APT groups`, `2 = there are two TTP present in all APT groups`, `3 = these are the common/similar TTP in all APT groups`.

  - To insert the score, go to "Technique Controls", click scoring icon, and put 1. Notice that TTP are now in color red.
    
    ![Scoring of APT group.](./screenshot/3.6%20Scoring%20of%20APT%20group..jpg)
    
      *Scoring of APT group.*

  - To add another separate layer of TTP for APT29 and APT39, click the plus icon.
    ![Create new layer.](./screenshot/3.9%20Create%20new%20layer.jpg)
    *Create new layer*
  - Repeat the process to `Search APT group` until `Scoring of APT group`.
  - At this point, there should be 3 layers, namely; APT28, APT29, and APT39.
  - 
  - For TTP of the three APT groups, below are the SVG file downloaded for APT28, APT29, and APT39 respectively.

    ![TTP of APT28 group.](./screenshot/3.8%20TTP%20of%20APT28%20group..svg)
    
      *TTP of APT28 group.*
  - 

### 3.2 Combining all APT group into single layer
Use case
1. See the collective TTPs of all APT groups in one layer
2. See the commonalities/similarities of what TTPs the groups share between them (in relation to color coding and scoring). The combined layer that has cumulative score of more than one means they occur more than once amongst APT group 

## Useful Links

[Introduction To The MITRE ATT&CK Framework](https://www.youtube.com/watch?v=LCec9K0aAkM)

[Mapping APT TTPs With MITRE ATT&CK Navigator](https://www.youtube.com/watch?v=hN_r3JW6xsY)




## References
Cyber Threat Intelligence (CTI): Definition, Types & Process. (n.d.). Retrieved from BlueVoyant: https://www.bluevoyant.com/knowledge-center/cyber-threat-intelligence-cti-definition-types-process

CTI. (n.d.). Retrieved from sekoia: https://www.sekoia.io/en/glossary/what-is-cti/

What is a threat actor? (n.d.). Retrieved from IBM: https://www.ibm.com/think/topics/threat-actor

How the MITRE ATT&CK Framework Transforms Enterprise Cybersecurity. (n.d.). Retrieved from Fortinet: https://www.fortinet.com/resources/cyberglossary/mitre-attck#:~:text=MITRE%20ATT%26CK%20refers%20to%20a,an%20organization%20and%20classify%20attacks

CyberPlatter. (n.d.). MITRE ATTACK | MITRE ATT&CK | MITRE ATT&CK Explained with an Example | MITRE ATT&CK Analysis. Retrieved from Youtube: https://www.youtube.com/watch?v=iOkkAfVAFyc

APT32. (n.d.). Retrieved from MITRE ATT&CK: https://attack.mitre.org/groups/G0050/






