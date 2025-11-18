-----------------------------------------Recon-------------------------------

`sudo nmap -p- -T4 -sV 10.10.11.221 -sC`  
Running the Initial Info. Gathering,
<img width="700" height="364" alt="image" src="https://github.com/user-attachments/assets/26386871-35cf-4a0c-b2f6-2b9c52a35a8c" />

We have found that 2 ports were Open , Starting with Webiste .  
--Add the 2million.htb in etc/hosts

---------------------------------Getting Hands Dirty------------------------------  

Diving into the site, you'll notice the Invite option, which is an OG method to sign up in HTB in past years.  
Inspect them to get anything interesting.

Found a JS file in "http://2million.htb/invite"
<img width="1300" height="950" alt="image" src="https://github.com/user-attachments/assets/263fae69-2501-4467-8054-a9e325c742e3" />

It is so messy, we'll make it readable. Paste it in https://beautifier.io/ and check  Detect packers and obfuscators? (unsafe) option.
This displays 2 API endpoints 
Requesting /api/v1/invite/how/to/generate prints a ROT13-encrypted string. Decrypt it.
Time to generate our Code
`curl -q -X POST http://2million.htb/api/v1/invite/generate | jq .`  
<img width="700" height="326" alt="image" src="https://github.com/user-attachments/assets/127da77b-ee9c-44df-b953-9960a84535ce" />
The data was a B64 encoded . Let us decode with the Unix command 
`curl -s -X POST http://2million.htb/api/v1/invite/generate | jq .data.code -r | base64 -d`

Use the Invite code to create an Account.

NO IDEA!!!! WHAT TO DO NEXT !!!

IT IS AN API GAME !! THINK LIKE THAT

http://2million.htb/home/access --Capture the response in Burp

Download the VPN File, you'll notice a API Endpoint /api/v1/regenerate

Pass the GET Request , /api/v1 --You'll find API routes

I tried several routes, but the PUT method works. It can make our account admin.

<img width="700" height="636" alt="image" src="https://github.com/user-attachments/assets/76d22770-815c-4bc3-a007-4425c5c82a1d" />  
Change the Content type to JSON. Passing this request  gives us admin rights

Again NO IDEA!!!! WHAT TO DO NEXT !!!

I checked with Walkthrough, the next step is Reverse Shell

"/api/v1/admin/vpn/generate" Change the Req method and add JSON input , pass the Command for Reverse Shell
Open the Listener in Kali 

BOOM !!! REVERSE SHELL ACTIVATED

Don't waste the TIME, open the Database.php file, it calls 4 Environment variables. (Hint)

`ls -la`  
Displays the .env file, displays the admin password
Use the password for SSH

`ssh admin@2million.htb`  
Again NO IDEA!!!! WHAT TO DO NEXT !!!

Reffered to Writeups : The Vulnerability is in Kernel .

I was navigated to the Git repo : `https://github.com/sxlmnwb/CVE-2023-0386`

Next steps were asusual , open a python server and wget in target machine.

The README in repo mentions ,2 shell interaction is needed to escalte privilege . 

Open another shell through SSH and run the two commands in exploit present directory (Refer the README file)

Finally Two flags 
--8b725c77196768ef0dcea34746aba58c
--c66d33415921887308bbfdc92d341c82

--------------------------------------------THE END---------------------------------------------------------------










