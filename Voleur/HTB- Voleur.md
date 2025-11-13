# HTB: Voleur - Writeup

**Difficulty:** Medium  
**Category:** Windows 

*Recon*
`nmap -sV -sC 10.10.11.76`
<img width="1360" height="450" alt="image" src="https://github.com/user-attachments/assets/ef153a20-471d-43d7-b1f4-96bec33005c7" />

Before diving into penetration testing, I had some doubts about Kerberos and how to connect :

## 1) How does Kerberos authenticate?

[User] --AS-REQ--> [DC/AS]
        <--AS-REP--
              (gets TGT)

[User] --TGS-REQ--> [DC/TGS]
        <--TGS-REP--  
        (gets Service Ticket)

[User] --AP-REQ--> [Service]
        <--Access--

## 2) How to get TGT in Kali ?

#Term 'Realm' means domain address "voleur.htb"
Two things are needed to get TGT:
1) kinit --> Pre-installed in Kali
2) krb5.conf 

Kerberos IS already installed in Kali Linux by default.
The tools you need are built into the MIT Kerberos client package:

>kinit — request a TGT

>klist — list tickets

>kdestroy — delete tickets

/etc/krb5.conf — Kerberos configuration

`kinit <username> --> move the krb5.conf file to the directory /etc/`  
Now use klist, which lists the tickets

[Note : The krb5.conf file causes biggest headache . I have attached a krb5.conf file,Kindly use that ]

`sudo ntpdate DC.voleur.htb`  
Dont forget to nun this to sync time with DC

--------------------------------------Initial Setup Done---------------------------------------------------------  
`nxc smb DC.voleur.htb -k -u ryan.naylor -p 'HollowOct31Nyt' --shares`  
It'll list the share folders available 

`netexec smb 10.10.11.76 -u ryan.naylor -p HollowOct31Nyt --shares`

Accessing a Share folder using `smbclient` cmd-line  
`smbclient  -U 'voleur.htb/ryan.naylor%HollowOct31Nyt' //dc.voleur.htb/IT `  

Navigate to \First-Line Support\, you'll find the Excel file.
`smb: \First-Line Support\> get Access_Review.xlsx`  
But the Excel file is encrypted, but sure the password is weak ,becoz of CTF .

#Using office2john tool to convert the file to Hash 
`office2john Access_Review.xlsx > hash.txt`  
Cracking password using johnTheRipper tool ;
`john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt`  





