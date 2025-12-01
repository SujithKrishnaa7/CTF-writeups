# HTB: Lame - Writeup

**Difficulty:** Easy 
**Category:** Linux

--------------------------------RECON--------------------------------  

`nmap -sV -sC -T5 10.10.10.3`  
> Provides the Nmap Scan results of Open Ports and Services running on it.

Trying to exploit vsftpd 2.3.4 doesn't work . So,I switched to other services.Fortunately , I noticed the Samba shares.

I executed `smbclient -L \\10.10.10.3\`  
It listed some the common shares that doesn't helpus in this scenario.

I tried for CVE exploit.I got one
> CVE-2007-2447

--------------------------------Gaining Access---------------------------------
`searchsploit Samba 3.0.20`  

Prints the Metasploit Results , load the exploit and Set the RHOSTS & LHOST(Note, you were connected to VPN , use the 10.x.x.x IP)  

RUN IT
BOOM!!!!!!!!!!!!!!!!!!!!!!!!!  

-------------------------------Privilege Escalation---------------------------  

Forunately, we are ROOT user.
> User Flag : f3f75f2d5c07515b22e429d6d398710d

> Root Flag : 4e86f9e1de1a179ff7a5eda52ed863b5
