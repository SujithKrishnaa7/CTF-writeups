# HTB: Voleur - Writeup

**Difficulty:** Medium  
**Category:** Windows 

*Recon*
`nmap -sV -sC 10.10.11.76`
<img width="1360" height="450" alt="image" src="https://github.com/user-attachments/assets/ef153a20-471d-43d7-b1f4-96bec33005c7" />

We came to know that,ssh is not connecting.

`netexec smb 10.10.11.76 -u ryan.naylor -p HollowOct31Nyt -k`

So stepping to Enumeration of SMB Shares.



`smbclient --use-kerberos=required //DC.voleur.htb/HR`



