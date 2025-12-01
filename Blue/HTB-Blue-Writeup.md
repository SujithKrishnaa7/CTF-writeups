# HTB: Blue - Writeup

**Difficulty:** Easy
**Category:** Windows 

---------------------------------------RECON--------------------------------------------  

`sudo nmap -sV -sC -T4 10.129.19.107  -oN /home/kali/Desktop/test`  
<img width="1337" height="368" alt="image" src="https://github.com/user-attachments/assets/a3dddadf-11d8-4460-b86a-02ecd89b8131" />  
Open ports were displayed  
--------------------------------EXPLOIT--------------------------------------------------
Even before scanning, I guessed it might be Vulnerable to the Eternal Blue attack, though it was a Windows Machine.  
Rather than trying extra things, I dove into Metasploit and searched EternalBlue.  
<img width="1821" height="92" alt="image" src="https://github.com/user-attachments/assets/bb6b9f75-104d-41b7-9d52-54e7ec995c81" />
set the LHOST and RHOST .  
!!!EXPLOIT!!!



> User flag : 7cf1a97b69e92a2be9724e4fe592ce6c

> Root flag : 95df7f41b87eae29aca49b2a5d51ecec
