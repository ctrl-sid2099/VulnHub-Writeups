# 📚 DC-2 Machine WriteUp

## ⚙️ Machine Info
  - Platform: VulnHub
  - Name: DC-2
  - Link: https://www.vulnhub.com/entry/dc-2,311/
  - Level: Easy

## 📄 Official Description

There are five flags in total, but the ultimate goal is to find and read the flag in root's home directory. 

## ✅ Find The Target IP, Open Ports, Services And Their Version.

Scan all IP addresses on the local subnet

     sudo arp-scan -l
     
Do port detection and service scanning

     nmap -p- -sV Target-IP

<img width="812" height="446" alt="1 Scanning-IP" src="https://github.com/user-attachments/assets/9b341031-6a47-49ff-b2a3-857ef8c6960b" />

### 👇 Result:
 
 - Port: 80 - Apache httpd 2.4.10 Debian
 - Port: 7744 - OpenSSH 6.7p1

## 🔥 Flag 1 in the wordpress site

<img width="1011" height="674" alt="2 Flag1" src="https://github.com/user-attachments/assets/86f29f32-2190-4ba0-bc98-f7a8be95e5c4" />

## 💡 Using cewl to make a custom wordlist

In Terminal Type:

    cewl *Target-Site* -w wordlists.txt

<img width="639" height="193" alt="3 Cewl" src="https://github.com/user-attachments/assets/4b12b091-c164-48ed-a31e-f1b113fdd60f" />


## 📄 Using gobuster to enumrate directories

In Terminal Type:

    gobuster dir -u *Target-Site* -w path-to-wordlists

<img width="686" height="486" alt="4 Dir" src="https://github.com/user-attachments/assets/e8f2e20b-3b97-4e04-9c57-ca4fa8e1b3cc" />

## 📄 Using nmap-script to enumrate usernames

In Terminal Type:

    nmap -p 80 --script http-wordpress-users.nse *Target-IP*

<img width="836" height="428" alt="6 Enumrating-Usernames" src="https://github.com/user-attachments/assets/9e53b11c-1002-49aa-9764-cd44ce4a3107" />

### 👇 Result 

Usernames Found
 
 - admin
 - tom
 - jerry

## 🔥 Using Burpsuite intruder to brute force the login panel

1) set attack-type to **Cluster Bomb Attack**

2) set wordlists to the one you made by cewl

3) Start Attack

<img width="1598" height="713" alt="8 Brute-FOrce-Result" src="https://github.com/user-attachments/assets/d486747f-bc32-4d0e-a8ec-0c8f2cad104c" />

### 👇 Result

If we sort out the Result by Status code we can see two Results with HTTP 302 (Redirect) code.

 - User tom - credentials found
 - User jerry - credentials found
 - User admin - credentials not found

## 🔥 Flag 2 in wordpress dashboard

Login in wordpress panel using user jerry credentials.

<img width="1599" height="747" alt="9 Jerry" src="https://github.com/user-attachments/assets/0959641c-3c42-4e31-8d57-3c8d7742699e" />

### 👇 Result

Flag 2 found

<img width="941" height="493" alt="10 Flag2-content" src="https://github.com/user-attachments/assets/f2f14618-430d-4865-875c-c0ad22668d48" />

## 💡 Using SSH to Remote access the Target machine

Login using credentials of jerry and tom

<img width="632" height="458" alt="11 SSH" src="https://github.com/user-attachments/assets/1ce6df3a-1bc7-46d0-b918-37316e41a3a3" />

## 🔥 Flag 3 in tom machine

<img width="272" height="149" alt="12 ENum" src="https://github.com/user-attachments/assets/46c373c9-9724-4817-9431-d4599defd46e" />

## 👇 Result 

<img width="808" height="373" alt="13 Flag3" src="https://github.com/user-attachments/assets/f4cee150-5bc2-43d3-b4c7-5af7839f71e2" />

## ⚙️ Spawning a Better Shell

In Terminal Type:

    vi
    :set shell=/bin/bash

  <img width="204" height="69" alt="14" src="https://github.com/user-attachments/assets/82920297-cd31-4ee1-ac05-50abdf1fe256" />

    :shell

  <img width="115" height="58" alt="15" src="https://github.com/user-attachments/assets/389036d5-b937-4ac4-ac5c-86f7d038dcdd" />

### 👇 Result

 - commands still not found (Must be because of restriction)

Using echo $PATH command to check the binaries path.

In Terminal Type:

    echo $Path

👉 Result: /home/tom/usr/bin

Use export command to change the binaries path to a better one.

In Terminal Type:

    export PATH=/bin:/usr/bin:/usr/local/bin:$PATH

<img width="842" height="232" alt="16 Better-Shell" src="https://github.com/user-attachments/assets/1ec5f929-5ff1-4612-a304-d078a7acd3d9" />

👉 Result = cat, whoami, sudo, etc. commands working.

## ✅ Changing the user to jerry

In Terminal Type:

    su jerry

<img width="342" height="33" alt="17 Jerry" src="https://github.com/user-attachments/assets/4b9dd388-8eae-4497-bb4b-24b10f006421" />

## 🔥 Flag 4 in /home/jerry directory

<img width="667" height="260" alt="18 Flag4 txt" src="https://github.com/user-attachments/assets/dca805c6-37d5-4736-9f62-008c07f95c16" />

Also check for the files that the user jerry is allowed to run/execute as elivated(sudo) privilege.

Command:

    sudo -l

## 👇 Result 

<img width="873" height="131" alt="19 git" src="https://github.com/user-attachments/assets/a65b2956-dd14-4475-9609-e2323e4daae7" />

## 🔥 Using git for privilege escalation

In Terminal Type:

    sudo git help config
    !sh
    whoami

## 👇 Result

<img width="426" height="53" alt="20 root-access" src="https://github.com/user-attachments/assets/9982655a-7beb-4b9d-8b47-857ca09fc31c" />

## 🔥 Final flag in /root directory

<img width="487" height="329" alt="21 final-flag" src="https://github.com/user-attachments/assets/27c5949b-fe20-4f64-a040-d04f30983ad7" />





