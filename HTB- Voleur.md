# HTB: Voleur - Writeup

**Difficulty:** Medium  
**Category:** Windows 

*Recon*
`nmap -sV -sC 10.10.11.76`
<img width="1360" height="450" alt="image" src="https://github.com/user-attachments/assets/ef153a20-471d-43d7-b1f4-96bec33005c7" />

Before diving into penetration testing, I had some doubts about Kerberos and how to connect :

1) How does Kerberos authenticate?

`[User] --AS-REQ--> [DC/AS]
        <--AS-REP--
              (gets TGT)

[User] --TGS-REQ--> [DC/TGS]
        <--TGS-REP--
              (gets Service Ticket)

[User] --AP-REQ--> [Service]
        <--Access--`

2) How to get TGT in Kali ?

\\Term 'Realm' means domain address "voleur.htb"
Two things are needed to get TGT:
1) kinit --> Pre-installed in Kali
2) krb5.conf 

Kerberos IS already installed in Kali Linux by default.
The tools you need are built into the MIT Kerberos client package:

kinit — request a TGT

klist — list tickets

kdestroy — delete tickets

/etc/krb5.conf — Kerberos configuration

kinit <username> --> move the krb5.conf file to the directory /etc/

Now use klist, which lists the tickets






We came to know that,ssh is not connecting.

`netexec smb 10.10.11.76 -u ryan.naylor -p HollowOct31Nyt -k`

So stepping to Enumeration of SMB Shares.



`smbclient --use-kerberos=required //DC.voleur.htb/HR`



