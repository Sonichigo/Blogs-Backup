---
title: "Advanced Linux commands and Google Dorking"
datePublished: Mon Aug 02 2021 06:47:41 GMT+0000 (Coordinated Universal Time)
cuid: ckru9t1hl0saidps15fxoh268
slug: advanced-linux-commands-and-google-dorking
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627886776032/r238Ztwy8.jpeg

---

<p>
We started off with discussing the importance of knowing what we are doing here.
There are basically three types of hackers in this community – Grey Hat, Black Hat and White Hat 
hackers. This is a training session for the future White Hat’s, aka Ethical Hackers!
White hat hackers choose to use their powers for good rather than evil. They employ the same 
methods of hacking as black hats, with one exception- they do it with permission from the owner 
of the system first, which makes the process completely legal. White hat hackers perform 
penetration testing, test in-place security systems and perform vulnerability assessments for 
companies.</p>

**Beware: We are not here for personal gains! Apart from this, Enjoy Hacking! :D **

Read more about this [here](https://sonichigo.hashnode.dev/introduction-to-ethical-hacking-or-linux-basics-or-networking)
<br><br>
# <u>ADVANCED LINUX</u>

Then I started with basic to advanced linux commands that we’re gonna work with throughout
this training session –
- NOTE – You always have a handy manual if you want to know more about any command, inbuilt
into Linux.

- Usage
     - ``man <command>`` Eg- man chmod
     - ``ls –lra`` - Listing directory contents { long listing, all, reverse }
     - ``gedit`` – graphical text editor
     - ``nano/vim/vi`` -terminal text editor
     - ``echo “CONTENT” > file.txt`` 
     - ``>`` is used to write/overwrite >> used to append to a file
Eg- ifconfig > ip.txt

- cat / tac / more / less - display contents of a file
- head / tail - first and last 10 lines of the content of a file
- nl <filename> – file with line number
- find command
-   Recursive so it’ll literally search everywhere inside that directory
-   Find /dir_name –name <file> - to search inside a specific directory
 which <command_name> -tells us the location of the binary. It searches for the binaries in 
- ``“echo $PATH”``
 where is <cmnd_name> - It finds for the binaries anywhere, not only in the above locations
 locate <file/command> - FASTER, uses a pre made database. NEW FILES CANNOT BE FOUND 

# <u>INFO GATHERING</u>

So do you know about IoT devices? 

Basically these are very small computers embedded with electrical devices to form a smart 
device. Eg- Smart home stuff, cameras, etc

Now if you wanna control them, you generally use your phone, and these IoT devices are 
connected to your home router. The catch here is.. These devices then become connected to 
Internet UNSECURED. They are very tough to secure… think about it! Imagine you have to put a 
password to switch on your smart bulb -_- 

The address to these devices can be found out easily on the internet with the help of Google 
Dorking or Shodan.

This is one of the few applications of Google Dorking. 
![0_EA62_VMXI5zNV8FS.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627886794906/TqaAwj0Ag.png)
