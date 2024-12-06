---
title: "Introduction to Ethical Hacking | Linux Basics | Networking"
datePublished: Sun Jul 25 2021 12:34:45 GMT+0000 (Coordinated Universal Time)
cuid: ckrj6okab0qtrnts101dh66dx
slug: introduction-to-ethical-hacking-or-linux-basics-or-networking
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627216453964/P2uGgeBPL.webp

---

### What is Ethical Hacking?
Hacking is the process of finding vulnerabilities in a system and using these found vulnerabilities to gain unauthorized access into the system to perform malicious activities ranging from deleting system files to stealing sensitive information. Hacking is illegal and can lead to extreme consequences if you are caught in the act. People have been sentenced to years of imprisonment because of hacking.

<img src="https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/12/Ethical-Hacking-What-is-Ethical-Hacking-Edureka-768x304.png">

#### These are various types of hackers:

```
1. White Hat Hackers (Cyber-Security Hacker)           
2. Black Hat Hackers (Cracker)                         
3. Gray Hat Hackers (Both) 
```

Let’s summarize them one-by-one.

1. <b>White Hat Hackers:</b>
Here, we look for bugs and ethically report it to the organization. We are authorized as a user to test for bugs in a website or network and report it to them. White hat hackers generally get all the needed information about the application or network to test for, from the organization itself. They use their skills to test it before the website goes live or attacked by malicious hackers.

2. <b>Black Hat Hackers:</b>
Here, the organization doesn’t allow the user to test it. They unethically enter inside the website and steal data from the admin panel or manipulate the data. They only focus on themselves and the advantages they will get from the personal data for personal financial gain. They can cause major damage to the company by altering the functions which lead to the loss of the company at a much higher extent. This can even lead you to extreme consequences.

3. <b>Grey Hat Hackers:</b>
They sometimes access to the data and violates the law. But never have the same intention as Black hat hackers, they often operate for the common good. The main difference is that they exploit vulnerability publicly whereas white hat hackers do it privately for the company.

## Resources

1. [Read](https://www.kali.org/blog/) about the Kali releases and it's features. Visually, it has become quite interesting recently! 
2. [Introduction to Ethical Hacking](http://wiki.cas.mcmaster.ca/index.php/Ethical_Hacking)
3. [Basic Kali Linux Commands](https://github.com/dexter-11/Konnexions-2020/blob/master/Day%201/Kali-Linux_Command_List.txt)
4. Linux essentials for Hackers - [Hackersploit](https://hackersploit.org/linux-essentials-for-hackers/) || [Nullbyte](https://null-byte.wonderhowto.com/how-to/linux-basics/) - RECOMMENDED




## Day 1a [Handy commands and actions]

### Your handy terminal shortcuts!
* *Ctrl+C* Terminates a job/process
* *Ctrl+Shift+C* Copy
* *Ctrl+Shift+V* Paste

### Keep system and applications up-to-date

**`sudo apt-get update && sudo apt-get upgrade`** OR **`sudo apt update && sudo apt full-upgrade`**    -> Update the kali repositories

**`sudo apt-get install <application_name>`** -> Install/update git community repositories


### Access privileges
Most Popular command - **`chmod 777 <file>`**   
            
When we want to set permissions, we just add up the number. 
For example, to set the permissions to read and write, we will use ‘6’ (4 + 2) for the permission. 
 
Here are the different permutations:
   * 0 – *no permission*
   * 1 – *EXECUTE*
   * 2 – *WRITE*
   * 3 – *write and execute*
   * 4 – *READ*
   * 5 – *read and execute*
   * 6 – *read and write*
   * 7 – *read, write, and execute*
   
Depending on the permissions you want to grant to the file, you just set the number accordingly.
What about the 3 digits ‘777’? Well, the First digit is assigned to the Owner, the Second digit is assigned to the Group and the Third digit is assigned to the Others. 
**So for a file with ‘777’ permission, everyone can read, write and execute the file.**


## Day 1b [Types of Hackers | Networking]

1. [Hacker_Roadmap](https://github.com/sundowndev/hacker-roadmap#wrench-exploitation-tools) (:star: Star this repository for future reference)

2. [Types of Hackers](https://www.malwarefox.com/types-of-hackers/) 

#### NETWORKING
```
sudo ifconfig 
OR ip -a           // network adapter information (Your machine's IP is visible at eth0)
iwconfig           // wlan adapters information
ping <ip/url>      // ping to check connection and stability
arp -a             // IP address with MAC address
route              //routing table tells you where the traffic exits
netstat -ano       // all open connections and which one is talking from what port number
```

* Running a local sever on Kali machine -> **`python -m SimpleHTTPServer 8080`** <br/>
Now, go in any browser and enter **<kali_ip>:8080** to access system files! <br/>

TCP : https   smtp   ftp <br/>
    1. Connection oriented <br/>
    2. Give Response <br/>
    
UDP : dns   ntp <br/>
    1.No response <br/>

_____________Three Way Handshake______________ <br/>
CLIENT  ---------->Server    <br/>
          *syn* <br/>
CLIENT<-----------Server <br/>
          *ack+syn* <br/>
CLIENT----------->Server<br/>
          *ack* <br/>
Connection complete! <br/>
______________________________________________ <br/>

* **Flags** <br/>
FIN - transmission finished <br/>
PSH - send buffer <br/>
URG - important packet <br/>
RST - Reset connection <br/> <br/>

* Read about a [**Christmas Tree packet**](https://en.wikipedia.org/wiki/Christmas_tree_packet).
<br/>

* **Ports**  <br/>
1.Open  - that actively respond to incoming connection <br/>
2.Closed  - that respond but does not have any services running on that port (Firewall not present) <br/>
3.Filtered - (Firewall present) protected and prevents nmap from determining open/closed <br/>
4.Unfiltered  -  nmap can access but cannot determine open/closed <br/>
5.Open-filtered - nmap belives to be open but can not say <br/>
6.Close-filtered - nmap belives to be closed but can not say <br/> <br/>


### Staying Anonymous in Kali Linux
[How to stay anonmous](https://www.youtube.com/watch?v=VZMHfO9rOCg&list=PLBf0hzazHTGOh6JBKc8WkpyuZgDPW6yTk&ab_channel=HackerSploit)