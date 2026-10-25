# Cyber Attack Lifecycle Lab

## Table of Contents
- [Cyber Attack Lifecycle Lab](#cyber-attack-lifecycle-lab)
  - [Table of Contents](#table-of-contents)
  - [Welcome!](#welcome)
  - [Your Mission](#your-mission)
  - [Lab Access and Topology](#lab-access-and-topology)
    - [Access Instructions](#access-instructions)

---

## Welcome!

Welcome to the **Feline Cyber Attack Lifecycle Lab**! In this lab, you will assume the identity of an intruder to learn how modern adversaries compromise enterprise infrastructure. We will step through stages such as reconnaissance, initial access, lateral movement, Active Directory exploitation, and persistence.

**Target Audience:** CISOs, Security & Network Architects, and Security Engineers.

The lab is not about learning hacking tools or how to craft malware that evades defenses. Rather, it explains *how* an intruder gains access and *what* an intruder can do once inside. You will also learn that stealthy adversaries avoid noisy exploitation and scanning to bypass automated cyber defense alarms.

To keep the focus on strategic architecture and the "why," commands are structured for quick copy-and-paste execution so you do not have to type long strings into a Linux shell.

---

## Your Mission

You will assume the identity of an intruder. Your mission—should you choose to accept it—is to compromise the enterprise **Cruel Testing Ltd.** As you will notice, the enterprise is run by cats who perform animal testing on rats, something you very much oppose. Your compromise must be complete: you will gain persistent control of their infrastructure and ultimately help your rat friends escape.

If you accept the mission, start by browsing to [http://www.crueltesting.com](http://www.crueltesting.com) to learn about your target.

![Cruel Testing](assets/cruelwebsite1.png)
*Cruel Testing Public Website*

---

## Lab Access and Topology

All you need to access the lab is a web browser (Chrome is recommended for easy copy/paste).

The lab topology is shown in the figure below. From your laptop, you will access a Guacamole server. Almost all your work will take place from `evil`, the attacking machine. You will also have access to a Windows 11 client machine (`garfield-client`) to simulate a target user opening files or inadvertently assisting the intruder.

Note that Cruel Testing has deployed a Next-Generation Firewall blocking all incoming traffic from the external network (`evil`) to internal subnets and the DMZ.

![Lab Topology](assets/lab-topology.png)
*Lab Topology*

### Access Instructions
1. Your proctor will provide your student number (`1, 2, 3... 15`) and password.
2. Access your assigned Guacamole instance: `http://guac<number>.felinecyber.com` *(e.g., Group 8 uses `http://guac8.felinecyber.com`)*.
3. Ignore security/certificate warnings and proceed.
4. Log in with username `guacadmin` and your assigned password.
5. Right-click both **`evil-rdp`** and **`garfield-client`** connection options to open them in separate browser tabs.

Click on your `evil-rdp` tab, open a terminal window, and type `startlab`:

```bash
startlab