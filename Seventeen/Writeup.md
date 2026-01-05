#HTB - Seventeen -Writeup

**Level**:Medium 

---------------------------------------RECON-------------------------------- 

`sudo nmap -sV -sC  -T5 10.129.227.14` 
<img width="643" height="404" alt="image" src="https://github.com/user-attachments/assets/776cd56d-cf27-4166-ade3-c4a7e24caee3" />  

I understood there are two things: this is Web Vulnerability Exploit LAB!!!!!!!!!!!!  

Visiting the Site ,everything normal ,no secondary pages ,only `Index.html` 
<img width="1919" height="640" alt="image" src="https://github.com/user-attachments/assets/0a81c906-9966-40ab-b746-f6ac0c54926b" /> 
As usual ,most common type of Content Discovery is Subdomain Enumeration . Moving to Ffuf 

`ffuf -u http://seventeen.htb -H "Host:FUZZ.seventeen.htb" -w /home/kali/Desktop/wordlists/top_subdomains.txt -t 150 -fs 20689` 

I got a Intresting vhost `exam.seventeen.htb` 

Found out an exam Reviewer System and a Parameter. It seems like talking to DB , so injected ['] into Param and led to SQL Injection. 

Searched on Google for Exploits for exam Reviewer System , I found two ,one ia Authenticated Method another is our finding SQL!!!!!!!!!!!!!!! 

<img width="1091" height="584" alt="image" src="https://github.com/user-attachments/assets/a5c984bd-ff39-4fd1-a20c-db6542ddb51c" /> 
Using ExploitDB , got an Endpoint where the vulnerability. hidden. 
`http://exam.seventeen.htb/?p=take_exam&id=1` 

<img width="1920" height="1080" alt="Screenshot (7)" src="https://github.com/user-attachments/assets/d49f43fe-6e22-4292-9b45-46d38d58ff98" /> 

Save as "exam.req" and use SQLMap to automate , because Boolean Injections where mostly guessing the values, it needs more iterations and repetitions, if it was UNION, things would be simple. 

`sqlmap -r exam.req -p id -dbs -threads 10` 

Enumerated 4 DB. 

[*] db_sfms 

[*] erms_db 

[*] information_schema 

[*] roundcubedb 

<img width="956" height="296" alt="image" src="https://github.com/user-attachments/assets/6e3c2b4a-fd70-4695-ab1c-5cd36d107796" /> 

When i encounter this oldmanagement, I got confused and reffered Ippsec YT video, it might be another VHOST , guess . It worked though. 

Now password cracking part...... 


-------------------------------------------------Initial Access---------------------------------------------- 

Copy these Hashes and pass to Hashcat 

<img width="424" height="195" alt="image" src="https://github.com/user-attachments/assets/e13eac35-37d3-4157-b19e-a1886a0f5a87" /> 

`hashcat --user -m 0  hash.txt /usr/share/wordlists/rockyou.txt` 

Found out the Password for Student:Shane 

Visit "oldmanagement.seventeen.htb" and login with Student ID and pwd . 

<img width="1919" height="692" alt="image" src="https://github.com/user-attachments/assets/aae597ba-273c-41a4-95ec-43fbc118f29f" /> 
There is a Uploading Option , I jumped into testing PHP Reverse-Shell. 

Through a lot of enumeration. Using a file storing path in SQLMap, I came to the directory 'oldmanagement/files' 
Again, a dead end, some walkthrough led me to file storing path 'http://oldmanagement.seventeen.htb:8000/oldmanagement/files/31234/Marksheet-finals.pdf' 
Through downloading the source code of the school management system, I found out the File Uploading method to escalate RCE. 

Two things 

1) Intercept the request. to upload files.
2) Change the Student ID for a Shell.PHP upload.
3) Go to 'http://oldmanagement.seventeen.htb:8000/oldmanagement/files/9001/shell.php?shell=$cmd'
Though we could escalate the Privileges.

Chnage the Dir to `var/www/html` ,move to employee management . 
Use `grep -Ri pass /var/www/html/employeemanagementsystem` to search for keyword pass. 





















