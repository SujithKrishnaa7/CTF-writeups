# Empire: Breakout - Writeup

**Difficulty:** Easy  
**Category:** Linux 

Initially starting with basic Scanning :
Found `-p 22,139.445,1000,2000`  
<img width="1087" height="389" alt="image" src="https://github.com/user-attachments/assets/667d6387-d057-49bc-9f79-1b517be8823e" />
The Samba shares don't allow us to log in as Anonymous.

It is stated as Easy, I know it is a Basic Privilege Escalation
`enum4linux <IP>`
Found a local user  
<img width="999" height="89" alt="image" src="https://github.com/user-attachments/assets/845a9fa3-4f4c-4c43-9d5d-da1b86269da9" />
Let's open the HTTP sites to find out interesting
> http://<IP>:1000 ; http://<IP>:2000 -->  Displays a login portal

http://<IP>:80 --> Displays a HTTP page. 
Open the source code to get any API endpoints.
Scroll to the last, and  you will find an Encrypted sign.
I checked with perplexity , it is a classic Brainfuck encoding language. 
Copy the String including '.'full stop
> Password : .2uqPEfj3D<P'a-3

---------------------------------------------------------------------------Initial Gain--------------------------------------------------------------------------
ssh would work ,but to be better login through usermin login.

In Website, there is a cmdshell , you will get User flag.
>3mp!r3{You_Manage_To_Break_To_My_Secure_Access}

**In simple, create a Reverse shell and make it Interactive**
In backup folder , you'll find a .bak file , it seems like a key for next door.
<img width="832" height="141" alt="image" src="https://github.com/user-attachments/assets/5359b44e-15f9-4f24-85c2-75c09ecfda9e" />
---------------------------------------------------------------------------Escalating--------------------------------------------------------------------------
In same DIR, you'll find TAR cmd-line utility to create archive
Tar :<img width="758" height="383" alt="image" src="https://github.com/user-attachments/assets/dbd8f3cc-1715-41cb-b961-f87b61442618" />  

`./tar -cf back.tar /var/backups/.old_pass.bak`  
> Creates a TAR file with the contents of .bak file
`tar -xvf back.tar`

Navigate through Directory , and cat r00t.txt

>Root Flag : 3mp!r3{You_Manage_To_BreakOut_From_My_System_Congratulation}
